# Transcoding video on Windows, the Don Melton way
Don Melton provides some [amazing scripts](https://github.com/donmelton/video_transcoding) that wrap [HandBrake](https://handbrake.fr/) to produce high quality, portable versions of Blu Ray rips. The tools Don provides are written in [Ruby](https://www.ruby-lang.org/en/), and they're packaged as a [Ruby Gem](https://rubygems.org/). All the tools are cross platform, requiring only the Ruby runtime to be installed. But they do depend on external programs to get anything useful done. These dependencies are cross platform as well, but Windows has no standard package management system (de facto or otherwise), so installing these dependencies for Windows can be a bit of a pain. But it is possible! There are several ways to go about installing these tools and their dependencies. Downloading binaries manually, or using the Linux Subsystem for Windows are two such options


## The two methods:
- [manually installing the binaries](https://github.com/JMoVS/installing_video_transcoding_on_windows/blob/master/native_method.md)
- [installing it in the linux subsystem for windows](https://github.com/JMoVS/installing_video_transcoding_on_windows/blob/master/lxss_method.md)
