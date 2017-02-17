# Installation

## Downloads
 - [HandBrakeCLI](https://handbrake.fr/downloads2.php)  
   Get the latest stable version
 - [ffmpeg](https://ffmpeg.org/download.html), the download page for Windows is [here](http://ffmpeg.zeranoe.com/builds/)  
   Get the latest stable 'static' release.
 - [mkvtoolnix](https://mkvtoolnix.download/downloads.html#windows), the download page for Windows is [here](https://www.fosshub.com/MKVToolNix.html)  
   Download the latest portable version. It's packaged as a .7z file, I'd recommend using [7zip](http://www.7-zip.org/download.html) to open these files
 - [mpv](https://mpv.io/), the download page for Windws is [here](https://mpv.srsfckn.biz/)  
   Get the latest version
 - [MP4v2](https://code.google.com/archive/p/mp4v2/)  
   Functional Windows binaries for this are hard to find, I've been using [this one](http://forum.doom9.org/showthread.php?t=171038). If anyone knows of a more official binary, please let me know
 - [RubyInstaller](https://rubyinstaller.org/downloads/)

## Steps
 1. Run the RubyInstaller to install Ruby and be sure to have "Add to path" ticked when you install.
 2. Create `C:\bin` and [add it to your PATH](#adding-a-folder-to-your-path-in-windows-10)
 3. From the Handbrake zip file extract `HandBrakeCLI.exe` to `C:\bin`
 4. From the ffmpeg zip extract `bin/ffmpeg.exe` to `C:\bin`
 5. From the MKVToolNix 7z extract `mkvpropedit.exe` and the `data` directory to `C:\bin`
 6. From the MP4v2 7z extract `libmp4v2.dll` and `mp4track.exe` to `C:\bin`
 7. From the mpv 7z extract `mpv.exe` and `d3dcompiler_43.dll` to `C:\bin`  
    `C:\bin` should now contain:
     - data/
     - d3dcompiler_43.dll
     - ffmpeg.exe
     - HandBrakeCLI.exe
     - libmp4v2.dll
     - mkvpropedit.exe
     - mp4track.exe
     - mpv.exe
 
 8. Open CMD and type `gem install video_transcoding`

Be sure to give the [README](https://github.com/donmelton/video_transcoding/blob/master/README.md) a thorough read, as it contains an enourmous amount of information about how to use the tools.

### Adding a folder to your PATH in Windows 10
 - Right click start menu
 - Open Control Panel
 - Search "Path"
 - Click "Edit system environment variables"
 - Click "Environment variables" buttin in lower right of the new window
 - In the system variables box find path, click to select it
 - Click "Edit" button

# Further information
## Batch control
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
