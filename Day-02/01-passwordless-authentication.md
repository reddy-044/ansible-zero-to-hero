# How to setup Passwordless Authentication

## EC2 Instances

### Using Public Key

```
ssh-copy-id -f "-o IdentityFile <PATH TO PEM FILE>" ubuntu@<INSTANCE-PUBLIC-IP>
```

- ssh-copy-id: This is the command used to copy your public key to a remote machine.
- -f: This flag forces the copying of keys, which can be useful if you have keys already set up and want to overwrite them.
- "-o IdentityFile <PATH TO PEM FILE>": This option specifies the identity file (private key) to use for the connection. The -o flag passes this option to the underlying ssh command.
- ubuntu@<INSTANCE-IP>: This is the username (ubuntu) and the IP address of the remote server you want to access.

### Using Password 1
(if your vm has alredy provisioned with username and passwd)
connect to vm using ssh azureuser@128.24.117.89 and passwd 
- Go to the file sudo vim`/etc/ssh/sshd_config
- Update `PasswordAuthentication yes`
- Restart SSH -> `sudo systemctl restart ssh'
- then do ssh-copy-id azureuser@128.24.117.89
- give passwd of it 'yadav@123456'
- then logout and again try ssh azureuser@128.24.117.89  you be able to connect your vm without passwd
### Using Password 2
- (if your vm has provisioned with ssh key)
- connect to vm using ssh -i path azureuser@128.24.117.89
- Go to the file sudo vim`/etc/ssh/sshd_config
- Update `PasswordAuthentication yes`
- Restart SSH -> `sudo systemctl restart ssh'
- then create passwd by using sudo passwd username
- give new passwd
- then do ssh-copy-id azureuser@128.24.117.89
- give passwd of it 'yadav@123456'
- then logout and again try ssh azureuser@128.24.117.89  you be able to connect your vm without passwd


### control node and manage node configuration cmds
### control node
ssh-keygen
ls ~/.ssh/
cat ~/.ssh/id_ed25519.pub
ls -l
vim inventory.ini

### manage node
mkdir -p ~/.ssh
vim ~/.ssh/authorized_keys (past code of cat ~/.ssh/id_ed25519.pub )
chmod 600 ~/.ssh/authorized_keys
chmod 700 ~/.ssh

Try ADAC Cmd: ansible -i inventory.ini -m ping webservers





#######

Module	🧠 What It Does (Simple)	💻 Example Command
1️⃣	command	- Run any Linux command (no pipes or vars)	- ansible all -m command -a "uptime"
2️⃣	shell - 	Run Linux command with pipes or redirection	- `ansible all -m shell -a "cat /etc/passwd
3️⃣	copy -	Copy a file from your system to VM -	ansible all -m copy -a "src=/tmp/a.txt dest=/home/azureuser/"
4️⃣	file -Create/delete a file or folder -	ansible all -m file -a "path=/tmp/test.txt state=absent"
5️⃣	yum	 - Install packages in RHEL/CentOS (Red Hat)	ansible all -m yum -a "name=httpd state=present"
6️⃣	apt	- Install packages in Ubuntu/Debian -	ansible all -m apt -a "name=nginx state=present"
7️⃣	service	Start, stop, restart services like nginx, httpd	ansible all -m service -a "name=nginx state=restarted"
8️⃣	user - Create or delete Linux users - ansible all -m user -a "name=testuser state=present"
9️⃣	authorized_key - 	Add your public SSH key to a server	- ansible all -m authorized_key -a "user=azureuser key='{{ lookup('file', '~/.ssh/id_rsa.pub') }}'"
🔟	setup -	Show system details like RAM, CPU, IP -	ansible all -m setup
🔢	lineinfile -	Add or update a line in a file	ansible all -m lineinfile -a "path=/etc/hosts line='10.0.0.5 web01'"
🔢	unarchive -	Extract .tar or .zip files	ansible all -m unarchive -a "src=/tmp/app.tar.gz dest=/opt/ remote_src=yes"
🔢	cron -	Create or remove a cron job	ansible all -m cron -a "name='cleanup' minute=0 hour=2 job='/opt/cleanup.sh'"
