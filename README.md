# SSH
SSH keys provide a secure way of logging into a Linux on the cloud or virtual machine on prem. 
```  bash
ssh [-46AaCfGgKkMNnqsTtVvXxYy] [-B bind_interface] [-b bind_address] [-c cipher_spec]
         [-D [bind_address:]port] [-E log_file] [-e escape_char] [-F configfile] [-I pkcs11]
         [-i identity_file] [-J destination] [-L address] [-l login_name] [-m mac_spec]
         [-O ctl_cmd] [-o option] [-p port] [-Q query_option] [-R address] [-S ctl_path]
         [-W host:port] [-w local_tun[:remote_tun]] destination [command]
``` 
### Steps to setup SSH connection
### Virtualbox
- install openssh-server
### Host
- install openssh-client
### Create the RSA Key Pair
```ssh-keygen -t rsa```
### Store the Keys and Passphrase    
- The ***public key*** is now located in /.ssh/id_rsa.pub. ***the one you send out***
- The ***private key*** (identification) is now located in /.ssh/id_rsa. 
### Copy the Public Key
#### from linux host
``` cat ~/.ssh/id_rsa.pub | ssh kosint@127.0.0.1 "mkdir -p ~/.ssh && chmod 700 ~/.ssh && cat >>  ~/.ssh/authorized_keys"```
#### from windows host
#### copy the key into a tmp file
``` scp -P 2222 id_rsa.pub kosint@127.0.0.1:/tmp/id_rsa.pub```
##### append the key into the authorized keys
``` cat /tmp/id_rsa.pub >>  ~/.ssh/authorized_keys```
### Disable root login
#### On the server side
```sudo nano /etc/ssh/sshd_config```
#### Add this line
```PermitRootLogin without-password```
#### Reload
```sudo systemctl reload sshd.service```

### Copy files to/from
#### single file to
```scp myfile.txt kosint@127.0.0.1:/remote/folder/```
#### * files to
```scp -r * kosint@127.0.0.1:/remote/folder/```
#### single file from 
```scp kosint@127.0.0.1:/remote/folder/myfile.txt  myfile.txt```
#### * files from 
```scp -r * kosint@127.0.0.1:/remote/folder/```

### Use SSH to Create an HTTP Proxy
#### client side
```ssh -D 8123 -f -C -q -N kosint@127.0.0.1```
##### flags:
- D: Tells SSH that we want a SOCKS tunnel on the specified port number (you can choose a number between 1025-65536)
- f: Forks the process to the background
- C: Compresses the data before sending it
- q: Uses quiet mode
- N: Tells SSH that no command will be sent once the tunnel is up
##### Configure Firefox
- Open Firefox, 
	- go to Edit
		- →Preferences
			- →Advanced
				- →Network
					- →Settings and specify that you want to use a Manual Proxy, localhost, port 12345 and SOCKS v5


### Use GUI programs from ssh
#### x11 
from windows you need to have installed xming. 
```ssh -XC kosint@127.0.0.1 -p 2222```

#### firefox
```firefox &```

### Reference
- [how-to-set-up-ssh-keys-on-linux-unix?](https://www.cyberciti.biz/faq/how-to-set-up-ssh-keys-on-linux-unix/)
- [how-to-set-up-ssh-keys](https://www.digitalocean.com/community/tutorials/how-to-set-up-ssh-keys--2)
- [how-to-copy-files-using-ssh](https://www.simplified.guide/ssh/copy-file)
- [SSH/OpenSSH/PortForwarding](https://help.ubuntu.com/community/SSH/OpenSSH/PortForwarding)
- [how-to-route-web-traffic-securely-without-a-vpn-using-a-socks-tunnel](https://www.digitalocean.com/community/tutorials/how-to-route-web-traffic-securely-without-a-vpn-using-a-socks-tunnel)
- [forward-ports-to-a-virtual-machine-and-use-it-as-a-server](https://www.howtogeek.com/122641/how-to-forward-ports-to-a-virtual-machine-and-use-it-as-a-server/)
- [Configure VM portforwarding video](https://youtu.be/ErzhbUusgdI?t=39)

![](https://raw.githubusercontent.com/frankietyrine/K-OSINT.iso/master/unnamed.png)
