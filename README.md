# DirtyUnicorns Project #
[<center><img src="https://lh3.googleusercontent.com/-p9S_rIap_No/V9iIHcrU1_I/AAAAAAAAEPk/Mba0HIFDvR8oE_1hmfj0SGWqlx561mZBwCL0B/w971-h547-n-no/12.png" height="100%" width="100%;"/></center>](https://github.com/dirtyunicorns)

#######################################
#  Step One: set up your environment  #
#######################################

-- Grab Java 8:
$ sudo apt-get update
$ sudo apt-get install openjdk-8-jdk
$ sudo apt-get install openjdk-8-jre

-- Install build tools
$ sudo apt-get install git-core gnupg flex bison gperf build-essential zip curl \
zlib1g-dev gcc-multilib g++-multilib libc6-dev-i386 lib32ncurses5-dev \
x11proto-core-dev libx11-dev lib32z-dev ccache libgl1-mesa-dev libxml2-utils \
xsltproc unzip

NOTE: Arch Linux users, use the Arch wiki page to install your packages then
come back here: https://wiki.archlinux.org/index.php/Android#Building_Android

######################################
#  Step Two: Configure Repo and Git  #
######################################

NOTE: If you have any problems with the below commands, try running as root:
$ sudo -s

Git is an open source version control system which is incredibly robust for
tracking changes across repositories. Repo is Google's tool for working with Git
in the context of Android. More reading if you are interested:
https://source.android.com/source/developing.html

Run these commands to get repo all working (only needed if you did the manual
method of setup above):
$ curl https://storage.googleapis.com/git-repo-downloads/repo > repo
$ chmod a+x repo
$ sudo install repo /usr/local/bin
$ rm repo

Then run these commands to get git all working:
$ git config --global user.name "Your Name"
$ git config --global user.email "you@example.com"

#####################################
#  Step Three: Download the source  #
#####################################

First, create a folder for your source:
$ mkdir ~/<foldername> (eg. mkdir ~/DU or ~/PN-Layers)
$ cd ~/<foldername>

When you go to build a ROM, you must download its source. All, if not most,
ROMs will have their source code available on Github. To properly download the
source, follow these steps:
-- Go to your ROM's Github (e.g. http://github.com/DirtyUnicorns)
-- Search for a manifest (usually called manifest or android_manifest).
-- Go into the repo and make sure you are in the right branch (located right
   under the Commits tab).
-- Go into the README and search for a repo init command. If one exists, copy
   and paste it into the terminal and hit enter.
-- If one does not exist, you can make one with this formula:
   repo init -u <url_of_manifest_repo>.git -b <branch_you_want_to_build>.
   For example:
      $ repo init -u https://github.com/DirtyUnicorns/android_manifest.git -b o8x
-- After the repo has been initialized, run this command to download the source:
   $ repo sync --force-sync -j$( nproc --all )
-- This process can take a while depending on your internet connection.


##########################
#  Step Four: Build it!  #
##########################

Here's the moment of truth... time to build the ROM! This process could take
15 minutes to many hours depending on the speed of your PC but assuming that
you have followed the instructions so far, it should be smooth sailing.

Before you compile, you may also consider setting up ccache if you can spare
about 50GB. If not, skip this section. ccache is a compiler cache, which keeps
previously compiled code stored so it can be easily reused. This speeds up
build times DRASTICALLY.

Open your bashrc or equivalent:
$ nano ~/.bashrc
- Append export USE_CCACHE=1 to the end of this file
   then hit ctrl-X, Y, and enter.
$ source ~/.bashrc

After that, run this command if you used the manual method of setup above
$ prebuilts/misc/linux-x86/ccache/ccache -M 50G (or however much you want).
Run this command if you used the automatic method of setup above
$ ccache -M 50G

After this, load up the compilation commands:
$ . build/envsetup.sh

Then, tell it which device you want to make and let it roll:
$ lunch du_gemini-userdebug
$ brunch gemini

NOTE: Some ROMs may use their own bacon command, read their manifest as they
will usually outline this.
Others may not use brunch, use mka or make -j$( nproc --all )

If you get an error here, make sure that you have downloaded all of the
proper vendor files from your ROM's repository.

Once you tell it brunch gemini, the computer will start building. You
will know that it is done when you see a message saying make completed and
telling you where your flashable zip is located.

Whenever you build again, make sure you run a clean build every time by placing
this command in between the other two:
$ make clean

That's it! You successfully compiled a ROM from scratch :) By doing this, you
control when you get the new features, which means as soon as they are available
on Github. After this, you may even want to start contributing, one of the
greatest things about Android and open source software in general.


#################
#  Jack issues  #
#################

For those of you who are having jack issues (like saying you ran out of memory),
follow these steps.

Type this into your terminal, substituting the # with how many GBs of RAM
you have:
$ export JACK_SERVER_VM_ARGUMENTS="-Dfile.encoding=UTF-8 -XX:+TieredCompilation -Xmx#g"

Then go into the root of the source folder and type the following:
$ ./prebuilts/sdk/tools/jack-admin kill-server
$ ./prebuilts/sdk/tools/jack-admin start-server

This will restart the jack server to reflect your new heap limit.


#############
#  Support  #
#############

If you have any questions, you can reach out to me via one of the following:
- Telegram (https://t.me/du_gemini)

I may not respond right away but I will do my best to help you out. However,
I do expect you to put effort into learning this process, I am not here to hold
your hand. Please do not message me if you are building a ROM for a device
unofficially, I own a Nexus device, which means I never have to worry about
working with other's device tree. I have no knowledge that will help you and
I will ignore your message.


####################
#  Special thanks  #
####################

@Mazda-- for spearheading the Dirty Unicorns project, his dedication to
open source, and helping me out at random points during compilation.

* [**DirtyUnicorns**](https://github.com/DirtyUnicorns)
* [**AEXmod**](https://github.com/AEXmod)
* [**LineageOS/Cyanogenmod**](https://github.com/LineageOS)

# Copyright (C) 2016-2017 Nathan Chancellor