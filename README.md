# BinwalkGroupProject
##**Welcome to Project Binwalk!**

###**Meet the team!**

Cameron Smith

<img src="https://user-images.githubusercontent.com/77790231/140398703-95f4a65e-1a8f-4f6e-abb5-d00ce65a0f0b.jpg" width="300" height="200" />

Ben Weddle

Orion Radford

Christopher Burke

<img src="https://github.com/MerelyChris/ExtractionImages/blob/main/Snapchat-1192554281.jpg" width="300" height="200" />

Binwalk is a tool that excells at searching through a file in order to find embedded files and executable code more specfically in firmware images.
binwalk also uses the libmagic library(which is a library which supports the file command on a unix system and handles the loading of database files . I have also included a [link](https://filemagic.readthedocs.io/en/latest/guide.html#:~:text=libmagic%20is%20the%20library%20that,as%20the%20associated%20mime%20types) with more information on that if you find it interesting or what to know more!) Now before we can jump into binwalk you are going to want to download kali linux for your WSL (Windows subsystem for linux) which can be found [here](https://aka.ms/wsl-kali-linux-new) you can download binwalk on windows but it is reccomended you install it on linux but if you really want it on windows i have provided a [link](https://blog.eldernode.com/install-and-use-binwalk-on-windows/#:~:text=Binwalk%20can%20be%20installed%20and,you%20install%20it%20on%20Linux.) for that aswell.

Now that you have your kali linux distrobution open it up and it will ask you to create a username and password for it I would reccomend you keep it to something you will remmeber. and afterwords it should look something like this. Now this is just a terminal version here of kali linux and you can get a desktop gui the process for that is [here](https://www.bleepingcomputer.com/news/security/kali-linux-gets-a-gui-desktop-in-windows-subsystem-for-linux/)
<img width="960" alt="binwalk1" src="https://user-images.githubusercontent.com/77790231/139098296-768c7dec-528f-47f1-a407-3d270380af9a.png">
now that we have kali up and going you can get binwalk installed very easily by doing 'wget https://github.com/ReFirmLabs/binwalk/archive/master.zip' this will download the master zip file from the binwalk repo which you can find [here](https://github.com/ReFirmLabs/binwalk) and is a very useful resource when first learning how to use binwalk
and this is where you run into your first problem is that you need to unzip the zip file you just got but you need to capabilities to do so. so your going to want to enter the command 'sudo apt-get install unzip' get that installed and then you can do 'unzip master.zip' and that will finish the binwalk instal process and you can do 'binwalk' and your screen should look something like this 
<img width="960" alt="binwalk2" src="https://user-images.githubusercontent.com/77790231/139099510-44c03423-e4a9-4a4b-bd15-5f9c14a88c46.png">
and boom were done installing binwalk and it was a super easy process compared to some of other tools out there but unfortunaly for us we dont get any cool UI or anything like that we just got a command like interface. and now we can move on to using binwalk

###**Cameron Smith - Signature Scan Options**
Alright for my part I will be doing signature scan options for binwalk which is easy to do and provides you with information about the firmware file I will be using which can be found [here](https://download1.dd-wrt.com/dd-wrtv2/downloads/betas/2017/10-10-2017-r33492/linksys-wrt1200ac/ddwrt-linksys-wrt1200ac-webflash.bin)
well start with just the basic scan command on the file you just downloaded 'binwalk -b ddwrt-linksys-wrt1200ac-webflash.bin' and you should get something like this
<img width="960" alt="binwalk3" src="https://user-images.githubusercontent.com/77790231/139101961-55f79a46-31ad-4918-8cdf-911e42d49f4d.png">
which we can now see some information about the file. whats cool about this is you can see the decimal and hexadecimal for each part of the file we just scanned you can also 
scan for more spefic stuff like common executable opcode signatures which you can do with 'binwalk -A file' which if you were to do it with our current file would look something like this.
<img width="960" alt="binwalk4" src="https://user-images.githubusercontent.com/77790231/139102947-fb322bb4-c45f-4e1d-a2cb-a3ba27836baf.png">
this is information in which can be used maybe along with another program to make use of it fully but this is a solid barebones way of getting this info you can also set include and exclude if you have more experience and are searching for spefics even more and this can be used to help reverse engineer firmware.
Now along with scanning for executable code and instructions as we have seen before you can also use 'binwalk -m file' to see how many signatures and time your file your looking at has been signed or if it has been signed at all.
<img width="955" alt="binwalk5" src="https://user-images.githubusercontent.com/77790231/140069929-5afac9fb-a892-404d-8cc3-18a3acf5c58a.PNG">
As we can see here our file has 411 signatures and we can see the time it took to scan it but more importantly we get an MD5 checksum which as we all know we can use in order to verify the file.

Ben Weddle - Entropy Options

Christopher Burke - Extraction Options
If you've ever wanted to play around with firmware, Binwalk's extraction options are quite useful. I will be using the firmware off of the same model router as the one I have at home. You can follow along by getting the file from [here](https://www.tp-link.com/en/support/download/archer-ax1500/#Firmware) The hash of the zipped file is da8edf2e81cfdfeb328207ff3f30a4a1d2733bc2fd03102bc445bd799c5f1d93. Unzip it, and you'll be left with the .bin file (I renamed the .bin file to ax1500.bin for shorter commands). After that, open your terminal in Kali and navigate to the folder the file is in. Once there, we can start extracting files from the firmware with the most basic of extraction options. Type 'binwalk -e ax1500.bin'. <img width="960" alt="binwalk4" src="https://github.com/MerelyChris/ExtractionImages/blob/main/extractImage1.JPG"> It will display the information seen in the image above, and also add a folder containing the extracted files. As seen here. <img width="960" alt="binwalk4" src="https://github.com/MerelyChris/ExtractionImages/blob/main/extractImage2.JPG"> Next, we'll search through the file and extract recursively through it. To do so, we'll use the matryoshka option '-M' as well as removing any files that couldn't be extracted or resulted empty files with '-r'. Type 'binwalk -Mre ax1500.bin'. <img width="960" alt="binwalk4" src="https://github.com/MerelyChris/ExtractionImages/blob/main/extractImage3.JPG"> This, unsurprisingly, leaves us with a much longer terminal print out and a few more extracted files to view. The last option we'll explore is the '-D' option. This allows us to specify file types to extract from the firmware, which is probably the most useful option for this course. To do so, we'll type 'binwalk -D 'png image:png' ax1500.bin'. <img width="960" alt="binwalk4" src="https://github.com/MerelyChris/ExtractionImages/blob/main/extractImage4.JPG"> As seen, this greatly decreases the amount of files you may not care about. <img width="960" alt="binwalk4" src="https://github.com/MerelyChris/ExtractionImages/blob/main/extractImage5.JPG"> The options covered are the main options of extraction, but there are additional, useful options in the link at the bottom of this README. Some useful sites I used were [this](https://en.kali.tools/?p=1634) and [this](https://embeddedbits.org/reverse-engineering-router-firmware-with-binwalk/).

Orion Radford - Raw Compression Options



This is a helpful [link](https://tools.kali.org/forensics/binwalk) when looking for commands for binkwalk.
