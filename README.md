<img src="https://imgur.com/qbGYyhh.png"
     style="float: left; margin-right: 10px;" />

# INTRODUCTION
This is a step-by-step walkthrough of the Hack The Box Easy Machine: Inject, I will be showcasing the strategies and techniques I used to successfully conquer it.<br/>
<br />

## ENVIRONMENTS USED
- Kali linux</b> 
- Burp suite

## Work Space
Lets first create the directory we will be working in and cd into it.
```
mkdir inject && cd inject
```
<img src="https://imgur.com/pJaYMhJ.png"
     style="float: left; margin-right: 10px;" />

## Reconnaissance
We start of with a complete port scan of the machine using nmap. The following scans are open
- Port 22: We will potentially SSH into this machine. 
- Port 8080

We will enumerate port 8080, and see what we can find along the way to gain access.

```
sudo nmap -sVC -p- -T4 -Pn --open -O {target} -oN tcpfullscan
```
<img src="https://imgur.com/ukTWtoH.png"
     style="float: left; margin-right: 10px;" />

### Port 8080
We start off by visiting the webserver and exploring it. 

<img src="https://imgur.com/LomWQ2w.png"
     style="float:  left; margin-right: 10px;" />
     
## Webserver analysis  
- checking the web on wappalyzer we can note that the webserver is running on Bootstrap. 
- login gives me an error page.
- register page is under construction and not much we can do.   
- we spot an upload function, first I will attempt to uplaod the HTB Inject png image and see if there arent any restrictions. 
<img src="https://imgur.com/drfEhS9.png"
     style="float: left; margin-right:10px;"  />
  
## LFI
- I wa able to upload and view the uploaded image, this parameter was reflected on the URL which means the websever could probably be vunerable to Local file inclusion. 
- Now that I know we can upload onto the webserver, I will run Burp Suite to try catch the requests we send. 
<img src="https://imgur.com/WqErkDX.png"
     style="float: left; margin-right:10px;" />
  
## BURP SUITE
- I will start burp suite, upload the file and turn on interceptor. 
- Once captured, I can manipulte the get requests to find where the image was uploaded. 
- Further enumeration of the files using burp suite I also find the framework used is the springcloud framework boot. 
<br/>
<img src="https://imgur.com/x1Lkt9I.png"
     style="float: left; margin-right:10px;" />
<br/>
- From here on we can move around to find important info in the directories. <br/>
- We managed to find a password and username belonging to pbil in the Frank directory. <br/> 
- This could be useful as we move along with our enumeration. <br/>

<img src="https://imgur.com/xBK3fHi.png"
     style="float: left; margin-right:10px" />     
<br/>

## GOOGLE FU
- Doing some research on the framwork I find that its vunerable to the spring cloud function spell. <br/>
- We can hop onto metasploit to exploit this vunerability. <br/>

```
https://www.rapid7.com/blog/post/2022/04/01/metasploit-weekly-wrap-up-155/
```
<br/>
## METASPLOIT
- Run metasploit and exploit the framework vunerability
- This gives us remote access to the server. 
- We will use this tool to find our user flag. 

<img src="https://imgur.com/3UpHmN0.png"
     style="float: left; margin-right:10px;" />

- navigating through the directories we get to a user.txt file which we do not have permission to view. 
- This means we need higher priveleges to read this file. 
- We navigate back to phil and swicth user, which requested password that we found during our Burp suite enumeration. 
- Navigating back to the user text, i can now read the flag. 

 <br/>
<img src="https://imgur.com/uoielW3.png"
     style="float: left; margin-right:10px;" />
 
<br/>
### ROOT FLAG
<br/>

- To gain root access on the server, we navigate back to the root folder, the top-level directory of the server's file system.<br/>
- Our next step is to create a YML file that we will upload to the server. The designated location for uploading this file is **/opt/automation/tasks**.<br/>

<img src="https://imgur.com/Vgm4n8g.png"
     style="float: left; margin-right:10px;" />    
<br/>
- Once we have accessed the specified path, we proceed to write a script that will help us increase our privileges.<br/>
 
<img src="https://imgur.com/QWIJPvB.png"
     style="float: left; margin-right:10px;" />
 <br/>
- Run a server  
<img src="https://imgur.com/9kLQ7yG.png"
     style="float: left; margin-right:10px;" />

































