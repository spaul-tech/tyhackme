<img width="1458" height="165" alt="rootme" src="https://github.com/user-attachments/assets/029f7941-5e32-49a9-97d5-2fbc33804f70" />

## 🔷 Task 2 - Reconnaissance
## 🔹 Q. Scan the machine, how many ports are open?

<img width="1920" height="644" alt="nmap-scan" src="https://github.com/user-attachments/assets/b5045c2b-fd68-4cea-8fc2-7938b7a0cf39" />  

## Answer- 2

---

## 🔹 Q. What version of Apache is running?  
## Answer- 2.4.41

---

## 🔹 Q. What service is running on port 22?  
## Answer- ssh

---

## 🔹 Q. What is the hidden directory?
<img width="1920" height="797" alt="gobuster-dir" src="https://github.com/user-attachments/assets/c4369e53-70a5-4c93-9c71-e9acd3ee31ab" />  

## Answer- /panel/  

---
## 🔷 Task 3 - Getting a shell
## 🔹 Q. Find a form to upload and get a reverse shell, and find the flag.

### Open the panel directory by `http://MACHINE_IP/panel` , here we have to upload a php file to get reverse shell.
### You can go to the official link [https://github.com/pentestmonkey/php-reverse-shell](https://github.com/pentestmonkey/php-reverse-shell/blob/master/php-reverse-shell.php) and copy the code and make a file with any name and give .php extension and paste the copied code. 
### In line **49** give your **tun0** ip to it and use a different port number in line **50** . Then save your file . 
### Try to upload it in the uploads page , it will give error , now rename your saved .php file extension with .php5 . Now try to upload it again , you will it get success .
### Now go to uploads page again by  `http://MACHINE_IP/uploads` and you will see the file name you used to upload will show there .

<img width="1920" height="922" alt="php-reverse-shell-file" src="https://github.com/user-attachments/assets/725ce471-76cb-482d-a7e3-73d4b305c8ae" />

### Now open your terminal and open a listener to get the shell by `nc -lvnp <port_number_mentioned_in_php5_file>` , then go to the uploads page and click the file we uploaded with .php5 extension . Then you will the shell on your terminal .
<img width="1920" height="922" alt="netcat" src="https://github.com/user-attachments/assets/41b0dd57-3dab-4876-9880-f6e9a96111fa" />

### Now we'll search for the user.txt file so do `find / -name user.txt 2>/dev/null` to find location of the file . After you found , use cat command to get your flag.
<img width="1920" height="318" alt="user txt" src="https://github.com/user-attachments/assets/0e8e047f-29d9-48d8-bda9-c729a709a862" />

---
## 🔷 Task 4 - Privilege escalation
## 🔹 Q. Search for files with SUID permission, which file is weird?

### To see files with suid permission type this command `find / -perm -u=s -type f 2>/dev/null` 
<img width="1920" height="514" alt="suid" src="https://github.com/user-attachments/assets/3c9940aa-5cad-48f7-b0e5-ee348fadc8bd" />

---
## 🔹 Q. Find a form to escalate your privileges.
### As we see there is a python file , so with this we'll escalate privileges , type the code `python2.7 -c 'import os; os.execl("/bin/sh", "sh")'`  and paste in the shell we got or you can check the official page of the code from https://gtfobins.org/gtfobins/python/  . After you run the code you will get root privileges .
### Type `find / -type f -name root.txt` to get location of the root txt file 
### Type `cat /root/root.txt` to get your flag .













