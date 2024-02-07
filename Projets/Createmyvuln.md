<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd">
<html xmlns="http://www.w3.org/1999/xhtml" xml:lang="en" lang="en">
  <head>
    <meta http-equiv="Content-Type" content="text/html; charset=utf-8" />
  </head>
  <body>
    <h1>Report on a Security Vulnerability Creation</h1>
    <p>Bouguerba Gihad</p>
    <h1>Introduction</h1>
    <p>
      <br />
    </p>
    <p>Before all this, we didn't know what to choose. To be honest, I had some ideas in mind but none were satisfying enough. From the first practical work, we thought of simply setting up a BadStore machine that would host several vulnerabilities depending on the chosen version. As in the official website documentation:</p>
    <p>
      <br/>
    </p>
    <p>
      <br />
    </p>
    <p>
      <span>
        <table border="0" cellspacing="0" cellpadding="0">
          <tr>
            <td>
![image](https://github.com/Cvrouu/cvrouu/assets/114159408/a8e52219-042c-4bc8-ab0c-50ab11245d20)
            </td>
          </tr>
        </table>
      </span>
    </p>
    <p>
      <br />
    </p>
    <p>In short, it's just a machine with vulnerabilities. But no, we thought that would be too easy, and it's a SAE, there has to be work to do. So, we thought of a well-known vulnerability in the cyber world: Server Side Request Forgery.</p>
    <h1>WHAT IS SSRF?</h1>
    <p>
      <br />
    </p>
    <p>
      <span>
        <table border="0" cellspacing="0" cellpadding="0">
          <tr>
            <td>
![image](https://github.com/Cvrouu/cvrouu/assets/114159408/0915429a-3ee0-4a84-9e20-ae11ced4b29f)
            </td>
          </tr>
        </table>
      </span>
    </p>
    <p>So according to our friends at Portswigger: Server-side request forgery (SSRF) is a web security vulnerability that allows an attacker to induce the server-side application to make HTTP requests to an arbitrary domain of the attacker's choosing.</p>
    <p>In a typical SSRF attack, the attacker can make the server establish a connection with services internal to the organization's infrastructure. In other cases, they may be able to force the server to connect to arbitrary external systems. This could lead to the leakage of sensitive data, such as authorization credentials.</p>
    <p>
      <br/>
    </p>
    <p>So, we designed a model so that it could implement</p>
    <p>this vulnerability:</p>
    <p>
      <br/>
    </p>
    <p>
      <span>
        <table border="0" cellspacing="0" cellpadding="0">
          <tr>
            <td>
![image](https://github.com/Cvrouu/cvrouu/assets/114159408/ddef200f-f19a-4cf3-aa2f-d30832fa9658)
            </td>
          </tr>
        </table>
      </span>
    </p>
    <p>
      <br />
    </p>
    <p>Following an in-depth analysis of the system's security, we identified a potential vulnerability related to a denial of service attack. It is important to note that this flaw differs somewhat from conventional SSRF attacks. But it still remains an SSRF.</p>
    <h1>Vulnerability Description</h1>
    <p>
      <br/>
    </p>
    <p>The attack in question is based on manipulating the Host field in HTTP requests. The attacker seeks to exploit this flaw by masquerading as an internal PC to gain access to a WEB server located in a DMZ at the bottom right of the model. This server allows access only from a specific IP address, which is that of serverWEB200.</p>
    <p>
      <br />
    </p>
    <h1>Attack Procedure</h1>
    <p>
      <br/>
    </p>
    <p>Step 1: We generate a forged request from our own PC, simulating belonging to the company's subnet.</p>
    <p>
      <br/>
    </p>
    <p>Step 2: By substituting the Host field of the request with a guessed IP address, we attempt to bypass the access restrictions of the WEB server in the DMZ.</p>
    <p>
      <br />
    </p>
    <p>Step 3: The WEB server in the DMZ, thinking it is dealing with a legitimate request from the internal PC, responds with an HTTP 408 code.</p>
    <p>
      <br/>
    </p>
    <h1>Meaning of HTTP 408 Code</h1>
    <p>
      <br />
    </p>
    <p>The HTTP 408 Request Timeout response status code indicates that the server would like to shut down this unused connection. For some servers, this code is sometimes sent on an idle connection without necessarily having had a request from the client.</p>
    <p>
      <br />
    </p>
    <p>A server must send the Connection header with the value close in response, as 408 implies the server decided to close the connection rather than continue to wait. It's like with Scapy in practical work actually but less annoying...</p>
    <p>So, as expected, we observe an http 408 on the server side:</p>
    <p>
      <span>
        <table border="0" cellspacing="0" cellpadding="0">
          <tr>
            <td>
![image](https://github.com/Cvrouu/cvrouu/assets/114159408/573c2f59-a5ec-456c-aa04-d0d53c55e647)
            </td>
          </tr>
        </table>
      </span>
    </p>
    <p>
      <br />
    </p>
    <h1>Potential Consequences</h1>
    <p>
      <br/>
    </p>
    <p>Successful manipulation of the Host field can result in an HTTP 408 response from the remote server in the DMZ. By repeating this attack, it is possible to cause a denial of service and potentially crash the remote server.</p>
    <p>
      <br/>
    </p>
    <h1>Recommended Solution</h1>
    <p>
      <br/>
    </p>
    <p>Strict Request Filtering: It is recommended to implement strict filters for incoming requests, especially for the Host field, to limit the possibility of manipulation.</p>
    <p>
      <br />
    </p>
    <p>Enhanced Authentication: It is essential to implement robust authentication mechanisms to ensure that only legitimate users have access to sensitive resources.</p>
    <p>
      <br />
    </p>
    <p>Continuous Monitoring: Setting up a monitoring system can detect and react quickly to abnormal activities, such as repeated attack attempts.</p>
    <p>
      <br />
    </p>
    <h1>Conclusion</h1>
    <p>In typical cases, SSRF attacks often exploit trust relationships to escalate an attack from the vulnerable application and perform unauthorized actions. These trust relationships can exist either with the server or with other backend systems within the same organization. it presents a significant risk of denial of service. Implementing strong preventive measures is essential to mitigate this potential threat. All this to say that our friend can become our worst enemy in an instant.</p>
  </body>
</html>
