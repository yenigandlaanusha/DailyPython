| Key | Value |
| --- | --- |
| Method - Endpoint | GET - /certmgmt/status |
| Authentication Mechanism | None |
| Request Headers | 'Content-type': 'application/json',  <br> 'Accept': 'application/json' |
| Request Params | detail=<true/false> (Default false) |
| Response Headers | As per table |
| Response Body | { <br>&nbsp; "url" : "certmgmt/status", <br>&nbsp; "description": "CertMgmt Service Status Snapshot", <br>&nbsp; "version": Cloud-Connect-Version, <br>&nbsp; "build-no": CertMgmnt-build-no, <br>&nbsp; "status": "IN_SERVICE", <br>&nbsp; "status_data": "message_if_service_fails", <br>&nbsp; "server_time": &lt;host.current_time.isoformat()&gt;, <br>&nbsp; "up_since": &lt;host.up_time.isoformat()&gt;, <br>&nbsp; "up_time": &lt;no_of_days/hours/mins/seconds&gt;, <br>&nbsp; "db_status" : "IN_SERVICE", <br> "managed_hosts": [ <-- for detail=true <br>&nbsp; { <br>&nbsp;&nbsp; "url" : "certmgmt/host/&lt;host.id&gt;" <br>&nbsp;&nbsp; "host_id":&lt;host_id&gt;, <br>&nbsp;&nbsp; "hostname":&lt;hostname&gt;, <br>&nbsp;&nbsp; "last_contact_time":&lt;last_contact_time&gt; <br>&nbsp;} <br>&nbsp; ] <br> } |
| Success Response Status | 200 |
