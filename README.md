# Lab-week-3-vulnerabilty-analysis
<img width="1920" height="923" alt="IP discovery" src="https://github.com/user-attachments/assets/dc28ee86-076d-4dc0-b9b1-ac4a6441e260" />
To identify the target machine, an Nmap scan was performed using the -Pn option. The -Pn parameter instructs Nmap to skip host discovery and treat all hosts as online, which is useful when ICMP requests are blocked or filtered. During the scan, the host 192.168.0.141 was identified as the target machine because it exposed three significant services: FTP (21), SSH (22), and HTTP (80). The MAC address also indicated a VirtualBox virtual network adapter, confirming that it was the BlueMoon virtual machine
<img width="1920" height="923" alt="Web Enumeration" src="https://github.com/user-attachments/assets/d9217539-a253-492e-a7cf-249cb742ab56" />
After identifying port 80 as an active HTTP service, the target website was accessed through a web browser. The homepage displayed a welcome message, "Are You Ready To Play With Me.....!", indicating that the web application may contain hidden content or clues. This page served as the starting point for further web enumeration and directory discovery.
<img width="1920" height="923" alt="hidden text founded" src="https://github.com/user-attachments/assets/26471148-213e-4873-942c-784f2120636a" />
<img width="591" height="1280" alt="WhatsApp Image 2026-11-02 at 11 30 13 AM" src="https://github.com/user-attachments/assets/a8e2f113-865b-4226-bc1e-10c9b1777b33" />
During further web enumeration, a hidden directory named /hidden_text was discovered. Inside this directory, a QR code image (QR_Cod3.png) was found. The QR code was analyzed using a QR code scanner, revealing a shell script that contained FTP credentials. The extracted credentials were:

Username: userftp
Password: ftpp@ssword

These credentials were later used to successfully authenticate to the FTP service running on the target machine.
<img width="1920" height="923" alt="FTP Enumeration and Credential Discovery" src="https://github.com/user-attachments/assets/79cffa72-2fb2-449a-bf95-92e33f4bbd29" />
Using the credentials obtained from the QR code analysis, a successful connection was established to the FTP service running on the target machine. After authentication, two files, information.txt and p_lists.txt, were discovered and downloaded. These files contained valuable information that was later used to identify the SSH username and password.
