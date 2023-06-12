<img src="https://imgur.com/Q7fBbju.png"
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
<br/>
<img src="https://imgur.com/x1Lkt9I.png"
     style="float: left; margin-right:10px;" />
<br/>
- From here on we can move around to find important info in the directories in here. 
- We managed to find frank and Phill users and credentials. 

<img src="https://imgur.com/s3CgLCZ.png"
     style="float: left; margin-right:10px" />
  
<br/>

<img src="https://imgur.com/xBK3fHi.png"
     style="float: left; margin-right:10px"


## PHP Reverse shell 
- You can find php webshells in Kali located at the below directory. Copy it into the inject directory
- You will then edit the file by adding the port we will listen on and our ip address. 
```
/usr/share/webshells/php/php-reverse-shell.php 

```
```
cp /usr/share/webshells/php/php-reverse-shell.php 

```
```
nano php-reverse-shell.php 

```
<img src="https://imgur.com/ce5XAAZ.png"
     style="float: left; margin-right:10px;" />

<br/>

<img src="https://imgur.com/QBGrI2J.png"
     style="float: left; margin-right:10px;" />
     
    
 


































