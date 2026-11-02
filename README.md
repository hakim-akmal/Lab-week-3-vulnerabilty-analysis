# Lab-week-3-vulnerabilty-analysis
<img width="1920" height="923" alt="IP discovery" src="https://github.com/user-attachments/assets/dc28ee86-076d-4dc0-b9b1-ac4a6441e260" />
To identify the target machine, an Nmap scan was performed using the -Pn option. The -Pn parameter instructs Nmap to skip host discovery and treat all hosts as online, which is useful when ICMP requests are blocked or filtered. During the scan, the host 192.168.0.141 was identified as the target machine because it exposed three significant services: FTP (21), SSH (22), and HTTP (80). The MAC address also indicated a VirtualBox virtual network adapter, confirming that it was the BlueMoon virtual machine


<img width="865" height="790" alt="VirtualBox_kali-linux-2025 3-virtualbox-amd64_02_11_2026_10_05_02" src="https://github.com/user-attachments/assets/4fcd1077-66ac-4744-9491-51af5d0eeca0" />
To identify hidden files and directories within the web application, Gobuster was used with the common wordlist provided by Kali Linux. The scan revealed several resources, including index.html and server-status. Although no sensitive information was immediately disclosed, the enumeration process helped identify accessible web resources and guided further investigation into hidden directories.


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


<img width="1920" height="923" alt="list information" src="https://github.com/user-attachments/assets/f5b6a77e-3259-4dcd-b7b9-001e2f652128" />
After downloading and examining the files from the FTP server, the file information.txt revealed the username robin and indicated that a password list had been provided due to weak password practices. The second file, p_lists.txt, contained multiple potential passwords. This information was later used during the credential attack phase to obtain valid SSH credentials for the target system.


<img width="1920" height="923" alt="got the password" src="https://github.com/user-attachments/assets/cb1c06d2-234d-45af-a7ed-904d8213f067" />
After identifying the username robin and obtaining a list of potential passwords from the FTP server, a password attack was performed against the SSH service using Hydra. The tool successfully identified the valid SSH credentials:

Username: robin
Password: k4rv3ndh4nh4ck3r

These credentials provided initial access to the target system through the SSH service.


<img width="1920" height="923" alt="successfull login robin" src="https://github.com/user-attachments/assets/47582248-77b8-40ff-a6d2-2789db0724e4" />
Using the credentials obtained from the Hydra attack, an SSH connection was successfully established to the target machine. After authentication, access was gained as the user robin. Basic enumeration commands such as whoami, pwd, and ls -la were executed to identify accessible files and directories. Further privilege enumeration using sudo -l revealed that the user was allowed to execute the script feedback.sh without requiring a password, indicating a potential privilege escalation vector.


<img width="1920" height="923" alt="successfull" src="https://github.com/user-attachments/assets/2cdf5c7d-18c8-41db-956c-1f8daf78bb30" />
After gaining SSH access as the user robin, privilege escalation enumeration revealed that a script, feedback.sh, could be executed with elevated privileges. Further analysis of the environment showed that the user was a member of the docker group. This misconfiguration was exploited by launching a privileged Docker container and mounting the host filesystem. By chrooting into the mounted filesystem, root-level access to the target machine was obtained.

Once root privileges were achieved, the root.txt file was accessed and the root flag was successfully retrieved, confirming full compromise of the target system.
