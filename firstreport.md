<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
</head>
<body>
<div class="container">
  <h1>Bug Bounty Report</h1>
  <p>Hello,</p>
  <h2>Introduction</h2>
  <p>
    I am submitting this bug bounty report regarding a security vulnerability I discovered in a bug bounty program. I wish to inform you of this vulnerability to complete my BugBountyJourney. 🐞
  </p>
  <h2>Description of the Vulnerability</h2>
  <p>
    I have discovered a "File Information Disclosure" vulnerability in a web application. This vulnerability allows an attacker to retrieve sensitive information, such as the contents of system or configuration files, which should not be publicly accessible. 🕵️‍♂️
  </p>
  <p>To reproduce the vulnerability, please follow the steps below:</p>
  <ul>
    <li>Access this address "<a href="http://example.com">http://example.com</a>" and enumerate all files that can be possibly accessed.</li>
    <li>After enumerating, it can be observed that there are some directories that are accessible by anyone.</li>
    <li>In these directories, sensitive configuration and executable files can be found.</li>
  </ul>
  <div class="risk-assessment">
    <h3>Impact</h3>
    <p>
      <strong>Risk Assessment:</strong> I rate the severity of this vulnerability as medium due to its ability to disclose sensitive and potentially confidential information. An attacker could use this information to carry out further targeted attacks or compromise system security. ⚠️
    </p>
  </div>
</div>
</body>
</html>
