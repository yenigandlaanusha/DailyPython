# REST API for Certificate Manager Service

## Table of Content
- [Base URL](#BaseURL)
- [Container Service](#ContainerService)
    - [Container Service Status](#ContainerServiceStatus)
- [Client Scripts](#ClientScripts)
    - [Client Scripts](#ClientScripts)
- [Metrics](#Metrics)
    - [Metrics Details](#MetricsDetails)
- [Host Management](#HostManagement)
    - [Onboarding Host](#OnboardingHost)
    - [Offboarding Host](#OffboardingHost)
    - [Fetch Host Details](#FetchHostDetails)
- [Audit](#Audit)
    - [Audit Details For Host](#AuditDetailsForHost)
- [Email List Management](#EmailListManagement)
    - [Create Email List](#CreateEmailList)
    - [Fetch All Email List](#FetchAllEmailList)
    - [Fetch Email List](#FetchEmailList)
    - [Update Email List](#UpdateEmailList)
    - [Delete Email List](#DeleteEmailList)
- [Certificate Expiry Alert Configuration](#CertificateExpiryAlertConfiguration)
    - [Fetch All Certificate Expiry Alert Configurations](#FetchAllCertExpiryAlertConfig)
    - [Fetch Certificate Expiry Alert Configuration](#FetchCertExpiryAlertConfig)
    - [Update Certificate Alert Configuration](#UpdateCertAlertConfig)
- [Host Unavailability Alert Configuration](#HostUnavailabilityAlertConfiguration)
    - [Fetch Host Unavailability Alert Configuration](#FetchHostUnavailabilityAlertConfig)
    - [Update Host Unavailability Alert Configuration](#UpdateHostUnavailabilityAlertConfig)
- [Email Alerts - Reporting](#EmailAlerts-Reporting)
    - [Get Generated Certificate Email Alerts](#GetGeneratedCertEmailAlerts)
    - [Get Generated Node Unavailability Email Alerts](#GetGeneratedNodeUnavailabilityEmailAlerts)
    - [Get Generated Node Specific Email Alerts](#GetGeneratedNodeSpecificEmailAlerts)
- [Certificate Management](#CertificateManagement)
    - [Send Certificate Details By Agent](#SendCertificateDetailsByAgent)
- [Response Header](#ResponseHeader)

### BaseURL
- https://&lt;cconecthostname&gt;:&lt;8445&gt;

## ContainerService
### ContainerServiceStatus
| Key | Value |
| --- | --- |
| Method - Endpoint | GET - /certmgmt/status |
| Authentication Mechanism | None |
| Request Headers | 'Content-type': 'application/json',  <br> 'Accept': 'application/json' |
| Request Params | detail=<true/false> (Default false) |
| Response Headers | As per table |
| Response Body | { <br>&nbsp; "url" : "certmgmt/status", <br>&nbsp; "description": "CertMgmt Service Status Snapshot", <br>&nbsp; "version": Cloud-Connect-Version, <br>&nbsp; "build-no": CertMgmnt-build-no, <br>&nbsp; "status": "IN_SERVICE", <br>&nbsp; "status_data": "message_if_service_fails", <br>&nbsp; "server_time": &lt;node.current_time.isoformat()&gt;, <br>&nbsp; "up_since": &lt;node.up_time.isoformat()&gt;, <br>&nbsp; "up_time": &lt;no_of_days/hours/mins/seconds&gt;, <br>&nbsp; "db_status" : "IN_SERVICE", <br> "cluster": { <br>&nbsp; "nodes": [ <br>&nbsp; { <br>&nbsp;&nbsp; "primary": true, <br>&nbsp;&nbsp; "host_fqdn": "cconnectsub106.stooges.icm" <br>&nbsp; }, <br>&nbsp; { <br>&nbsp;&nbsp; "primary": false, <br>&nbsp;&nbsp; "host_fqdn": "cconnectpub106.stooges.icm" <br>&nbsp; } <br>&nbsp; ] <br> }, <br> "onboard_count": &lt;active_onboard_nodes&gt;, <br> "managed_hosts": [ <-- for detail=true <br>&nbsp; { <br>&nbsp;&nbsp; "url" : "certmgmt/host/&lt;node.id&gt;" <br>&nbsp;&nbsp; "host_id":&lt;host_id&gt;, <br>&nbsp;&nbsp; "hostname":&lt;hostname&gt;, <br>&nbsp;&nbsp; "last_contact_time":&lt;last_contact_time&gt; <br>&nbsp;} <br>&nbsp; ] <br> } |
| Success Response Status | 200 |

## ClientScripts
### ClientScripts
| Key | Value |
| --- | --- |
| Method - Endpoint | GET - /certmgmt/modules |
| Authentication Mechanism | None |
| Request Headers | 'Content-type': 'application/json',  <br> 'Accept': 'application/json' |
| Request Params | OS="win/vos", <br> Component="component-name" <br> Version="component-version" <br>module="discovery/certfinder/upload" |
| Response Headers | Content-Disposition: attachment; filename="script.zip" <br> Content-Type : "application/octet-stream" <br> As per table |
| Response Body | NA |
| Success Response Status | 204 |
| Notes | module=discovery needs only OS parameter <br> module=certfinder/upload expects component-name and component-version to be provided along with the OS parameter. |

## Metrics
### MetricsDetails
| Key | Value |
| --- | --- |
| Method - Endpoint | GET - /certmgmt/metrics |
| Authentication Mechanism | Basic Auth with CConnect Username - Password |
| Request Headers | 'Content-type': 'application/json',  <br> 'Accept': 'application/json' |
| Response Headers | As per table |
| Response Body | { <br>&nbsp; "&lt;metric_id&gt;": <br>&nbsp; { <br>&nbsp;&nbsp;&nbsp; "desc": &lt;metric desc&gt;, <br>&nbsp;&nbsp;&nbsp; "unit": &lt;ms/datetime&gt;, <br>&nbsp;&nbsp;&nbsp; "value": &lt;value&gt;, <br>&nbsp;&nbsp;&nbsp; "statistics": &lt;COUNT/VALUE&gt; <br>&nbsp; } <br> } |
| Success Response Status | 200 |

## HostManagement
### OnboardingHost
| Key | Value |
| --- | --- |
| Method - Endpoint | POST - /certmgmt/host |
| Authentication Mechanism | Basic Auth with CConnect Username - Password |
| Request Headers | 'Content-type': 'application/json',  <br> 'Accept': 'application/json' |
| Request Body | { <br> "hostname": "&lt;node.hostname&gt;", <br> "server_type": "&lt;node.server_type&gt;" <br> } |
| Response Headers | As per table |
| Response Body | { <br>&nbsp; "url" : "certmgmt/host/&lt;node.host_id&gt;", <br>&nbsp; "id": "&lt;node.host_id&gt;", <br>&nbsp; "hostname": "&lt;node.hostname&gt;", <br>&nbsp; "server_type": "&lt;node.server_type&gt;", <br>&nbsp; "onboarding_time": "&lt;node.added_time.isoformat()&gt;", <br>&nbsp; "secret_key": "&lt;node.secret_key&gt;", <br>&nbsp; "old_secret_key": "&lt;node.old_secret_key&gt;", <br>&nbsp; "secret_update_time": "&lt;node.secret_update_time.isoformat()&gt;", <br>&nbsp; "last_contact_time": "&lt;node.last_contact_time.isoformat()&gt;" <br> } |
| Success Response Status | 201 (created), 208 (already exist) |

### OffboardingHost
| Key | Value |
| --- | --- |
| Method - Endpoint | DELETE - /certmgmt/host/&lt;host_id&gt; |
| Authentication Mechanism | Basic Auth with CConnect Username - Password |
| Request Headers | 'Content-type': 'application/json',  <br> 'Accept': 'application/json' |
| Response Headers | As per table |
| Response Body | NA |
| Success Response Status | 204 |

### FetchHostDetails
| Key | Value |
| --- | --- |
| Method - Endpoint | GET - /certmgmt/host/&lt;host_id&gt; |
| Authentication Mechanism | Basic Auth with CConnect Username - Password |
| Request Headers | 'Content-type': 'application/json',  <br> 'Accept': 'application/json' |
| Response Headers | As per table |
| Response Body | { <br>&nbsp; "url" : "certmgmt/host/&lt;node.host_id&gt;", <br>&nbsp; "id": "&lt;node.host_id&gt;", <br>&nbsp; "hostname": "&lt;node.hostname&gt;", <br>&nbsp; "server_type": "&lt;node.server_type&gt;", <br>&nbsp; "onboarding_time": "&lt;node.added_time.isoformat()&gt;", <br>&nbsp; "secret_key": "&lt;node.secret_key&gt;", <br>&nbsp; "old_secret_key": "&lt;node.old_secret_key&gt;", <br>&nbsp; "secret_update_time": "&lt;node.secret_update_time.isoformat()&gt;", <br>&nbsp; "last_contact_time": "&lt;node.last_contact_time.isoformat()&gt;" <br> } |
| Success Response Status | 200 |

## Audit
### AuditDetailsForHost
| Key | Value |
| --- | --- |
| Method - Endpoint | GET - /certmgmt/host/&lt;host_id&gt;/auditlogs |
| Authentication Mechanism | Basic Auth with CConnect Username - Password |
| Request Headers | 'Content-type': 'application/json',  <br> 'Accept': 'application/json' |
| Request Params | from=&lt;YYYY-MM-DD&gt;<br>to=&lt;YYYY-MM-DD&gt; |
| Response Headers | As per table |
| Response Body | { <br>&nbsp; "url": "certmgmt/host/&lt;node.host_id&gt;/auditlogs", <br>&nbsp; "audit_data" : [ <br>&nbsp; &nbsp; { <br>&nbsp; &nbsp; &nbsp; &nbsp; "id": &lt;audit.audit_id&gt;, <br>&nbsp; &nbsp; &nbsp; &nbsp; "datetime": &lt;audit.datetime.isoformat()&gt;, <br>&nbsp; &nbsp; &nbsp; &nbsp; "host_id": &lt;audit.host_id&gt;, <br>&nbsp; &nbsp; &nbsp; &nbsp; "activity": &lt;audit.activity&gt;, <br>&nbsp; &nbsp; &nbsp; &nbsp; "details": &lt;audit.details&gt; <br>&nbsp; &nbsp; } <br>&nbsp; ] <br> } |
| Success Response Status | 200 |

## EmailListManagement
### CreateEmailList
| Key | Value |
| --- | --- |
| Method - Endpoint | POST - /certmgmt/email_lists |
| Authentication Mechanism | Basic Auth with CConnect Username - Password |
| Request Headers | 'Content-type': 'application/json',  <br> 'Accept': 'application/json' |
| Request Body | {<br>&nbsp; "email_list_name": &lt;email_list_name&gt;, <br>&nbsp; "to": &lt;[{"email1@xyz.com", "email2@xyz.com"}]&gt;, <br>&nbsp; "cc": &lt;[{"email1@xyz.com", "email2@xyz.com"}]&gt; <br>} |
| Response Headers | As per table |
| Response Body | {<br>&nbsp; "url": "certmgmt/email_list/&lt;email_list.email_list_id&gt;", <br>&nbsp; "email_list_id": &lt;email_list_id&gt;, <br>&nbsp; "to": &lt;[{"email1@xyz.com", "email2@xyz.com"}]&gt;, <br>&nbsp; "cc": &lt;[{"email1@xyz.com", "email2@xyz.com"}]&gt; <br>} |
| Success Response Status | 201 |

### FetchAllEmailList
| Key | Value |
| --- | --- |
| Method - Endpoint | GET - /certmgmt/email_lists |
| Authentication Mechanism | Basic Auth with CConnect Username - Password |
| Request Headers | 'Content-type': 'application/json',  <br> 'Accept': 'application/json' |
| Response Headers | As per table |
| Response Body | {<br>&nbsp; "url": "certmgmt/email_list/&lt;email_list.email_list_id&gt;", <br>&nbsp; "email_list_id": &lt;email_list_id&gt;, <br>&nbsp; "to": &lt;[{"email1@xyz.com", "email2@xyz.com"}]&gt;, <br>&nbsp; "cc": &lt;[{"email1@xyz.com", "email2@xyz.com"}]&gt; <br>} |
| Success Response Status | 200 |

### FetchEmailList
| Key | Value |
| --- | --- |
| Method - Endpoint | GET - /certmgmt/email_lists/&lt;email_list_id&gt; |
| Authentication Mechanism | Basic Auth with CConnect Username - Password |
| Request Headers | 'Content-type': 'application/json',  <br> 'Accept': 'application/json' |
| Response Headers | As per table |
| Response Body | {<br>&nbsp; "url": "certmgmt/email_list/&lt;email_list.email_list_id&gt;", <br>&nbsp; "email_list_id": &lt;email_list_id&gt;, <br>&nbsp; "to": &lt;[{"email1@xyz.com", "email2@xyz.com"}]&gt;, <br>&nbsp; "cc": &lt;[{"email1@xyz.com", "email2@xyz.com"}]&gt; <br>} |
| Success Response Status | 200 |

### UpdateEmailList
| Key | Value |
| --- | --- |
| Method - Endpoint | PUT - /certmgmt/email_lists/&lt;email_list_id&gt; |
| Authentication Mechanism | Basic Auth with CConnect Username - Password |
| Request Headers | 'Content-type': 'application/json',  <br> 'Accept': 'application/json' |
| Response Body | {<br>&nbsp; "url": "certmgmt/email_list/&lt;email_list.email_list_id&gt;", <br>&nbsp; "email_list_id": &lt;email_list_id&gt;, <br>&nbsp; "to": &lt;[{"email1@xyz.com", "email2@xyz.com"}]&gt;, <br>&nbsp; "cc": &lt;[{"email1@xyz.com", "email2@xyz.com"}]&gt; ← Empty list allowed <br>} |
| Response Headers | As per table |
| Response Body | NA |
| Success Response Status | 204 |

### DeleteEmailList
| Key | Value |
| --- | --- |
| Method - Endpoint | DELETE - /certmgmt/email_lists/&lt;email_list_id&gt; |
| Authentication Mechanism | Basic Auth with CConnect Username - Password |
| Request Headers | 'Content-type': 'application/json',  <br> 'Accept': 'application/json' |
| Response Headers | As per table |
| Response Body | { <br>&nbsp; "affected_criteria": [<br>&nbsp; { "url": "certmgmt/node_criteria" }, <br>&nbsp; { "url": "certmgmt/cert_criteria/INFO" }, <br>&nbsp; { "url": "certmgmt/cert_criteria/WARN" }... <br>] <br> } |
| Success Response Status | 200 |

## CertificateExpiryAlertConfiguration
### FetchAllCertExpiryAlertConfig
| Key | Value |
| --- | --- |
| Method - Endpoint | GET - /certmgmt/alert_configs |
| Authentication Mechanism | Basic Auth with CConnect Username - Password |
| Request Headers | 'Content-type': 'application/json',  <br> 'Accept': 'application/json' |
| Response Headers | As per table |
| Response Body | { <br>&nbsp; "&lt;level&gt;": <br>{ <br>&nbsp; "enabled": &lt;true/false&gt; <br>&nbsp; "email_list_ids": &lt;[email_list_id1, email_list_id2]&gt; <br>&nbsp; "remaining_days": &lt;remaining_days&gt;<br>}, <br>&nbsp; "&lt;level&gt;": <br>{ <br>&nbsp; "enabled": &lt;true/false&gt; <br>&nbsp; "email_list_ids": &lt;[email_list_id1, email_list_id2]&gt; <br>&nbsp; "remaining_days": &lt;remaining_days&gt;<br>}<br>} |
| Success Response Status | 200 |

### FetchCertExpiryAlertConfig
| Key | Value |
| --- | --- |
| Method - Endpoint | GET - /certmgmt/alert_configs/&lt;INFO/WARN/HIGH/ERROR&gt; |
| Authentication Mechanism | Basic Auth with CConnect Username - Password |
| Request Headers | 'Content-type': 'application/json',  <br> 'Accept': 'application/json' |
| Response Headers | As per table |
| Request Body | { <br>&nbsp; "enabled": &lt;true/false&gt; <br>&nbsp; "email_list_ids": &lt;[email_list_id1, email_list_id2]&gt; <br>&nbsp; "remaining_days": &lt;remaining_days&gt; <br>} |
| Success Response Status | 200 |

### UpdateCertAlertConfig
| Key | Value |
| --- | --- |
| Method - Endpoint | PUT - /certmgmt/alert_configs/&lt;INFO/WARN/HIGH/ERROR&gt; |
| Authentication Mechanism | Basic Auth with CConnect Username - Password |
| Request Headers | 'Content-type': 'application/json',  <br> 'Accept': 'application/json' |
| Request Body | { <br>&nbsp; "enabled": &lt;true/false&gt; <br>&nbsp; "email_list_ids": &lt;[email_list_id1, email_list_id2]&gt; <br>&nbsp; "remaining_days": &lt;remaining_days&gt; <br>} |
| Response Headers | As per table |
| Response Body | NA |
| Success Response Status | 204 |

## HostUnavailabilityAlertConfiguration:
### FetchHostUnavailabilityAlertConfig
| Key | Value |
| --- | --- |
| Method - Endpoint | GET - /certmgmt/node_alert_configs |
| Authentication Mechanism | Basic Auth with CConnect Username - Password |
| Request Headers | 'Content-type': 'application/json',  <br> 'Accept': 'application/json' |
| Response Headers | As per table |
| Response Body | { <br>&nbsp; "enabled": &lt;true/false&gt;, <br>&nbsp; "email_list_ids": &lt;[email_list_id1, email_list_id2]&gt;, <br>&nbsp; "max_days_checkin_delay" :&lt;unavailable_days&gt; <br> } |
| Success Response Status | 200 |

### UpdateHostUnavailabilityAlertConfig
| Key | Value |
| --- | --- |
| Method - Endpoint | PUT - /certmgmt/node_alert_configs |
| Authentication Mechanism | Basic Auth with CConnect Username - Password |
| Request Headers | 'Content-type': 'application/json',  <br> 'Accept': 'application/json' |
| Request Body | {<br>&nbsp; "enabled": &lt;true/false&gt;, <br>&nbsp; "email_list_ids": &lt;[email_list_id1, email_list_id2]&gt;, <br>&nbsp; "max_days_checkin_delay": &lt;unavailable_days&gt; <br>} |
| Response Headers | As per table |
| Response Body | NA |
| Success Response Status | 204 |

## EmailAlerts-Reporting
### GetGeneratedCertEmailAlerts
| Key | Value |
| --- | --- |
| Method - Endpoint | GET - /certmgmt/alert_configs/&lt;INFO/WARN/HIGH/ERROR&gt;/alerts |
| Authentication Mechanism | Basic Auth with CConnect Username - Password |
| Request Headers | 'Content-type': 'application/json',  <br> 'Accept': 'application/json' |
| Request Param | from_dt = &lt;YYYYMMDD&gt;<br>to_dt = &lt;YYYYMMDD&gt; |
| Response Headers | As per table |
| Response Body | {<br>&nbsp;&nbsp;"alerts": [<br>&nbsp;&nbsp;&nbsp;&nbsp;{<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"datetime": "&lt;datetime.isoformat()&gt;",<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"host_id": "&lt;host_id&gt;",<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"details": {<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"title": "&lt;Email Subject&gt;",<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"key1": "&lt;value1&gt;", <!-- for example, "last_contact_time" --> <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"key2": "&lt;value2&gt;"<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;// Add more key-value pairs as needed<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;}<br>&nbsp;&nbsp;&nbsp;&nbsp;}<br>&nbsp;&nbsp;]<br>} |
| Success Response Status | 200 |

### GetGeneratedNodeUnavailabilityEmailAlerts
| Key | Value |
| --- | --- |
| Method - Endpoint | GET - /certmgmt/node_alert_configs/alerts |
| Authentication Mechanism | Basic Auth with CConnect Username - Password |
| Request Headers | 'Content-type': 'application/json',  <br> 'Accept': 'application/json' |
| Request Param | from_dt = &lt;YYYYMMDD&gt;<br>to_dt = &lt;YYYYMMDD&gt; |
| Response Headers | As per table |
| Response Body | {<br>&nbsp;&nbsp;"alerts": [<br>&nbsp;&nbsp;&nbsp;&nbsp;{<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"datetime": "&lt;datetime.isoformat()&gt;",<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"host_id": "&lt;host_id&gt;",<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"details": {<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"title": "&lt;Email Subject&gt;",<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"key1": "&lt;value1&gt;", <!-- for example, "last_contact_time" --> <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"key2": "&lt;value2&gt;"<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;// Add more key-value pairs as needed<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;}<br>&nbsp;&nbsp;&nbsp;&nbsp;}<br>&nbsp;&nbsp;]<br>} |
| Success Response Status | 200 |

### GetGeneratedNodeSpecificEmailAlerts
| Key | Value |
| --- | --- |
| Method - Endpoint | GET - /certmgmt/host/&lt;host_id&gt;/alerts |
| Authentication Mechanism | Basic Auth with CConnect Username - Password |
| Request Headers | 'Content-type': 'application/json',  <br> 'Accept': 'application/json' |
| Request Param | from_dt = &lt;YYYYMMDD&gt;<br>to_dt = &lt;YYYYMMDD&gt;<br>type = &lt;certificate/node&gt;<br>level = &lt;INFO/WARN/HIGH/ERROR&gt; |
| Response Headers | As per table |
| Response Body | {<br> "alerts": [<br>&nbsp; {<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; "type": "&lt;certificate/node&gt;",<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"level": "&lt;INFO/WARN/HIGH/ERROR&gt;",<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; "host_id": "&lt;host_id&gt;",<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; "datetime": "&lt;datetime.isoformat()&gt;",<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; "details": {<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; "title": "&lt; "&lt;Email Subject&gt;",<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; "key1": "&lt;value1&gt;",  // Example: "cn", "serial_number", etc.<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; "key2": "&lt;value2&gt;,"  // Add more key-value pairs as needed<br>&nbsp;&nbsp; }<br>&nbsp; }<br> ]<br>} |
| Success Response Status | 200 |

## CertificateManagement
### SendCertificateDetailsByAgent
| Key | Value |
| --- | --- |
| Method - Endpoint | PUT - /certagent/host/&lt;host_id&gt;/certificates |
| Authentication Mechanism | API Key with request header - <br>'Authorization': "secret_key=&lt;secret_key&gt;" |
| Request Headers | 'Content-type': 'application/json',  <br> 'Accept': 'application/json' |
| Request Payload | [<br>&nbsp; {<br>&nbsp;&nbsp; "service_name": "&lt;service_name&gt;",<br>&nbsp;&nbsp; "cert_sn": "&lt;certificate serial number&gt;",<br>&nbsp;&nbsp; "cert_type": "&lt;Root/Intermediate/Public&gt;",<br>&nbsp;&nbsp; "cert_issuer": "&lt;Certificate Issuer&gt;",<br>&nbsp;&nbsp; "cert_subject": "&lt;Certificate Subject Name&gt;",<br>&nbsp;&nbsp; "leaf_cert_cn": "&lt;Certificate Common Name for Leaf Certificate&gt;",<br>&nbsp;&nbsp; "leaf_cert_sn": "&lt;Certificate Serial Number for Leaf Certificate&gt;",<br>&nbsp;&nbsp; "cert_cn": "&lt;Certificate Common Name&gt;",<br>&nbsp;&nbsp; "cert_sans": [&lt;list of SAN&gt;],<br>&nbsp;&nbsp; "cert_not_before": "YYYYMMDDHHMMSSZ",<br>&nbsp;&nbsp; "cert_not_after": "YYYYMMDDHHMMSSZ",<br>&nbsp;&nbsp; "cert_modified_date": "YYYYMMDDHHMMSSZ"<br>&nbsp;&nbsp; "cert_path": "&lt;path/to/cert/keystore&gt;"<br>&nbsp; }<br>] |
| Response Headers | As per table |
| Response Body | NA |
| Success Response Status | 204 |


## ResponseHeader
| Header | Value |
| --- | --- |
| X-Frame-Options | deny |
| X-Content-Type-Options | nosniff |
| Referrer-Policy | no-referrer-when-downgrade |
| Cache-Control | no-store, max-age=0 |
| Pragma | no-cache |
| Content-Security-Policy | default-src 'none'; script-src 'self'; connect-src 'self'; img-src 'self'; style-src 'self'; frame-ancestors 'self'; form-action 'self'; |
| Strict-Transport-Security | max-age=31536000 ; includeSubDomains |
