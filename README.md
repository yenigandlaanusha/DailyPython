| Key | Value |
| --- | --- |
| Method - Endpoint | GET - /certmgmt/status |
| Authentication Mechanism | None |
| Request Headers | 'Content-type': 'application/json',  <br> 'Accept': 'application/json' |
| Request Params | None |
| Response Headers | As per table |
| Request Body | None |
| Response Body | { <br>&nbsp; "url" : "certmgmt/status", <br> "modules": [<br>&nbsp; { <br>&nbsp;&nbsp; "os" : "win" <br>&nbsp;&nbsp; "service_name":&lt;CG&gt;, <br>&nbsp;&nbsp; "identifier":&lt;cg-cert-discovery-15.0&gt;, <br>&nbsp;&nbsp; "type":&lt;cert-discovery&gt; <br>&nbsp; &nbsp;&nbsp; "script_name":&lt;cg-cert-discovery-15.0.zip&gt;} <br>&nbsp; ] <br> } |
| Success Response Status | 200 |
