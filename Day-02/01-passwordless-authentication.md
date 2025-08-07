# How to setup Passwordless Authentication


### Passwordless SSH Setup Between Control Node and Managed Node (or)  Setting up SSH Key-Based Authentication
 IF VM IS CREATED WITH PASSWORD :
 
STEP 1: SSH into Control VM 
ssh azureuser@<control-vm-public-ip>

STEP 2: Generate SSH key
ssh-keygen -t ed25519
Just press Enter 3 times
This creates:
Private key: ~/.ssh/id_ed25519
Public key: ~/.ssh/id_ed25519.pub

STEP 3: Copy the SSH key to the newly deployed VM
ssh-copy-id azureuser@10.0.1.5
Replace 10.0.1.5 with your new VM’s private IP

STEP 4: Test SSH access from Control VM to Target VM
ssh azureuser@10.0.1.5
You should log in without a password

 STEP 5: (Optional but Recommended) Disable password login on the Target VM
After logging into web-vm:
sudo vim /etc/ssh/sshd_config

PasswordAuthentication no
PermitRootLogin no
Save and exit (:wq)

Then restart SSH:
sudo systemctl restart ssh

### Passwordless SSH Setup Between Control Node and Managed Node (or)  Setting up SSH Key-Based Authentication
IF VM IS CREATED WITH SSH KEY:

STEP 1: SSH into Control VM 
ssh azureuser@<control-vm-public-ip>

STEP 2: Generate SSH key
ssh-keygen -t ed25519
Just press Enter 3 times
This creates:
Private key: ~/.ssh/id_ed25519
Public key: ~/.ssh/id_ed25519.pub

Step 3: On Control VM – View your public key
cat ~/.ssh/id_ed25519.pub

Step 4: SSH into target VM from your laptop (if you can):
ssh -i ~/.ssh/your-private-key azureuser@<target-vm-public-ip>
Then on target VM:
mkdir -p ~/.ssh
vim ~/.ssh/authorized_keys

Paste the key you copied from control VM

chmod 600 ~/.ssh/authorized_keys
chmod 700 ~/.ssh
Now, the Control VM can access the Target VM without password.

#### in real world we use jumpserver for login to other vms and in jump server we install ansible and terraform for configuration and deployments so:

If you create a VM using Terraform and inject the jump server's public key, you can connect from the jump server without a password. No need to manually create .ssh or authorized_keys — Terraform handles it. Just use ssh azureuser@<vm-ip> from the jump server if the key matches.
admin_ssh_key {
    username   = "azureuser"
    public_key = file("/home/azureuser/.ssh/id_ed25519.pub")  # Jump server's public key
  }



#######Try ADHOC Cmd: ansible -i inventory.ini -m ping webservers

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
