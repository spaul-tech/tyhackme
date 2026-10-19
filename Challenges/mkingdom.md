<img width="1524" height="183" alt="mkingdom" src="https://github.com/user-attachments/assets/93846ada-f02c-4bf2-b4fc-ac8e633a0448" />

### Found only one port open at 85 , service http .
<img width="1920" height="804" alt="nmap-scan" src="https://github.com/user-attachments/assets/6c0cafc1-0d82-43a7-8a40-e55980a173a3" />

### After opening it in browser , found this page but nothing helpful .
<img width="1920" height="922" alt="found-nothing" src="https://github.com/user-attachments/assets/13be0401-7463-4899-8185-b064eec08471" />

### Then ran gobuster and found a directory `/app` .
<img width="1920" height="640" alt="gobuster" src="https://github.com/user-attachments/assets/d27469a7-8e0e-4d4f-a5af-74b191fe30de" />

###  After opening it clicked in Jump , and a page opened . Then in wappalyzer found with what it was made and found CMS 8.5.2 , which is vulnerable to php .
<img width="1920" height="922" alt="wappalyzer" src="https://github.com/user-attachments/assets/f8338c7c-955c-4b57-a3a8-dc485d095e40" />

### In the very bottom of the page found `Log in` , after clicking it entered admin:password as default credential .
---
### In https://vulners.com/hackerone/H1:768322 site found how to exploit this version .  

### Now at right side bottom click on `System & Settings` . Click on Allowed File Types .
<img width="1920" height="851" alt="allowed-files" src="https://github.com/user-attachments/assets/d1600651-ef45-4e6a-a64e-b74a6aaed329" />

### Insert php at last and Save it .
<img width="1920" height="836" alt="inserting-php" src="https://github.com/user-attachments/assets/b9745ba9-5934-4d8b-8b81-c5edaeb32fe0" />

### At right side bar at top find Files , click it at top-right you will find Upload-files , so for this we'll use https://github.com/pentestmonkey/php-reverse-shell/blob/master/php-reverse-shell.php , change ip to your tun0 . After saving it into your machine upload it and click Close .
### Now keep ready your listener , and click the URL to file link , and you'll get you shell
<img width="1920" height="848" alt="reverse-shell-link" src="https://github.com/user-attachments/assets/f540da3a-053a-4d92-b493-e349ba1b74b6" />

### Now to stabilise the shell give this command `python -c 'import pty;pty.spawn("/bin/bash")'` . I saw there are are two users toad and mario in /etc/passwd .
<img width="1920" height="869" alt="two-user" src="https://github.com/user-attachments/assets/72520646-fd51-4408-a1a9-0adbbfde6bf7" />

### In google I saw the passwords of the user of CMS are stored in a database , and got a path.
<img width="400" height="35" alt="database-path" src="https://github.com/user-attachments/assets/22d43eb2-bb6c-4e91-a8bd-502b2a872118" />

### So i searched with find and got the correct path .
<img width="1920" height="661" alt="user-pwd" src="https://github.com/user-attachments/assets/07081030-4632-4a07-b20f-2447b489e289" />

### Use `su toad` and give the password .

---
### For more enumeration I used linpeas , go to this link and download it https://github.com/carlospolop/PEASS-ng/releases/latest/download/linpeas.sh . So now I transferred this file into target machine
<img width="1920" height="165" alt="sender" src="https://github.com/user-attachments/assets/527bb5ee-d6a9-4164-85ad-056ca360a36c" />
<img width="1915" height="815" alt="receiver" src="https://github.com/user-attachments/assets/dda43633-3e19-412a-897a-e5658fa71b61" />

### After running it got a pwd token which might be mario's passoword .
<img width="1920" height="713" alt="mario-base64" src="https://github.com/user-attachments/assets/9c9caf38-9bbf-441e-b460-0bcad9f0eb42" />

### I used cyberchef to get the password as the token was in base64 .
<img width="1539" height="567" alt="mario-pwd" src="https://github.com/user-attachments/assets/3f98b2a8-5e45-4a46-bc21-93d7dc5a3d06" />

### As we got the password we can do `su mario` and enter the password .
### Used `sudo -l` but got nothing interesting , so searched for our first flag but showing permission denied so i copied it to `/tmp` to see as in tmp dir every permission are given .
<img width="1920" height="741" alt="first-flag" src="https://github.com/user-attachments/assets/50c26034-b737-4a85-a784-228673de3d8b" />






 
