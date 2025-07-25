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

### Using Password 
(if your vm has alredy provisioned with username and passwd)
connect to vm using ssh azureuser@128.24.117.89 and passwd 
- Go to the file sudo vim`/etc/ssh/sshd_config
- Update `PasswordAuthentication yes`
- Restart SSH -> `sudo systemctl restart ssh'
- then do ssh-copy-id azureuser@128.24.117.89
- give passwd of it 'yadav@123456'
- then logout and again try ssh azureuser@128.24.117.89  you be able to connect your vm without passwd

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

