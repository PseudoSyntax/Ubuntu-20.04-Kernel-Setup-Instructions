![Hits](https://hitcounter.pythonanywhere.com/count/tag.svg?url=https://github.com/TPseudoSyntax/Python)
# Ubuntu-20.04-Kernel-Setup-Instructions 
A basic guide on how to install and setup kernel to complete a syscall

*(Note: This guide is not fuly complete. If you see anything that needs to be emphaisized or changed add a comment )*
Credits: @DanTheMan#1494 , @Aturasu#8710 

## **~Setup for VM**
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


## **~After you Open Ubuntu**
-Click install Ubuntu

-Click next on all default options including erase disk option

-After Instalation finishes make note of your root password(dont forget it)

-Log In to Ubuntu

-Go to https://www.kernel.org/ on firefox

-Download stable or longterm version kernel

-Drag the zipped file into Documents from recent

-Right click and click "extract here"(give it a min or 2 to fully extract all files)


## **~Reccomended Optional Steps to Fix Screen**
*These steps are to fix your screen size resolution and to make things easier*

-Click devices 

-Select "Insert Guest Additions CD"

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


## **~Terminal Stage**
-Go to apps click or search for settings->power->set power saving to __never__ (this will keep VM from sleeping)

-Right click in the extracted Kernel file and select "open in Terminal"[**see image01**]

Run >cp -v /boot/config-$(uname -r) .config

Run >sudo apt-get install build-essential libncurses-dev bison flex libssl-dev libelf-dev

Run >scripts/config --disable SYSTEM_TRUSTED_KEYS

**===MAKE SURE KEYS ARE REVOKED:===** 

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



**===============================**

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

## **Making a Syscall Successfully[in progress]**
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

   In the 32x file go to the bottom and add the proper formatting like so
   ![image](https://user-images.githubusercontent.com/43308680/132942162-8b3ca317-446f-4997-b41b-b23e9e2fcc7c.png)

   In the 64x file go to the bottom and add the proper formatting like so
   ![image](https://user-images.githubusercontent.com/43308680/132942220-ff2c7418-15f4-4191-852f-0ca123d9bf57.png)

-Now navigate back to the base kernel directory linux-5.14.2[or whatever version you have] and run
>sudo make -j 4[# of jobs]

-When that finishes run
>sudo make install -j 4[# of jobs]

## **Summary**

Hopefully this helped you set up the VM and kernel properly. More steps regarding syscall will come soon after images are added for this guide. 



<img width="72" alt="Chicken1" src="https://user-images.githubusercontent.com/43308680/132917546-e46cdfeb-0f53-4868-af4c-9c3a0742a332.PNG"> 

![Jokes Card](https://readme-jokes.vercel.app/api)

