# Personal Details
+ Name: Vladislav Babkin
+ University: Dnipropetrovsk National University (DNU), Ukraine
+ Degree Program: B. Sc. 
+ Course: Applied mathematics, System Analysis
+ Email: vladiksonic@mail.ru, vladiksonic@gmail.com
+ Website: hotab1994.wordpress.com (Decided to start a blog rather recently, it is totally empty right now)
+ LinkedIn: [Hotab](https://ua.linkedin.com/in/hotab/en)

# Availability and time

+ I plan to spend around 20-30 hours per week. Maybe more when possible (I will be working on a startup in parallel)
+ I will have my exams in university in the beginning of June for 1 or 2 weeks - I won't be able to spend as much time in this period - thus I intend to start maybe before May 25 to compensate. I think that by August 24 most (95%+) of the work will be complete.
+ I plan to support this project after GSoC, as well.

# Project Description
[Develop a kpatch delivery mechanism](http://wiki.centos.org/GSoC/2015/Ideas#kpatch)

After looking through the proposal and discussing it with the mentor, I came to a conclusion that this should be a unified tool written in python with its own repo, a tool to load the plugin queue (more about it later), and a yum plugin, which will hook to kernel updates (optionally). It is so, because:

+ python is installed by default in CentOS, and thus, is available on every machine. It is slow, but for one-off tasks like updating and installing - it is perfect. It will give us ability to implement platform-independent tools to work with the patches (pull them, update them, etc) - most of the work will be done here.
+ having a yum plugin automatically refreshing patches on kernel updates will allow us to partially fight the issue with updates to the kernel without updating the plugins, but might impose a function the user does not want - thus it should be made optional.
+ also, we can add a virtual package to the system to let the patches autoupdate on yum update with the plugin - this is a more clean solution of the previous point, but it will not work on kernel-only updates. We need to capture the event of the kernel update to also match these.

I can see this structure of a kpatch recipe:

+ Metafile - we will store the targeted kernels, architecture (and so on) here, and we will also do the versioning through this file. More importantly - metafile will list the functions the patch uses. This will allow us to add control over incompatible patches - the ones that patch the same function should never be activated together.
+ Description file - longer description of what exactly the patch does and what instabilities it might bring - to warn the user beforehand.
+ Patch files for each kernel version and each architecture, named in some format (for example: ```version_architecture```) - these should not take much space.
+ If the patch needs some additional steps to use - ```pre_build.sh``` file.
+ ```post_build.sh``` file to test the stability/functioning after installation.
+ Any other info the patch needs.

_Note_: The repo manager will need a way to obsolete patches, for example: patch A and patch B both patch function A. Then patch C unifies them, and thus - can obsolete both of them. If this is not done - the user will have additional steps to remove the patches, which is rather dirty. This info can be stored in the metafile.

Core script would be able to:
+ Pull/obsolete/[any other meaningful operation] the patches
+ (De)Activate the patches for the specific core, or for all the kernels - this is different from installation - installed patches are not used - only activated ones.
+ Refresh patch list.

Yum plugin will be able to do exactly what is stated in the beginning of the proposal - no need to repeat it over.

### Patch queues

I have stated that we will have a patch queue - this will be a list of all the patches to be applied to the specific kernel on boot, in exact order of application. We need the exact ordening to make sure the patches are loaded the same way as when the user applies them one by one to avoid any quirks. Also, this would let us separate out the patches applied to different kernels, and allow the user to only use a patch for one specific version, for example.

This file can be as simple as just a list of the patch id's in a file - users won't have way too many kernels installed anyway, and propably won't have too many patches activated to load - thus, it will be effective. It can be changed later, if needed, with ease and backwards compatibility.

Having the queues in this form makes them piece of cake to apply on the kernel boot - we can put it to autorun after the kernel is ready.

I propose this schedule:
### Schedule

+ Week 1-5 - Develop the repo, by examining yum's code or writing everything from scratch. This will not include patch activation/deactivation, but rather all the passive management and patch creation/editing.
+ Week 6-8 - Develop patch activation/deactivation, queue management, queue startup script, and make the repo clearer in general. 
+ Week 9-10 - Develop the yum plugin.
+ Week 11-12 - Testing and cleanup. 

### Mid-term

I will have a repo working by this time, and maybe some code for activation/deactivation, but it will still be dirty.

### Ways to do more if time allows

If anything is finished faster than in schedule - these features can get implemented as well:
+ Allow the user to install potentially incompatible patches (under his/her own risk, and after a "big red warning" not to do so), and add possible exceptions to the metafile.
+ Make a yum plugin/extend one for patch installation from yum

### Shortcomings

+ Using an own repo adds one more package manager to the system - maybe it is possible to integrate with yum, but that adds other shortcomings, like - having all the patches in one packages, or having a lot of packages for each patch, and that makes managing it harder. Maybe there is a workaround for it.
+ Creating an open repo with patches can make a security problem - there is no control over what content is in the patch.

I do not see any other major shortcomings.

### Future

I am not sure I can crunch these features into the schedule for GSoC, but, as I am willing to continue working on that after GSoC, I add them here:

+ Some common api to test out the patches - having it will save a lifetime for the developers, but it requires a lot of research and coding. 

# Background information

### Education

I am currently in my 3rd year of studying (out of 4) in DNU.

Also, I took a course on coursera, which might be relevant here:
+ [Computer Networks](https://www.coursera.org/course/comnetworks) - 95%

Also, I am a participant of ACM ICPC Contests. I participated in SEERC (semi-finals) as part of the team DNU_Cifeidion for 2 times (2013, 2014).

### Interests

I am generally interested in Cryptography, Infomation Security, Pentesting, and all connected with this field.
Also, I am interested in back-end development in general. 

As I have stated in the mailing list - this is the project which took the second place in my priority list. I placed it there because it would hone my skills in backend development and give me experience in working with the very kernel - which will boost my pentesting skills (not directly) - I will know the kernel better, and that is a different attack vector from those I am aware of. Also, I will be able to share this knowledge with people, and make the internet a better place.

### Techinal skills
(ordered by their importance for the project)
+ CentOS configuration (and other distros in general) - I have skills with configuring CentOS (6.5 and 7) (and other distros in general) for running with hardened security (firewall, SSL for web-server, non-default ports for everything not connected with the public services, SELinux, and so on).
+ Git - I can use it well, including branching, merging branches, forking and so on, but I never made pull requests (excluding the case with multer.js) (I will be writing repo here, and having a skill with an existing repo is important, even if this repo is very different)
+ Bash scripting - I have basic bash skills, I can write basic scripts to do various administration tasks (like configure firewall, pull files from git, pull files from ftp and so on) - should be enough for this project.
+ Python - I have some python skills (2.7.3) (Full understanding of syntax, understanding of functional programming, but no knowledge of special features and no experience in optimising the code in it), and I even wrote one Django site (unfortunately, closed), but I will learn it better for this project before summer - I wanted to do it anyway.
+ Networks - I understand how protocols of the current web work.

Not very relevant skills, but I will list them:
+ C++ (traditional) - I am well familiar with it, but I have no experience in handling big projects in it (2000 lines max) (I know that estimating hardness in lines of code is a bit incorrect, but in this case we estimate size)
+ C# (and .NET in general) - I have some projects in this language. One is published in my github account: [Optical Builder](https://github.com/hotab/OpticalBuilder). Code there is a bit of a mess at places, though.
+ Nodejs (Javascript) - I am working with it currently, in a startup.
+ Objective-C - I wrote a game (in a team) in it: https://itunes.apple.com/au/app/smash-evil-match-3-puzzle/id910895645?mt=8
+ Cryptography - I have understanding of this subject, and I understand what to select for encryption/authentication. 
+ Pentesting - I have some skill in pentesting - learned it from CTFs and pentesting my own projects (Django site and current startup), but not much of a theoretical base - mostly read OWASP guides.

PS. I have no open-source experience right now (apart from C# project I mentioned earlier). For me - this project is an entry point. 

### Development environment
+ I currently use Webstorm and Sublime Text 2 on Mac OS X, Linux and Windows.
+ I use Visual Studio for C++ and C#, and AppCode for Objective-C and C++
+ I plan to use PyCharm for Python.
+ I will be able to get a server of my own to test out the patches - I have a good notebook (will hold virtualization), and a home PC I dedicated as the server a long time ago - why not use it?
