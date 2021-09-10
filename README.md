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

go into the .config file using, >sudo vi .config 

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


## **Summary**
Hopefully this helped you set up the VM and kernel properly. More steps regarding syscall will come soon after images are added for this guide.
<img width="72" alt="Chicken1" src="https://user-images.githubusercontent.com/43308680/132917546-e46cdfeb-0f53-4868-af4c-9c3a0742a332.PNG">
