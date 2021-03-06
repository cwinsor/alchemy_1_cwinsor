
xxxxxxxxxxxxxxxxxx
# One-time Setup
These are steps to stand up the Ubuntu host, access desktop using VNC port forwarding,
basic config, and populate the Alchemy project from cwinsor github.

The primary value add is time savings - all the details of 
Ubuntu config and setting up VNC port forwarding... lots of time.

## Steps:
There are two ways to run Ubuntu: AWS EC2 Ubuntu host and VNC, Windows 10 WSL (Windows Subsystem for Linux) Ubuntu.
We only have instructions for EC2, but WSL on laptop should be a subset of this.

### via AWS/EC2:
The following are commands to set up an AWS EC2 running Ubuntu with access to gnome desktop via VNC port forwarding.

* Open an AWS account.

* On AWS: start an Ubuntu 20 EC2 Server.  t3.2xlarge is what I used and
may be overkill.  When actively used it can be $20/day - of course
like any EC2 you need to turn it off when not using it!

* Starting the server provides .pem key for access.

* On laptop: enable WSL (Windows Subsysem for Linux).  Refer to Microsoft for detailed steps.

* On laptop: start a WSL shell and demonstrate basic SSH access:
  * ssh -i SomeKey.pem   username@192.168.123.456
* On laptop: for future reference a basic SCP (copy) would be:
  * scp some_file.zip  username@192.168.123.456:some_file.zip
 * On AWS: install tightVNC
  * sudo apt update
  * sudo apt install xfce4 xfce4-goodies gh
  * this will ask gdm3 or lightdm... lightdm worked for me
  * sudo apt install tightvncserver
  * vncserver  (this is done only to establish an initial ~.vnc/xstartup)
  * set the password
* You need to edit ./vnc/xstart based on the type of desktop.  An example xstartup below assumes xface (the default desktop).  You can also check what desktop you have via "grep Exec= /usr/share/xsessions/*.desktop" 
  * cd .vnc
  * vi xstartup
  * contents of xstartup for xface:
```
#!/bin/sh
DESKTOP_SESSION=xfce
export DESKTOP_SESSION
startxfce4
vncserver-virtual -kill $DISPLAY
re-start vncserver:
vncserver -kill :1
vncserver :1
```

* vncserver -kill :1 
* vncserver
* On the laptop - start a second WSL shell and use it to run port forwarding (port 5901 is tightVNC)
  * ssh -L 5901:127.0.0.1:5901 -C -N -l username 192.168.2.110
* or with pem file...
  * ssh -i pem_blah_blah.pem -L 5901:127.0.0.1:5901 -C -N -l ubuntu 3.17.191.16

* On laptop: install VNC viewer.  This involves setting up a VNC account.
  * Use the VNC viewer to connect specifying localhost:5901 - this will be forward to the IP above (port forwarding)
   localhost:5901

* At this point you should have access to ubuntu gnome shell on the AWS host.

* To install Chrome:
  * download it using this command: wget https://dl.google.com/linux/direct/google-chrome-stable_current_amd64.deb
  * execute the downloaded installer: sudo apt install ./google-chrome-stable_current_amd64.deb
  * launch the browser: google-chrome
  * To make the default browser icon to launch google chrome:
  * run xfce4-settings-manager
  * find "Preferred Applications"
  * under "Web Browser", click "Other..."
  * type in /usr/bin/google-chrome

* Installing g++, gdb, make, etc etc...
* Alchemy requires make, bison, flex, g++, perl.  The IDE will additionally require gdb.
* The majority of this is achieved via "build_essentials:
  * sudo apt-get update
  * sudo apt-get install build-essential
* then additionally:
  * sudo apt-get install gdb
  * sudo apt-get install bison
  * sudo apt-get install flex
  * sudo apt-get install perl

* Confirm the parts/pieces work:
  * g++ --version
  * gdb --version
  * make --version
  * bison --version
  * flex --version
  * perl --version

* Install an IDE.  Here I recommend CLion from IntelliJ. There is a 29 day trial but an IDE that works on Linux is definately worth a couple bucks.
* Download and follow the readme to install and start CLion
* Start a project taking the default workspace folder - this will create a folder ~/CLionProjects

* At this point you need the Alchemy source.  Here I am working with Alchemy classic (Version 1). You can pull the tar from U-Washington but note that may not compile (see bottom) it is at http://alchemy.cs.washington.edu/alchemy1.html    The other option is to git pull from https://github.com/cwinsor/alchemy_1_cwinsor.git  where I have fixed those compile errors.  To get from cwinsor repo:
  * cd ~/CLionProjects
  * mkdir alchemy_101
  * cd alchemy_101
  * git pull https://github.com/cwinsor/alchemy_1_cwinsor.git
  * Return to the CLion IDE and create new project pointing to source folder above.


Failure to compile:
```
ubuntu@ip-172-31-29-142:~/code_alchemy/alchemy_1a/src$ g++ --version
g++ (Ubuntu 7.5.0-3ubuntu1~18.04) 7.5.0
Copyright (C) 2017 Free Software Foundation, Inc.
This is free software; see the source for copying conditions.  There is NO
warranty; not even for MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.

ubuntu@ip-172-31-29-142:~/code_alchemy/alchemy_1a/src$ make depend
updating the dependencies for:
     ../src/util/arguments.cpp ../src/util/powerset.cpp ../src/util/random.cpp ../src/parser/fol.cpp ../src/parser/listobj.cpp ../src/logic/predicatetemplate.cpp ../src/logic/functiontemplate.cpp ../src/logic/function.cpp ../src/logic/predicate.cpp ../src/logic/term.cpp ../src/logic/domain.cpp ../src/logic/clause.cpp ../src/logic/clausesampler.cpp ../src/infer/groundclause.cpp ../src/learnwts/learnwts.cpp ../src/infer/infer.cpp ../src/learnstruct/learnstruct.cpp ../src/logic/Polynomial.cpp ../src/logic/logichelper.cpp ../src/infer/inferutil.cpp ../src/infer/createhypercube.cpp ../src/infer/createhypercubebasic.cpp ../src/infer/createhypercubedt.cpp ../src/infer/joinhypercube.cpp ../src/infer/refinehypercube.cpp ../src/infer/superpred.cpp ../src/infer/superclause.cpp ../src/infer/node.cpp ../src/infer/factor.cpp
ubuntu@ip-172-31-29-142:~/code_alchemy/alchemy_1a/src$ make
g++ -O3 -DNDEBUG -static  -finline-limit=100000 -Wno-deprecated -Wall -I. -I../src/util -I../src/logic -I../src/parser -I../src/learnwts -I../src/learnstruct -I../src/infer -o ../bin/obj/learnwts.o -c ../src/learnwts/learnwts.cpp
In file included from ../src/learnwts/learnwts.cpp:70:0:
../src/util/arguments.h:223:43: warning: ISO C++ forbids converting a string constant to `char*' [-Wwrite-strings]
   static ArgsRetCode parseFromFile(char*f="argsFile");
                                           ^~~~~~~~~~
In file included from ../src/infer/inferutil.h:74:0,
                 from ../src/infer/inferenceargs.h:70,
                 from ../src/learnwts/learnwts.cpp:71:
../src/util/array.h: In member function `void Array<Type>::shuffle()':
../src/util/array.h:371:51: error: cannot call member function `int Random::randomOneOf(const int&)' without object
       int swapwith = Random::randomOneOf(numItems_);
```
