# How to get to a working setup of the [video_transcoding scripts](https://github.com/donmelton/video_transcoding) on windows 10's subsystem for linux

_With the upcoming (aka Spring win 10 update), this process will get a lot easier as some things like patchelf and other things are getting fixed, obleviating the need for some workarounds still present in this gist_

## How to update when you successfully installed all this

```
sudo apt-get update && sudo apt-get upgrade
brew update && brew upgrade
sudo gem update video_transcoding
```

## Rationale

I wanted a way I could provide my father on his windows 10 machine to sort of easily transcode the videos and I was never able to work with Cygwin (and also didn't research deeply into it). Coincidentally, Microsoft announced and shipped the linux subsystem for windows, so I thought, let's use this.

## What needs to be done

We essentially want just the dependencies to work, that are listed on the video_transcoding github page. Those are:

- Ruby 2.x
- HandBrake-CLI
- ffmpeg
- mkvtoolnix
- mp4v2

## What we will get

- some dependencies installed via apt-get
- some installed dependencies installed via linuxbrew (As it often has current versions compared to repos)
- video_transcoding installed via ruby gem (comparable to original manual)

## Activating the linux subsystem for windows

To begin, go to security&updates -> for developers -> and change your computer to "developer mode".

Head into your search bar in windows and look for "windows-features" and you should find a control panel link getting you to a nice window where you can activate the "Windows-Subsystem for Linux (Beta)". When this is done, start your ubuntu shell (if you don't immediatly see a link, search for "ubuntu" in the search bar) and follow the instructions. When you're done, you should run

```
sudo apt-get update && sudo apt-get upgrade
```

to get your system up to date. 

## Let's first ~~collect~~ install all the ruby versions's we need

```
sudo apt-add-repository ppa:brightbox/ruby-ng
sudo apt-get update
sudo apt-get install ruby2.4
sudo apt-get install ruby1.9
```

and select ruby version __2.4__ with 

`sudo update-alternatives --config ruby`

## Getting linuxbrew brewing

get the dependencies right:

```
sudo apt-get install build-essential curl git python-setuptools m4
```

and then actually install, as documented on the [linuxbrew](https://linuxbrew.sh) website in the "Install Linuxbrew (tl;dr)" section.

Proceed, when you have a partially working brew system. _Note:_ You may have to insert a

```
. .bash_profile
```

inside your .bashrc, because on lxss, right now, .bash_profiles seem to be ignored.

You also will want to have your .bash_profile to look something alike:

```
export PATH="/home/justin/.linuxbrew/bin:$PATH"
export MANPATH="/home/justin/.linuxbrew/share/man:$MANPATH"
export INFOPATH="/home/justin/.linuxbrew/share/info:$INFOPATH"
export HOMEBREW_BUILD_FROM_SOURCE=1
export EDITOR="/usr/bin/nano"
export HOMEBREW_VERBOSE=1
```

I set the `verbosity`and `build_from_source`, because bottles currently don't work on lxss and installing something without verbose when compiling often makes it seem stuck when in reality, it is just still compiling.

## The fun part with linuxbrew

Before you start, we need to trick a little:
```
sudo update-alternatives --config ruby
```

select the 1.9 version now. Now enter `brew config`  - a lot of text from portable ruby should scroll through. We thereby force homebrew to use the portable ruby version isntead of the system one. After you've done that, issue `sudo update-alternatives --config ruby` again and change back to 2.4. 

Now you should be able to run

```
brew update && brew upgrade
```

Some build dependencies depend on some other tools, that aren't marked as dependencies in them and you also need some things to get the ball rolling on lxss:

```
brew install m4 xz gettext gcc ffmpeg mkvtoolnix mp4v2
```

Now grab a cup of coffee or tee or go in the Biergarten and drink a Maß and come back ~~later~~ the next day, it'll take some LONG time. ;-)

## We need handbrake

The easiest way to obtain handbrake is from their ppa page, this means:
```
sudo add-apt-repository ppa:stebbins/handbrake-releases
sudo apt-get update
sudo apt-get install handbrake-cli
```

## Install video_transcoding

Nothing special here, just run:

```
sudo gem install video_transcoding
```

## Caveats

If you have your movie files in the windows file system (so NOT in ~userfolder/AppData/Local/lxss/home...), for example on your desktop, you have to provide the path to it via `/mnt/[drive letter]/path/to/your/file/location`. It then should work.

I hope this works. If it doesn't, feel free to contact me on twitter @JMoVS or comment below.

This readme was originally a gist: https://gist.github.com/JMoVS/75f3c6b344648deef59bc761e5e5a0e6 - there are some good comments over there.
