[![Stargazers][stars-shield]][stars-url]
[![Issues][issues-shield]][issues-url]
![Hits](https://hitcounter.pythonanywhere.com/count/tag.svg?url=https://github.com/1PseudoSyntax/Python)

&nbsp;
&nbsp;

[stars-shield]: https://img.shields.io/github/stars/PseudoSyntax/Ubuntu-20.04-Kernel-Setup-Instructions.svg?style=flat-square
[stars-url]: https://github.com/PseudoSyntax/Ubuntu-20.04-Kernel-Setup-Instructions/stargazers
<!--style=flat-square   and  style=for-the-badge-->
[issues-shield]: https://img.shields.io/github/issues/PseudoSyntax/Ubuntu-20.04-Kernel-Setup-Instructions.svg?style=flat-square
[issues-url]: https://github.com/PseudoSyntax/Ubuntu-20.04-Kernel-Setup-Instructions/issues


<!-- PROJECT LOGO -->

<p align="center">
  <a href="https://sketchywebsite.net/">
     <img width="72" alt="Chicken1" width="80" height="80" src="https://user-images.githubusercontent.com/43308680/132917546-e46cdfeb-0f53-4868-af4c-9c3a0742a332.PNG">
  </a>
  
  
  <h3 align="center">Mr.ChickenCombo's Guide to Kernel Setup</h3>

  <p align="center">
    An great guide towards making your own personal Kernel on Linux!
    <br />
    <a href="https://github.com/PseudoSyntax/Ubuntu-20.04-Kernel-Setup-Instructions/wiki"><strong>Explore the docs »</strong></a>
    <br />
    <br />
    <a href="https://github.com/PseudoSyntax/Ubuntu-20.04-Kernel-Setup-Instructions">Home</a>
    ·
    <a href="https://github.com/PseudoSyntax/Ubuntu-20.04-Kernel-Setup-Instructions/issues">Report an Issue</a>
    ·
    <a href="https://github.com/PseudoSyntax/Ubuntu-20.04-Kernel-Setup-Instructions/wiki">Ubuntu Error Wiki</a>
  </p>
</p>

# Ubuntu-20.04-Kernel-Setup-Instructions 
A basic guide on how to install and setup kernel to complete a syscall
*(Note: This guide is fully complete. If you see anything that needs to be emphaisized or changed add a comment )*

Contributors: @DanTheMan#1494 , @Aturasu#8710 

<!-- TABLE OF CONTENTS -->
<details open="open">
  <summary>Table of Contents</summary>
  <ol>
    <li>
      <a href="#Setup-for-VM">Setup for VM</a>
    </li>
    <li><a href="#After-you-Install-Ubuntu">After you Install Ubuntu</a></li>
    <li><a href="#Recommend-Optional-Steps-to-Fix-Screen">Recommend Optional Steps to Fix Screen</a></li>
    <li><a href="#Terminal-Stage">Terminal Stage</a></li>
    <li><a href="#Making-a-Syscall-Successfully">Making a Syscall Successfully</a></li>
    <li><a href="#Summary">Summary</a></li>
  </ol>
</details>

## **Setup for VM**
-Install Ubuntu 20.04 from https://ubuntu.com/download/desktop?version=20.04&architecture=amd64

-Open virtual box and Select Linux ubuntu 64

-Video Memory set to 3010

-Create a virtual hard disk

-Pick VDI

-Pick Dynamic Allocation

-Allocate at least 25GB to it

-Go to storage on VM click disk icon and pick "choose disk image file"

-Insert the ISO file ubuntu 20.04

-Power Up VM

-Pick ubuntu 20.04


## **After you Install Ubuntu**
-Click install Ubuntu

-Click next on all default options including erase disk option

-After Instalation finishes make note of your root password(dont forget it)

-Log In to Ubuntu

-Go to https://www.kernel.org/ on firefox

-Download stable or longterm version kernel

-Drag the zipped file into Documents from recent

-Right click and click "extract here"(give it a min or 2 to fully extract all files)


## **Recommend Optional Steps to Fix Screen**
*These steps are to fix your screen size resolution and to make things easier*

-Click devices 

-Select "Insert Guest Additions CD"

![image](https://user-images.githubusercontent.com/43308680/132942749-63c0f26d-b286-46fd-a407-7634680aeac4.png)


-Click "Run"

-Type in your root password when Prompted

-Click Enter

**If nothing pops up to run use this in your terminal**
>sudo apt-get install virtualbox-guest-dkms virtualbox-guest-utils virtualbox-guest-x11



-Click "Devices" then "Shared Clipboard"

-Select "Bidirectional"

-Close VM and select "power off" or "send shutdown signal"

-Power on VM to see changes

-Select "View" -> "Virtual Screen1" -> Choose whatever resolution you want


## **Terminal Stage**
-Go to apps click or search for settings->power->set power saving to __never__ (this will keep VM from sleeping)

-Right click in the extracted Kernel file and select "open in Terminal"

![image](https://user-images.githubusercontent.com/43308680/132942808-32303107-f4e7-4d57-be16-b84e4ebc111c.png)


Run 
>cp -v /boot/config-$(uname -r) .config

Run 
>sudo apt-get install build-essential libncurses-dev bison flex libssl-dev libelf-dev

Run 
>scripts/config --disable SYSTEM_TRUSTED_KEYS

### **~MAKE SURE KEYS ARE REVOKED:==================================** 

go into the .config file using, 

>sudo nano .config 

Type this to activate search 

>ctrl+w 

and then search for 

>SYSTEM_REVOCATION_KEYS 

Set ="debian/canonical-revoked-certs.pem" to this =""

Then do 

>ctrl+s

to save file. Then do the following below to leave file editor.

>ctrl+x



**===================================================================**

Run ->If you get an error stretch terminal screen a then rerun command 

>make menuconfig

   ->Click enter on general
   
   ->set local version to your first and last name(no spaces) EX: JohnDoe.config
   
   ->Click exit(Use Tab button on keyboard)
   
   ->Click Enter on "Binary Emulations"
   
   ->__turn off 32x abi for 64 bit mode by clicking "N" on it (should be second option)__
   
   ->save as .config then exit the GUI
   


Run 

>make bzImage -j 4

Run 
>make modules -j 4

[optional]Run
>sudo apt install binutils

Run 
>sudo make -j 4

Run
>sudo make modules_install -j 4

Run
>sudo make install -j 4 

Run
>sudo update-grub

Run -> change timeout to -1
>sudo nano /etc/default/grub file

Run
>sudo update-grub

Run
>reboot

-When it finishes booting go to "advanced Ubuntu Options" and select kernel with your name to boot up

-You can run the command below to ensure you booted the right kernel. It should have your name or whatever you named the .config file

>uname -r

## **Making a Syscall Successfully**
-Make sure your inside your booted kernel

-Start out by going into the kernel folder directory 

![image](https://user-images.githubusercontent.com/43308680/132941635-505f4a44-066d-4323-ab8f-671d8a47dfbe.png)

-Write to a new C file using the following

>nano -w nameOfFile.c

-Type some code for the C file and be sure to add at the top of it
```
#include <linux/syscalls.h>
#include <linux/printk.h>
```

-In the kernel folder directory edit the Makefile

>nano Makefile

Add nameOfFile.o to the end of this line in the file
![image](https://user-images.githubusercontent.com/43308680/132941795-3fc0b588-8c62-4feb-a9c4-523727f60a3f.png)

Save the file

>ctrl+s 

-Go to the linux-5.14.2/arch/x86/entry/syscalls directory path and edit the files syscall_32.tbl and syscalls_64.tbl

**NOTE: use TAB since these files don't support spaces**

   In the 32x file go to the bottom and add the proper formatting like so
   ![image](https://user-images.githubusercontent.com/43308680/132942162-8b3ca317-446f-4997-b41b-b23e9e2fcc7c.png)

   In the 64x file go to the bottom and add the proper formatting like so
   ![image](https://user-images.githubusercontent.com/43308680/132942220-ff2c7418-15f4-4191-852f-0ca123d9bf57.png)

-Navigate back to kernel directory then run
>sudo make kernel/mycall.o

-Now navigate back to the base kernel directory linux-5.14.2[or whatever version you have] and run
>sudo make -j 4

-When that finishes run
>sudo make install -j 4

-When that finishes run
>reboot

-When Ubuntu loads back up select a file outside of your kernel folder (ie downloads or desktop for example) and write to a new C file
>nano -w secondFile.c

-Add some code to the file using this format at the top. For the arguments add the integer of the line added in the 64x.tbl file
```
#include <unistd.h>
#include <linux/kernel.h>
#include <sys/syscall.h>

```

-Save and exit the file with 
>ctrl+s
>ctrl+x

-Run in the terminal your second C file
>gcc secondFile.c

-Run in the terminal your output file
>./a.out

-Run this command to see syscall logs as well as your syscall
>sudo dmesg


**Your Done Congrats! Easy wasn't it?**

**No message Popped up!**
If it didnt work try redoing sudo make, there is a possibility that your computer may have sleep and inadvertenly stop the download. If errors appeared
in the log or at any point check here <a href="https://github.com/PseudoSyntax/Ubuntu-20.04-Kernel-Setup-Instructions/wiki">Ubuntu Error Wiki</a>

## **Summary**

Hopefully this helped you set up the VM and kernel properly. If you do find issues please let me know via comments or commits. This guide 
was intended for new Linux users who want to know how to build a kernel on Ubuntu as well as a reference for me if my VM ever gets corrupted.



 

![Jokes Card](https://readme-jokes.vercel.app/api)

