# Remote display Vscode from Linux to Windows
Visual Studio Code open at Linux Host and Display on windows

##
Since I always can't remember so I make this manual for myself on how-to.

I need to develop lots of code on either Linux Remote VM / Local VM but I am using windows as my desktop and I have try different ways to make it like SAMBA, SFTP but all didn't work well. Like sometime I need to debug from the host and etc... So I think why not Xforward from Linux to Windows.

Installation & configuration
---
#### 
On windows side:
===
Install XLaunach from VcXsrv, Download from <a href="https://sourceforge.net/projects/vcxsrv/" >VcXsrv</a>
then you will need a <a href="https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html">Putty</a>

On windows putty:
Configure the X11Forwarding. 
1. Connection --> X11  -> Check Enable X11 Forwarding 
   for local VM 
2. X display location: localhost:0.0
   for remote machine 
   <Target Host IP address>:0.0
3. Session Save the Setting as 127.0.0.1 with port 22 

on windows VcXsrv (XLaunch.exe) 
Just follow the installer flow to install the application. Once installation completed. 
a X icon should be at the Task menu. 


On Linux side: (Centos 7) 
======
Install xauth 
```bash
   sudo yum install -y xorg-x11-xauth
   xauth list
``` 
1. This will allow the remote system to connecting the Xserver you have installed on you local PC

Install vscode 
2. import the pgp key `sudo rpm --import https://packages.microsoft.com/keys/microsoft.asc` 
3. download the repo 
```bash
 sudo sh -c 'echo -e "[code]\nname=Visual Studio Code\nbaseurl=https://packages.microsoft.com/yumrepos/vscode\nenabled=1\ngpgcheck=1\ngpgkey=https://packages.microsoft.com/keys/microsoft.asc" > /etc/yum.repos.d/vscode.repo' 
 ```
4. check update and install vscode
```bash
    sudo yum check-update
    sudo yum install code
```
5. To prevent strange characters appears. We will need to install true type fonts.
```bash
   yum install curl cabextract xorg-x11-font-utils fontconfig
   yum install https://downloads.sourceforge.net/project/mscorefonts2/rpms/msttcore-fonts-installer-2.6-1.noarch.rpm
```
6. On Linux Host,
    ```bash 
    sudo vi /etc/ssh/sshd_config 
    X11Forwarding yes
    X11DisplayOffset 10
    PrintMotd yes
    PrintLastLog yes
    X11UseLocalHost no
    ```
then restart the ssh service 
   ```bash 
       systemctl restart sshd
   ```
### 
The Startup

Open the Putty SSH, load and open the ssh connectivity. 
login as normal. then go to the directory you want and type 
``` bash 
    code& . 
``` 
a new VS code will be shown on your windows desktop. 
