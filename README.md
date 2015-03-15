# CentOS-application

# Personal Details
+ Name: Vladislav Babkin
+ University: Dnipropetrovsk National Univercity (DNU), Ukraine
+ Degree Program: B. Sc. 
+ Course: Applied mathematics, System Analysis
+ Email: vladiksonic@mail.ru, vladiksonic@gmail.com
+ Website: hotab1994.wordpress.com (Decided to start a blog rather recently, it is totally empty right now)
+ LinkedIn: [Hotab](https://ua.linkedin.com/in/hotab/en)

# Availability and time

+ I plan to spend around 20-30 hours per week. Maybe more when possible (I will be working on a startup in parallel)
+ I will have my exams in univercity in the beginning of June for 1 or 2 weeks - I won't be able to spend as much time in this period - thus I intend to start maybe before May 25 to compensate. I think that by August 24 most (95%+)  of the work will be complete.
+ I plan to support this project after GSoC, as well.

# Project Description
[Extendable system hardening tool](http://wiki.centos.org/GSoC/2015/Ideas#sys-hardening-tool)

After analything how is it done now, I came to a conclusion that this should me a modular tool, wich will use python and sh scripts. I think so, because:

+ sh scripts, that are to be used here, are rather simple (say, opening a port for connections, closing it, locking root user login to mysql, and so on), up to being one-liners, thus it is easier to implement them directly, so that it is familiar for those, who will implement the best practices, and can be easily read by those only using it, in case they do.
+ python is installed by default in CentOS, and thus, is available on every machine. It is slow, but for a one-off task like hardening security - it is perfect. It will give us ability to implement platform-independed tools to determine installed software, execute recipes, and so on - most of the work will be done here.

In short: We will use sh commands to do the actual hardening, and we will use python scripts to execute needed commands. 

Thus, I can see this structure of a recipe:

+ Metafile - here we will store the recipe info (like, which files does it use to do what, name, author, version and so on), and more importantly - the recipes it depends upon (more on that later)
+ Main script - the actual script that will take commands in some form (for example: ```firewall open port 42\tcp```)
+ Include script - the script, which can be used by other recipes. This will be needed, for example, to allow mysql recipes to open/close ports.
+ Folder with sh scripts this recipe will use (for example - firewall commands will need to determine which firewall is installed first - firewalld or iptables, and then run commands)
+ Also, both main and include scripts will need to allow a command to audit everything. For example, ```firewall audit port 42\tcp```

_Note_: Roles are just recipes which only use and control other recipes. For example, mysql_role recipe can include firewall and mysql recipes

Core script (let's name it ```secctl```, but that is subject to change) would be able to:
+ Run recipes (```secctl firewall open port 42\tcp```)
+ Get repo info (installed packages)
+ Fetch available scripts
+ Create scripts
+ Check software presence and activity
+ and so on.
+ As we will use python for the main tool too - we can use yum's code for the repo of recipes, and we can integrate with it to look up software (will need to be changed when it is ported to non-rpm based distros, which is rather possible with all the other tooling that was selected, but that won't be hard to do).

I propose this schedule:
### Schedule

+ Week 1 - Think through most of the functions the core module will need, think through all the metafile format and propose a set of common commands for scripts, propose a common format for the include script for ease of usage. (This will require a lot of communication with community, and thus I give a week for it, and it might continue throughout all summer, and even after that (I am thinking about supporting this project after GSoC as well). But 80% will be done in the beginning).
+ Week 1 - 5. Code the functions for the core script - repo communication, recipe creation, read software from yum, and so on. Examine yum's code to see whether there are parts to take from it. It is good and well-tested code, and that will simplify repo management as well - it can be a modification of some tool which is now in use (modifying it is also included in this period). If that is not possible or is too hard - create this tool and create the repo format.
+ Week 6 - 9 - Write some recipes, and ajust the core functions to match the actual needs of the recipes.
+ Week 10 - 12 - Test everything through - create tests, verify them, and so on.

Week 1 interlaps in the shedule, because I can create the formats for everything and exploring yum```'s code in parallel. I can also implement some obvious features that way.

### Ways to do more if time allows

If anything is finished faster than in schedule - these features can get implemented as well:
+ If there are many kinds of the same kind of software installed, give some generalized way to check which one is active right now (e.g. CentOS 7 has both ```iptables``` and ```firewalld``` after installation)
+ Make a yum plugin for recipe installation (It is good to have all the installation done by one tool, thus - this is a good idea)
+ Simplify role creation
+ Explore ways to avoid using sh for commands (see Shortcomings)
+ Code more recipes

### Shortcomings

+ Using sh might be an overkill for this task - it is too flexible. Maybe there is a way to circument creating a macro language (which is also overkill, but from another point of view - it will eat up time and might not cover all the features), which will also be simple to implement;
+ Creating an open repo with recipes can make a security problem in itself - there is no control over what content is in the package.

I do not see any other major shortcomings.

### Future

I am not sure I can crunch these features into the schedule for GSoC, but, as I am willing to continue working on that after GSoC, I add them here:

+ external audit for recipes like ```firewall```, ```mysql``` (test root login externally) and others. The core should have a way to set variables to allow that without hardcoding anything.

# Background information

### Education

I am currently in my 3rd year of studying (out of 4) in DNU.

Also, I took some courses on coursera, which might be relevant here:
[Cryptography I](https://www.coursera.org/course/crypto) - 100% with Distinction
[Computer Networks](https://www.coursera.org/course/comnetworks) - 95%
[Malicious Software and its Underground Economy: Two Sides to Every Story](https://www.coursera.org/course/malsoftware) - 94.4%

Also, I am a participant of ACM ICPC Contests. I participated in SEERC (semi-finals) as part of the team DNU_Cifeidion for 2 times (2013, 2014).

### Interests

I am generally interested in Cryptography, Infomation Security, Pentesting, and all connected to this field.
Also, I am interested in back-end development in general. 

That is why I decided to choose this project - it will allow me to develop skills in protecting systems up to a good level. And, more importantly - share this skill with other people. Currently, internet is rather insecure, and I am well aware of that. This project allows to fix it, at least a little bit. And that is why I think it is important.

### Techinal skills
(ordered by their importance for the project)
+ Bash - I have basic bash skills, I can write basic scripts to do various administration tasks (like configure firewall, pull files from git, pull files from ftp and so on) - should be enough for this project.
+ Python - I have some python skills (2.7.3) (Full understanding of syntax, understanding of functional programming, but no knowledge of special features and no experience in optimising the code in it), and I even wrote one Django site (unfortunately, closed), but I will learn it better for this project before summer - I wanted to do it anyway.
+ Cryptography - I have understanding of this subject, and I understand what to select for encryption/authentication. 
+ Pentesting - I have some skill in pentesting - learned it from CTFs and pentesting my own projects (Django site and current startup), but not much of a theoretical base - mostly read OWASP guides.
+ Networks - I understand how protocols of the current web work.
+ Git - I can use it well, including branching, merging branches, forking and so on, but I never made pull requests (excluding the case with multer.js)

Not very relevant skills, but I will list them:
+ C++ (traditional) - I am well familiar with it, but I have no experience in handling big projects in it (2000 lines max) (I know that estimating hardness in lines of code is a bit incorrect, but in this case we estimate size)
+ C# (and .NET in general) - I have some projects in this language. One is published in my github account: [Optical Builder](https://github.com/hotab/OpticalBuilder). Code there is a bit of a mess at places, though.
+ Nodejs (Javascript) - I am working with it currently, in a startup.
+ Objective-C - I wrote a game (in a team) in it: https://itunes.apple.com/au/app/smash-evil-match-3-puzzle/id910895645?mt=8

PS. I have no open-source experience right now (apart from C# project I mentioned earlier). For me - this project is an entry point. 

### Development environment
+ I currently use Webstorm and Sublime Text 2 on Mac OS X, Linux and Windows.
+ I use Visual Studio for C++ and C#, and AppCode for Objective-C and C++
+ I plan to use PyCharm for Python.


