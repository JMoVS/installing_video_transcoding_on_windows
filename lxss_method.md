# How to get to a working setup of the [video_transcoding scripts](https://github.com/donmelton/video_transcoding) on windows 10's subsystem for linux

## What needs to be done

1. Activate the [Windows Subsystem for Linux](https://msdn.microsoft.com/en-gb/commandline/wsl/install_guide)
1. Install the dependencies for `video_transcoding`
1. Install `video_transcoding`

## Activating the linux subsystem for windows

To begin, open Settings and go to Update & Security -> For Developers -> and change your computer to "Developer Mode".

Head into your search bar in Windows and look for "windows-features" and you should find a control panel link getting you to a nice window where you can activate the "Windows-Subsystem for Linux (Beta)". When this is done, start your Ubuntu shell (if you don't immediately see a link, search for "Ubuntu" in the search bar) and follow the instructions. When you're done, you should run

```
sudo apt-get update && sudo apt-get upgrade
```

to get your system up to date. 


## Installing dependencies
```
sudo apt-get install ruby ffmpeg mp4v2-utils mkvtoolnix
```

The easiest way to obtain handbrake is from their ppa page, this means:
```
sudo add-apt-repository ppa:stebbins/handbrake-releases
sudo apt-get update
sudo apt-get install handbrake-cli
```

The Windows Subsystem for Linux can't launch graphical Linux applications, but as of the [Windows Creators Update](https://blogs.windows.com/windowsexperience/2017/04/11/whats-new-in-the-windows-10-creators-update/#hjT88QBz4geTArU4.97) it can launch Windows binaries. This means we can install the Windows version of [MPV](https://mpv.io/) then use an alias to launch it from Bash:

- Create `C:\bin` and [add it to your PATH](https://github.com/JMoVS/installing_video_transcoding_on_windows/blob/master/native_method.md#adding-a-folder-to-your-path-in-windows-10)
- Go download [mpv](https://mpv.io/) from [here](https://mpv.srsfckn.biz/)
- Extract it to `C:\bin`
- In Bash create an alias for mpv:
    ```
    echo "alias mpv=mpv.exe" >> .bash_profile
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
