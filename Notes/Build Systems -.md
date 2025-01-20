Definitive Guides -
[Makefiles, but in English - YouTube](https://www.youtube.com/watch?v=FfG-QqRK4cY)
[Makefiles: 95% of what you need to know - YouTube](https://www.youtube.com/watch?v=DtGrdB8wQ_8)
[Makefiles in Python For Professional Automation - YouTube](https://www.youtube.com/watch?v=Yt-UF7fNLJE)
## Build systems
A build system's goals are to 
- **organize the magical incantations:** automate the compilation and linking of source files into executables 
- **process only what's necessary:** recompile only the changed portion of the source code, and the portion dependent on changed code 
- **make maintaining the build system easier**: the build system should be a programming language that allows you to avoid redundant (copied) code 
To motivate and give examples of each of these, 
see ['01_build_script'](https://github.com/gparmer/evening_os_hour/tree/master/f19/10.2-makefiles/01_bu ild_script) build - This shows how to use a shell script to encode the magical incantations of your build.
This has a number of downsides including repetition within the script, and the fact that it can perform a single function. 
You could make the script arbitrarily complex and add arguments to do multiple operations (e.g. clean and gemu in 'xv6"), but now you're writing a build system, and it won't track modified files and only rebuild them.