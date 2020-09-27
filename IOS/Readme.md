# IOS Penetration Testing 
<img title="GitHub version" src="https://camo.githubusercontent.com/5a5546dd5c71564ddcdb6f7a315bf7dac2033915/68747470733a2f2f6432356c6369707a696a3137642e636c6f756466726f6e742e6e65742f62616467652e7376673f69643d676826747970653d3626763d312e302e302678323d30" data-canonical-src="https://d25lcipzij17d.cloudfront.net/badge.svg?id=gh&amp;type=6&amp;v=1.0.0&amp;x2=0" style="max-width:100%;">

### Follow me on
[![Twitter Follow](https://img.shields.io/twitter/follow/thevillagehackr?style=social)](https://twitter.com/thevillagehackr)![Twitter Follow](https://img.shields.io/twitter/follow/TVHSecurity?style=social)
## Introduction
In this article series, i will be sharing about the tools and techniques which i used to perform penetration testing and Vulnerability assessment on IOS Applications.

## Types of Analysis
There are two types of analysis present to perform penetration testing are :
* Static
* Dynamic

## Static Ananlysis
To perform static ananlysis we need an good reverse engineering tool most of the people prefer MOBSF Framework for decompiling both android and IOS (ipa) package files for static analysis.

### Tool
>[MobSF Security Framework ](https://github.com/MobSF/Mobile-Security-Framework-MobSF)

Once the tool is installed run in on localhost and upload the (ipa) package file and analyze.

Check for manifest.xml file and check for if there is any hardcoded credentials like internal IP, Database Credentials, API key.

## Dynamic Analysis

### Product Information
> * Device Model: Iphone 6
> * Os Version: 12.4.5  

### Jailbreaking your device
If you are serious about IOS security, then having a jailbroken device is a must. In this section, we will look at how we can jailbreak an IOS device. Jailbreaking a device has many advantages. You can install tools like nmap, metasploit and even run your own custom python code on the device. Imagine having the power to run a vulnerability scan on a website from the palm of your hand. So i used a tool called [checkra1n](https://checkra.in/linux) to jailbreak my IOS device. For installation check the link below [checkra1n_installation](https://checkra.in/linux).
Once the tool is successfully installed the connect your Iphone to your PC through USB and at your terminal type **checkra1n** and the terminal like below opens.

![figure 1](https://github.com/rootnvnj/Mobile-Penetration-testing/blob/master/IOS/img/checkrain.png)

No need to enter into boot mode or something just plug the usb to start jailbreaking. Once the jailbreaking is successfully done then cydia is automatically installed on your device.
 
 ### Install .ipa files in your device
 Once the jailbreaking is done then we need to install the .ipa file in our device download the [3utools](http://www.3u.com/) and install it in windows machine.Install the .ipa file in your device as below procedure for your reference:
 >* [Article-1](http://www.3u.com/news/articles/1505/how-to-install-ipa-file-in-iphone-using-3utools)
 >* [Article-2](https://kubadownload.com/news/3utools-install-ipa/)

### Install Frida in ios device
* Install Frida on Device
* Start Cydia App
* Add Frida's repository by navigating through the menu: Manage => Sources => Edit => Add
* Enter https://build.frida.re
* Wait till Cydia reloads the repository.
* Find and install the frida package or if you want a specific version<br>
*For your reference*
<div style="text-align: center">
<img src="https://github.com/rootnvnj/Mobile-Penetration-testing/blob/master/IOS/img/frida.png" alt="drawing" width="500"></div>

### Frida Usage
Frida reference [frida_docs](https://frida.re/docs/ios/)<br>
After the installation of Frida fireup your terminal and follow these instructions:<br>
  
* Install frida in your attack machine by using python-pip `pip3 install frida-tools` 
* Now, back on your Windows or macOS system it’s time to make sure the basics are working. Run:<br>
`$ frida-ps -U`
* make sure your device is plugged via USB
* Unless you already plugged in your device, you should see the following message:
`Waiting for USB device to appear...
`
* Plug in your device, and you should see a process list along the lines of:
```js

 PID NAME
 488 Clock
 116 Facebook
 312 IRCCloud
1711 LinkedIn

```
Great, we’re good to go then! now you can run frida to know what proess are running in your device. Make sure USB is connected when running Frida.

#### Install OpenSSH for remote access
* start cydia App
* Navigate to search option and search OpenSSH and click to install
<div style="text-align: center">
<img src="https://github.com/rootnvnj/Mobile-Penetration-testing/blob/master/IOS/img/openssh0.PNG" alt="drawing" width="375"></div>
<br>

* select and **install**
<br>

<div style="text-align: center">
<img src="https://github.com/rootnvnj/Mobile-Penetration-testing/blob/master/IOS/img/openssh1.PNG" alt="drawing" width="400"/></div>

Initiate remote access through SSH you need to know the device IP Navigate to **Settings-> WiFi-> [your connected network]-> IP Address**<br>

`ssh root@<device_ip>` the apple devices default password is ***alpine***
Then check for file systems for any files like database files. For analyzing those database files you better install [snowflake](https://github.com/subhra74/snowflake) for goog experience. Snowflake is SSH tool for both windows and linux platforms. by using snowflake the file transfer and listing the directories through SSH is better comfortable.

### Runtime Analysis with Objection
### Install Objection Tool
Now, we will setup Frida on our computer by installing Objection (which includes Frida!).Installation is easy – you will need to have python3 and pip3 installed, then simply open up a new terminal window and type:

`pip3 install objection`

<div style="text-align: center">
<img src="https://github.com/rootnvnj/Mobile-Penetration-testing/blob/master/IOS/img/obj-1.png" alt="drawing" width="800"></div>

#### Application exploration:
1. To browse applications file

    `ls`
1. Print current directory

    `pwd print`

3. To browse applications file

    `cd /folder/path/name`
#### Sensitive Data Exposure
1. Dump .plist files:
* Print environment information
   
    `env`

* Goto document folder
 
``` sh
cd /var/mobile/Containers/Data/Application/04256310-C7E7-4A1A-BA98-54D4F98235E2/Documents 
```

* Download .plist file
    `file download Credentials.plist creds.plist`
 
* To read that downloaded file:
    `!type creds.plist`

2. Dump keychain file of target application:
Keychain is an encrypted container (128 bit AES algorithm) and a centralized SQLite database that holds identities & passwords for multiple applications and network services, with restricted access rights.

    `ios keychain dump`

3. Dump sqlite files:
* Print environment information
 
    `env`
* Goto document folder

```sh
cd /var/mobile/Containers/Data/Application/04256310-C7E7-4A1A-BA98-54D4F98235E2/Documents
```
* Download .sqlite file
```sh 
sqlite connect /var/mobile/Containers/Data/Application/04256310-C7E7-4A1A-BA98-54D4F98235E2/Documents/Credentials.sqlite file
```
4. Memory dump:
* We will download and save memory dump into json file:
  
    `memory list modules --json memory.json`
It will list memory modules and store in **memory.json**

* To read memory.json
    `!type memory.json`

#### SSL pinning disable/bypass:
One of the cool feature of objection is one command ssl pinning bypass. To disable ssl pinning checks for target application run following command:

`ios-sslpinning-disable`

#### Dump cookies stored by target application:
Some of iOS application’s stores user’s session cookies into local storage. This cookies may contain server secrets, oauth tokens, api keys, etc.

`ios cookies get`

#### Job listing and kill:
You can list and enable/kill any running job according to test scenarios.
* To list out all running jobs:
    `jobs list`
* Run following command to kill job
    `jobs kill JOBID`

#### Running Objection

`objection --gadget AppName explore`

Now you should see this output

<div style="text-align: center">
<img src="https://github.com/rootnvnj/Mobile-Penetration-testing/blob/master/IOS/img/obj-1.png" alt="drawing" width="800"></div>

For more information https://kalilinuxtutorials.com/objection-mobile-exploration/ 

While we are talking about the filesystem, it is also possible to download files straight off the device (where you have read access) as well as have the ability to re-upload files where write access is granted, such as the applications Documents directory.

objection also includes an inline SQLite editor to make manipulating random sqlite databases that might exist a breeze.

<div style="text-align: center">
<img src="https://github.com/rootnvnj/Mobile-Penetration-testing/blob/master/IOS/img/objection_sqlite.png" alt="drawing" width="800"></div>

#### Finding methods for a class
Let’s define a search to find a class.

`ios hooking list classes`

`ios hooking search classes <class_name>`

To print, the methods for a particular class is as simple as:

`ios hooking search methods <class_name>`

We can now modify them at runtime and set to true/false.

`ios hooking set return_value`

#### Executing Frida Scripts
If you try to run the scripts as a file from the command line (frida -U -p 1234 -l test_script.js) then it will get terminated if execution time exceeds 28 seconds.

I recommend attaching to the target app’s process and then pasting the Frida script code you want to execute.You can also utilize the Python script for executing the Frida script code.

### Requests and Responses of an iOS Application
Once your device is connected to your network configure the device proxy server as your machine IP and PORT as what you have in Burpsuite Proxy. Set the Burpsuite proxy into all interfaces

<div style="text-align: center">
<img src="https://github.com/rootnvnj/Mobile-Penetration-testing/blob/master/IOS/img/proxy.png" alt="drawing"></div>

Then now you can able to intercept the connection.


## Conclusion
So these are all the steps i researched and doing Security testing for IOS Devices.
