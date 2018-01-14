## Transcoding video on Windows, the Don Melton way

Don Melton provides some [amazing scripts](https://github.com/donmelton/video_transcoding) that wrap [HandBrake](https://handbrake.fr/) to produce high quality, portable versions of Blu-ray rips. The tools Don provides are written in [Ruby](https://www.ruby-lang.org/en/), and they're packaged as a [Ruby Gem](https://rubygems.org/). All the tools are cross platform, requiring only the Ruby runtime to be installed. But they do depend on external programs to get anything useful done. These dependencies are cross platform as well, but Windows has no standard package management system (de facto or otherwise), so installing these dependencies for Windows can be a bit of a pain. But it is possible! There are several ways to go about installing these tools and their dependencies, the two discussed here are to use [Bash on Ubuntu on Windows](https://msdn.microsoft.com/en-gb/commandline/wsl/about) (which is what we recommend for users of Windows 10), but if you have an older version of Windows you can also download Windows versions of everything that's needed.

### Windows 10
If you're using an up-to-date version of Windows 10, you have access to "[Bash on Ubuntu on Windows](https://msdn.microsoft.com/en-gb/commandline/wsl/about)". This feature allows you to run command line Linux programs on Windows without using a virtual machine, and it also gives you access to Ubuntu's extensive package archives. Everything you need to run these scripts is easily installable with apt-get.

1. First up, you need to activate Bash.
    1. Open Settings then go to `Update & Security -> For Developers`
    1. Change your computer to "Developer Mode"
    1. Click `Start` then look for `Turn Windows features on or off`
    1. Select `Windows Subsystem for Linux (beta)`
    1. Click ok. You'll need to restart your PC
    1. Now open up `cmd.exe` and run `bash`. Follow the instructions then bash is set up and ready to go 
    1. When you're asked to create a username and password you can choose whatever you like, it doesn't have to match your Windows username and password
1. Next you install the dependencies for `video_transcoding`. Inside of bash:
    1. Run `sudo apt-get update && sudo apt-get upgrade` to ensure everything's up to date
    1. Run `sudo apt-get install ruby ffmpeg mp4v2-utils mkvtoolnix`
    1. Install HandBrakeCLI
        1. `sudo add-apt-repository ppa:stebbins/handbrake-releases`
        1. `sudo apt-get update`
        1. `sudo apt-get install handbrake-cli`
1. Now you can install `video_transcoding`
    1. `sudo gem install video_transcoding`

The `detect-crop` command has an optional dependency on `mpv` for previewing crops. You can't launch graphical Linux apps using "Bash on Ubuntu on Windows", but it is capable of launching Windows programs; this means you can install `mpv` for Windows, then use an alias to launch it from bash:

1. Create `C:\bin` and [add it to your path](#adding-a-folder-to-your-path)
1. Download a Windows build of [mpv](https://mpv.io/) from [here](https://mpv.srsfckn.biz/)
1. Extract it to `C:\bin`
1. Create an alias for mpv:
    `echo "alias mpv=mpv.exe" >> .bashrc`
1. Reload `.bashrc` by running `source ~/.bashrc` 

If you have your movie files in the Windows file system (so NOT in `~userfolder/AppData/Local/lxss/home`), for example on your desktop, you have to provide the path to it via `/mnt/[drive letter]/path/to/your/file/location`. You can also navigate to the folder in Explorer, then type bash in the address bar to open a bash instance to that location

### Pre Windows 10 Creators Update and earlier
In old versions of Windows 10, or in Windows 8.1 and earlier, you either don't have access to Bash on Ubuntu on Windows or the version that's there can't be used to do this. All the dependencies are available for Windows though, you just need to track them down:

- [HandBrakeCLI](https://handbrake.fr/downloads2.php)  
    Get the latest stable version
- [ffmpeg](https://ffmpeg.org/download.html), the download page for Windows is [here](http://ffmpeg.zeranoe.com/builds/)  
   Get the latest stable 'static' release.
- [mkvtoolnix](https://mkvtoolnix.download/downloads.html#windows), the download page for Windows is [here](https://www.fosshub.com/MKVToolNix.html)  
    Download the latest portable version. It's packaged as a .7z file, I'd recommend using [7zip](http://www.7-zip.org/download.html) to open these files
- [mpv](https://mpv.io/), the download page for Windows is [here](https://mpv.srsfckn.biz/)  
    Get the latest version
- [MP4v2](https://code.google.com/archive/p/mp4v2/)  
    Functional Windows binaries for this are hard to find, I've been using [this one](http://forum.doom9.org/showthread.php?t=171038). If anyone knows of a more official binary, please [file an issue](https://github.com/JMoVS/installing_video_transcoding_on_windows/issues/new) or [tweet at me](https://twitter.com/_samhutchins/)
- [RubyInstaller](https://rubyinstaller.org/downloads/)

1. Download everything from the links above
1. Run the Ruby installer and make sure "Add to path" is ticked during install
1. Create `C:\bin` and [add it to your PATH](#adding-a-folder-to-your-path)
1. From the HandBrake zip file extract `HandBrakeCLI.exe` and the `fonts\` directory to `C:\bin`
1. From the ffmpeg zip file extract `bin\ffmpeg.exe` to `C:\bin`
1. From the MKVToolNix 7z file extract `mkvpropedit` and the `data\` directory to `C:\bin`
1. From the MP4v2 7z file extract `libmp4v2.dll` and `mp4track.exe` to `C:\bin`
1. From the mpv 7z file extract `mpv.exe` and `d3dcompiler_43.dll` to `C:\bin`  
    `C:\bin` should now contain:
    - data\
    - fonts\
    - d3dcompiler_43.dll
    - ffmpeg.exe
    - HandBrakeCLI.exe
    - libmp4v2.dll
    - mkvpropedit.exe
    - mp4track.exe
    - mpv.exe
1. Open `cmd.exe` and run `gem install video_transcoding`

### Adding a folder to your PATH
1. Right click `Start`
1. Open Control Panel
1. Search for "Path"
1. Click "Edit System Environment Variables"
1. Click the "Environment Variables" button in the lower right corner
1. In the System Variables box find "PATH" and select it
1. Click the "Edit" button

## Further information
### Batch control in `cmd.exe`
Batch control can be achieved with the following .bat file:
```
@echo off

setlocal EnableDelayedExpansion

set work=%cd%
set queue=%work%\queue.txt
set crops=%work%\Crops

for /F "tokens=*" %%I in (queue.txt) do (
    set title_name=%%~nI
    set crop_file=%crops%\!title_name!.txt

    if exist !crop_file! (
        for /F "tokens=* USEBACKQ" %%F in (`type "!crop_file!"`) do set crop_option=--crop %%F
    ) else (
        set crop_option=
    )

    call transcode-video !crop_option! "%%I"
)
```

It works in much the same way that Don's [bash script](https://github.com/donmelton/video_transcoding#batch-control-for-transcode-video) works, although it does have the limitation of not being able to "resume" the queue in the way that Don's does. I would recommend creating a `batch-transcode.bat` file in your `C:\bin` folder, then you can create the necesarry file structure anywhere:
```
Crops\
queue.txt
```

In CMD you can `CD` to that directory, populate the `queue.txt` file then simply call `batch-transcode` and it'll transcode everything in the queue

To create a `queue.txt` with every mkv file in a directory you can use the following command:  
`for %a in (*.mkv) do echo %~fa >> queue.txt`
