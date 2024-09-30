# Acu-Write-Up

Summary Title: SQL Injection Vulnerability Grants Unauthorized Admin Access, Potentially Exposing Sensitive Server Data.
Target: testasp.vulnweb.com
Technical Severity: Very High (according to Zofixer rating).

Vulnerability Location: (http://testphp.vulnweb.com/login.php)

Description & Mitigation: The login page is vulnerable to SQL Injection, enabling any user to gain admin access. By utilizing SQLMap and BurpSuite, I discovered that injecting the string ‘ or 1=1 or ‘’=’ into the tbUsername field permits login as the ‘admin’ user.

![image](https://github.com/user-attachments/assets/33624fab-fb24-4544-930c-70f28318b59d)

After logging in, I utilized the admin SessionID to conduct further testing with SQLMap.

This led to the discovery of additional vulnerable fields on the site, including time-based Blind SQL Injection and Stacked Queries for Microsoft SQL Server.

![image](https://github.com/user-attachments/assets/e8e5d793-0ac8-4787-b064-4300b35d9e8f)

![image](https://github.com/user-attachments/assets/7f8ef7b4-7286-4f2a-9ec9-0e527fdba454)

Using the Stacked Queries payload for Microsoft SQL Server identified by SQLMap, I was able to retrieve a list of six databases from testasp.vulnweb.com.

Remediation/Mitigation:

To mitigate this type of attack, it is recommended to implement prepared statements and use parameterized queries, which would prevent users or attackers from manipulating the intent of the queries.

Summary Title: Default Credentials Allow Any User to Log In as Admin Without a Password
Target: testhtml5.vulnweb.com
Technical Severity: Very High (according to Zofixer rating).

Vulnerability Location: http://testhtml5.vulnweb.com/#/popular

Description & Solution: The site features a login page accessible from the home screen. Users can log in without needing to provide a password.

![image](https://github.com/user-attachments/assets/9b8bc3d7-d957-472b-b898-b373d05620e8)

![image](https://github.com/user-attachments/assets/77b309ad-975e-4af1-94ad-b21ddb0cc674)

Once logged in as admin, the attacker can exploit additional vulnerabilities to access sensitive information, deface the website, or launch phishing campaigns.

Remediation/Mitigation:

To address this vulnerability, it’s crucial to change or disable default credentials immediately and ensure that passwords are both long and unique. Various tools, both free and paid, can help assess whether a system is using default credentials.

Summary Title: Local File Inclusion Vulnerability Enables Access to Sensitive Files, Such as /etc/passwd.
Target: testphp.vulnweb.com
Technical Severity: Very High

Vulnerability Location: (http://testphpvulnweb.com/showimage.php?file=./pictures/1.jpg)

Description & Solution: I examined the website closely, particularly the URL, to identify potential Local File Inclusion (LFI) vulnerabilities. I discovered that image URLs utilized scripts that accepted filenames as parameters, like the one mentioned above.

By monitoring regular web traffic with Wireshark, I determined the operating system was Ubuntu, indicating that I could potentially access the /etc/passwd file through Local File Inclusion.

![image](https://github.com/user-attachments/assets/e2152124-ac47-4e07-9312-06c8bc612f56)

![Uploading image.png…]()

Next, I crawled the website using getallurls, filtering the results to exclude redirects in order to streamline the output.
