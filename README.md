<img src="https://imgur.com/Q7fBbju.png"
     style="float: left; margin-right: 10px;" />

# INTRODUCTION
This is a step-by-step walkthrough of the Hack The Box Easy Machine: Inject, I will be showcasing the strategies and techniques I used to successfully conquer it.<br/>
<br />

## ENVIRONMENTS USED
- Kali linux</b> 

## Work Space
Lets first create the directory we will be working in and cd into it.
```
mkdir inject && cd inject
```
<img src="https://imgur.com/pJaYMhJ.png"
     style="float: left; margin-right: 10px;" />

## Reconnaissance
We start of with a complete port scan of the machine using nmap. The following scans are open
- Port 22
- Port 8080

We will enumerate port 8080, and see what we can find along the way to gain access.

```
sudo nmap -sVC -p- -T4 -Pn --open -O {target} -oN tcpfullscan
```
<img src="https://imgur.com/ukTWtoH.png"
     style="float: left; margin-right: 10px;" />

## Port 8080
We start off by visiting the webserver and exploring it. 

<img src="https://imgur.com/LomWQ2w.png"
     style="float:  left; margin-right: 10px;" />
##   
- login gives me an error page.
- register page is under construction and not much we can do.   
- we spot an upload function, first I will attempt to uplaod a random file and see if there arent any restrictions. 
<img src="https://imgur.com/drfEhS9.png"
     style="float: left; margin-right:10px;"  />






Lets start with installing the Windows Server 2019. 

If you encounter error "**Windows cannot find microsoft Software license Terms** ..."  remove the virtual floppy drive and restart the process. 

<img src="https://imgur.com/bRChFmg.png"
     style="float: left; margin-right: 10px;" />

After the installation is complete, go to pc name and change it to an easy to recognise name for a domain controller. 

<img src="https://imgur.com/vo0A2sw.png"
     style="float: left; margin-right: 10px;" />


### Domain Controller Installation
This will acts as a central authority that ensures security, manages access, and keeps things organized within a network of computers in an organization. 
Restart your server, upon restart you should have a windows server manager dashboard. 
- You will click Manage on the top right of your screen.
- **Add roles & features**.
- Click next on default selections until you reach server roles. 
- Check **Active Directory Domain Services** in server roles

<img src="https://imgur.com/CsIJ3In.png"
     style="float: left; margin-right: 10px;" />
     
Click next until you finally get to install. 

<img src="https://imgur.com/C6KMbCQ.png"
     style="float: left;margin-right: 10px;" />


Once complete with installation you will see a notification pop up on the top right. <br />
click it **promote this server to a domain controller.**

<img src="https://imgur.com/7Ww45EH.png"
     style="float: left;margin-right: 10px;" />

### Domain Controller Configuration
Once you promote the server to domain controller, you will have a configuration pop up

-**Deployment configuration**: Add a new forest: Name your domain and give it a .local (company.local)<br />
-**Domain Controll Options**: Create a password<br />
-**Click next for rest of steps until installation and auto reboot**

<img src="https://imgur.com/xhxf7q3.png"
     style="float: left; margin-right: 10px;" />
     
### Domain Controller Complete
You have finally completed the creation of the domain controller.<br />
As you can see your next login will show the domain controller name/administrator

<img src="https://imgur.com/6thQNjT.png"
     style="float: left; margin-right: 10px;" />

### Add 2 Users
- Start the installation process again like you did the server above<br />
- Rename the pc name<br />
- Repeat once more. <br />
- You are all done now, 2 users and the domain controller<br />

<img src="https://imgur.com/54lJx7x.png"
     style="float: left; margin-right: 10px;" />

# USERS, GROUPS & POLICIES

### Users
- Start your domain controller & head over to the **Tools** tab on the top right.<br />
- Click **Active Directory Users & Computers**<br />
- Right click on your domain controller name, and **create new organizational units** name them Groups<br />

<img src="https://imgur.com/3XyRJ6j.png"
     style="float: left; margin-right: 10px;" />

- Remove all users excluding **Administrator & Guest** in the **Users folder** and place them into the new group you created above<br />
- Then go back into the **Users** folder where it should look like below<br />

<img src="https://imgur.com/Vv6Ip7Z.png"
     style="float: left; margin-right: 10px;" />




- <br />
- <br />
