<h1>Project name</h1>
SSH public key authentication

<h2>Project Description</h2>
This repository explains how to authenticate to a VPS using public \ private keys via SSH

<h2>Motivations</h2>
You have a server and you want to work on it secuerly

<h3>why SSH</h3>
SSH allow the info between client and server to be encrypted (reference [1])

<h3>why private \ public key authetication</h3>
More secure than user\password authentication

<h2>Basic assumptions</h2>
<ul>
<li>client : linux client or WSL if you use windows 10</li>
<li>server : linux</li>
<li>you : have basic knowledge in linux</li>
</ul>

<h2>How to authenticate a VPS user using SSH and public \ private keys</h2>

<h3>Step 1 : Allow public key autentication via /etc/ssh/sshd_config</h3>

```bash
nano /etc/ssh/sshd_config
PubkeyAuthentication yes 
```

after you save the file 

```bash
systemctl restart sshd
```



<h3>Step 2 : Create locally public \ client key files using ssh-keygen</h3>

```bash
ssh-keygen
```

This will create two files on  ~/.ssh

<h3>Step 3 : Copy the public key to the server using ssh-copy-id</h3>

<h3>Step 4 : Login to the server using the command ssh</h3>

<h3>Step 5 : Optionally add more security via /etc/ssh/sshd_config</h3>

<h2>Demo</h2>
This demo was created using digital ocean droplet and ubuntu 

<h3>Default public \ private keys</h3>

<h3>Non Default public \ private keys</h3>


<h2>Points of Interest</h2>
<ul>
    <li>windows 10 WSL client</li>
    <li>temporary use 'PasswordAuthentication yes' before ssh-copy-id </li>
</ul>

<h2>References</h2>
<ul>
    <li><a href='https://www.youtube.com/watch?v=R48-UaZ4q1k'> SSH Essentials in 7.5 minutes </a></li>
</ul>

