# Acu-Write-Up

A comprehensive collection of write-ups for Acunetix web vulnerability scans, detailing the identification, exploitation, and mitigation of various web security issues. This repository serves as a resource for security researchers and developers to understand and address vulnerabilities found by Acunetix.

Summary Title: SQLi Vulnerability that allows anyone to login as ‘admin’, leading to further exploit that reveals sensitive server-related information.

Target: testasp.vulnweb.com
Technical severity: Very High (based on Zofixer rating)

Location of Vulnerability: http://testphp.vulnweb.com/login.php

Description & Solution: Login page is susceptible to SQL Injection, allowing any user to login as admin. Using SQLMap and BurpSuite, I was able to identify the string ‘ or 1=1 or ‘’=’ can be injected into the tbUsername field to login as ‘admin’.

![image](https://github.com/user-attachments/assets/33624fab-fb24-4544-930c-70f28318b59d)

Once logged in, I was able to use the admin SessionID to continue test with SQLMap.

This revealed that there were more exploitable fields with payloads on the site, such as time-based Blind and Microsoft SQL Server, Stacked Queries.

![image](https://github.com/user-attachments/assets/e8e5d793-0ac8-4787-b064-4300b35d9e8f)

![image](https://github.com/user-attachments/assets/7f8ef7b4-7286-4f2a-9ec9-0e527fdba454)

Using the Microsoft SQL Server, Stacked Queries payload discovered by SQLMap, I was able to get a list of six databases in testasp.vulnweb.com:

Remediation/Mitigations:

One recommendation for mitigating this type of attack would be to use prepared statements and utilize parameterized queries, ensuring users/attackers are not able to manipulate the intent behind query.

Summary Title: Default credentials in place that allow any user to sign in as admin, without requiring password.

Target: testhtml5.vulnweb.com
Technical severity: Very High (based on Zofixer rating)

Location of Vulnerability: http://testhtml5.vulnweb.com/#/popular

Description & Solution: Site in question has a login page on the home screen. Clicking on it allows a any user to sign in without requiring password.

![image](https://github.com/user-attachments/assets/9b8bc3d7-d957-472b-b898-b373d05620e8)

****![image](https://github.com/user-attachments/assets/77b309ad-975e-4af1-94ad-b21ddb0cc674)

Once logged in as admin, the attacker can then exploit further vulnerabilities to harvest sensitive information, deface the site, or deploy phishing campaigns.

Remediation/Mitigations:

Remediations for this vulnerability are to change or disable the default credentials immediately and to make sure the passwords are long and unique. Reviewing if a system is using default credentials can be done through various tools, some free and others paid.

Summary Title: Local File Inclusion Vulnerability that allows any user to gain access to sensitive files such as the /etc/passwd file.

Target: testphp.vulnweb.com
Technical severity: Very High

Location of Vulnerability: http://testphpvulnweb.com/showimage.php?file=./pictures/1.jpg

Description & Solution: I first reviewed the website and was paying close attention to the URL, looking for the potential of Local File Inclusion (LFI) vulernabilities. I found that URLs to images within the site used scripts that accepted file names as parameters, such as with http://testphpvulnweb.com/showimage.php?file=./pictures/1.jpg

I then followed regular web traffic using Wireshark to identify the OS (Ubuntu), which told me that I could potentially gain access to the .. / .. /etc/psswd file through Local File Inclusion.

![Uploading image.png…]()

Next, I was able to crawl the website using getallurls and then filtered the results to exclude redirects to show less results.
