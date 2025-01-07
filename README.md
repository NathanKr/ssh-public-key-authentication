<h1>Project name</h1>
SSH public key authentication

<h2>Project Description</h2>
This repository explains how to authenticate to a VPS using public \ private keys via SSH

<h2>Motivations</h2>
You have a server and you want to work on it secuerly

<h3>Why SSH</h3>
SSH allow the info between client and server to be encrypted (reference [1])

<h3>Why private \ public key authetication</h3>
More secure than user\password authentication

<h2>Setup</h2>
<ul>
<li>Client machine operating system : is linux or WSL if you use windows 10</li>
<li>Server machine operating system : linux</li>
<li>You : have basic knowledge in linux</li>
</ul>

<h2>How to authenticate a VPS user using SSH and the default public \ private keys</h2>

<h3>Step 1 : Allow public key autentication via the server file /etc/ssh/sshd_config</h3>

```bash
nano /etc/ssh/sshd_config
PubkeyAuthentication yes 
```

Invoke the following command after you save the file 

```bash
systemctl restart sshd
```



<h3>Step 2 : Create locally public \ client key files using ssh-keygen</h3>

Invoke the following command 

```bash
ssh-keygen
```

This will create two files under your client machine  ~/.ssh 
<ul>
<li>public key file - id_rsa</li>
<li>private key file - id_rsa.pub</li>
</ul>

<h3>Step 3 : Copy the public key to the server using ssh-copy-id</h3>
Invoke the following command

```bash
ssh-copy-id username@hostname
```

The public key is copied to the server file /home/username/.ssh/authorized_keys

<h3>Step 4 : Login to the server using the command ssh</h3>

Invoke the following command

```bash
ssh username@hostname
```


<h3>Step 5 : Optionally add more security via /etc/ssh/sshd_config</h3>

```bash
nano /etc/ssh/sshd_config
PasswordAuthentication no 
PermitRootLogin prohibit-password
```

Invoke the following command after you save the file 

```bash
systemctl restart sshd
```


<h2>Demo</h2>
This demo was created using digital ocean droplet and ubuntu. digitial ocean droplet is a VPS

<h3>Default public \ private keys</h3>

Create keys with by invoking the following command

```bash
ssh-keygen 
```

This create the default key pair files id_rsa and id_rsa.pub as shown in the following image

<img src='./figs/default_keys.png'/>

Copy the public key to the server using the following command

```bash
ssh-copy-id 
```

Login using the following command

```bash
ssh username@hostname
```

as shown in the the following image

<img src='./figs/login-root.png'/>

<h3>Non Default public \ private keys</h3>
<p>This use case is relevant if you want to authenticate with more than one server or user</p>

Create keys as shown in the following image. Notice that specific private key file is used 

<img src='./figs/ssh-keygen-non-default.png'/>

The resulted key pair appears in the following image

<img src='./figs/create-non-default-keys.png'/>

Now you need to copy the public key to the server. If you want to use ssh-copy-id and have 

```bash
PasswordAuthentication no
```

As shown in the following image

<img src='./figs/password-no.png'>

Than you need to temporary change it in /etc/ssh/sshd_config to no and after save invoke the following command 

```bash
systemctl restart sshd
```

Now you can use ssh-copy-id

```bash
ssh-copy-id -i ~/.ssh/custom_key_name.pub username@your_server_ip
```

As shown inthe following image

<img src='./figs/copy-non-default.png'/>

Now change back to no

```bash
PasswordAuthentication no
```

and dont forget to invoke the following command after save

```bash
systemctl restart sshd
```

Now login using the specific private key

```bash
ssh -i path_to_private_key username@your_server_ip
```

As shown in the following image

<img src='./figs/cicd-login.png'/>




<h2>Points of Interest</h2>
<ul>
    <li><h3>windows 10 WSL client</h3></li>
    I found WSL to be very convenient as using linux on windows so i am using it.
    <li><h3>Caviates with 'PasswordAuthentication yes'</h3></li>
    If you want to allow a user to authenticate using public key with  'PasswordAuthentication no' and 'PubkeyAuthentication yes' you will be faced with catch 21. Only public key authentication is allowed but the new user does not have public key on the server. you have two options
    <ul>
    <li>Add the public key file to the server using other user that all ready has public \ private keys</li>
    <li>Temporary set 'PasswordAuthentication yes' and after ssh-copy-id is finish , revert back</li>
    </ul>
</ul>

<h2>References</h2>
<ul>
    <li><a href='https://www.youtube.com/watch?v=R48-UaZ4q1k'> SSH Essentials in 7.5 minutes </a></li>
</ul>

