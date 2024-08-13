| Key | Value |
| --- | --- |
| Method - Endpoint | GET - certmgmt/host/<host.host_id>/modules |
| Authentication Mechanism | None |
| Request Headers | 'Content-type': 'application/json',  <br> 'Accept': 'application/json' |
| Request Params | None |
| Response Headers | As per table |
| Request Body | None |
| Response Body | { <br>&nbsp; "url" : "certmgmt/host/<host.host_id>/modules", <br> "modules": [<br>&nbsp; { <br>&nbsp;&nbsp; "os" : &lt;win/vos&gt; <br>&nbsp;&nbsp; "service_name":&lt;service_name&gt;, <br>&nbsp;&nbsp; "identifier":&lt;identifier&gt;, <br>&nbsp;&nbsp; "type":&lt;cert-discovery/cert-upload/cert-delete&gt; <br>&nbsp; &nbsp;"script_name":&lt;script_name&gt;,<br>}, <br>&nbsp;<br> ] <br> } |
| Success Response Status | 200 |
