# Transcoding video on Windows, the Don Melton way
Don Melton provides some [amazing scripts](https://github.com/donmelton/video_transcoding) that wrap [HandBrake](https://handbrake.fr/) to produce high quality, portable versions of Blu Ray rips. The tools Don provides are written in [Ruby](https://www.ruby-lang.org/en/), and they're packaged as a [Ruby Gem](https://rubygems.org/). All the tools are cross platform, requiring only the Ruby runtime to be installed. But they do depend on external programs to get anything useful done. These dependencies are cross platform as well, but Windows has no standard package management system (de facto or otherwise), so installing these dependencies for Windows can be a bit of a pain. But it is possible! There are several ways to go about installing these tools and their dependencies. Downloading binaries manually, or using the Linux Subsystem for Windows are two such options


## The two methods:
- [manually installing the binaries](https://github.com/JMoVS/installing_video_transcoding_on_windows/blob/master/native_method.md)
- [installing it in the linux subsystem for windows](https://github.com/JMoVS/installing_video_transcoding_on_windows/blob/master/lxss_method.md)

## Notes about the two methods:

### Manually installing binaries

This option is recommended for most users. You will get a working system with all the binaries in place that will do the job. It won't be able to be updated easily though - meaning that you would have to hunt down newer versions of the dependencies manually and install them manually. As the dependencies aren't updated that often, this shouldn't really be a problem.

### Installing it in the linux subsystem for windows

This method is bleeding edge as in: it is quite cumbersome and very experimental right now. The whole subsystem is in beta status (and will be for the foreseeable future). The spring windows 10 update is supposed to fix some issues with it so that the need to self-compile everything will be gone, further streamlining and simplifying the process.
This guide is for the adventurous. It will result in a system that you can easily update (as detailed in the guide) but is hard to set up right now. It is also unclear whether it breaks with an update easily. Not really recommended until the spring update has landed for wide availability.
