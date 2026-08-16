<img width="1445" height="169" alt="Screenshot 2026-08-15 133126" src="https://github.com/user-attachments/assets/6b1bc80c-93ee-4822-acf4-2072f5d298aa" />


# 🔷 **Task 2 : Manual discovery-Common files**

### 1. What is the directory in robots.txt that isn't allowed to be viewed by web crawlers?

<img width="210" height="73" alt="62a7685ca6e7ce005d3f3afe-1777890954492" src="https://github.com/user-attachments/assets/0c921373-ae9b-41c3-8445-2abcc565f948" />

### Answer- `/staff-portal`

---

### 2. What is the path of the secret area found in sitemap.xml?

<img width="1920" height="922" alt="sitemap" src="https://github.com/user-attachments/assets/0085bc79-e5f3-4e2d-bd26-a3f2f2a364ae" />

### **See the bottom `loc` tag**  
### Answer- `/s3cr3t-area`

---

# 🔷 **Task 3 : Manual discovery-Headers & Framework stack**

### In your terminal type `curl http://MACHINE_IP -v`
<img width="1920" height="922" alt="Xflag" src="https://github.com/user-attachments/assets/42a14b47-c9a1-4e66-9925-58b4c4e15bdb" />  

---
### **Then scroll to the bottom of this output and you can find the framework link**
<img width="1920" height="922" alt="framework-link" src="https://github.com/user-attachments/assets/8f912260-72de-43f9-8af1-7ea139cab7a8" />

---

### After clicking the link, the site will open , then click on `Documentation` 

<img width="1920" height="922" alt="login-link" src="https://github.com/user-attachments/assets/6f28478a-f5db-481f-b843-93969f7f8768" />

### You can see the link of login , go through it by `http://MACHINE_IP/thm-framework-login`
<img width="1920" height="922" alt="login-page" src="https://github.com/user-attachments/assets/6c18411e-213e-4df5-bd8e-8ae6f25d8d9f" />  

### Enter the credentials and you'll get the flag

---
# 🔷 **Task 4 : OSINT - Search engines & web tools**

Q. What Google dork operator limits results to a specific site?    
Answer- `site:`

Q. What online tool and browser extension identifies what technologies a website is running?  
Answer- Wappalyzer

# 🔷 **Task 5 : OSINT - Repositories & archives**  

Q. What is the website address for the Wayback Machine?  
Answer- `https://web.archive.org/`

Q. What URL format do Amazon S3 buckets end in? (Answer starts with a .)  
Answer- `.s3.amazonaws.com`


# 🔷 **Task 6 : Automated discovery - Gobuster fundamentals** 

Q. What is the name of the directory beginning with /mo that was discovered?  
Answer- /monthly

Q. What is the name of the log file that was discovered?  
Answer- /development.log


# 🔷 **Task 7 : Automated discovery - Subdomains & virtual hosts** 

Q. Apart from dns and `-w`, which shorthand flag is required for dns mode?  
Answer- `-d`

Q. How many virtual hosts on acmeitsupport.thm respond with status code 200?  
```bash
gobuster vhost -u "http://MACHINE_IP" --domain acmeitsupport.thm -w /usr/share/wordlists/SecLists/Discovery/DNS/subdomains-top1million-5000.txt --append-domain --exclude-length 250-320
```
Type the above command to get the results  

Answer- 3













