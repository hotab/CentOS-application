# CentOS-application

# Project Description
[Extendable system hardening tool](http://wiki.centos.org/GSoC/2015/Ideas#sys-hardening-tool)

After analything how is it done now, I came to a conclusion that this should me a modular tool, wich will use python and sh scripts. I think so, because:

+ sh scripts, that are to be used here, are rather simple (say, opening a port for connections, closing it, locking root user login to mysql, and so on), up to being one-liners, thus it is easier to implement them directly, so that it is familiar for those, who will implement the best practices, and can be easily read by those only using it, in case they do, and creating a good macros language for that would not simplify it further.
+ python is installed by default in CentOS, and thus, is available on every machine. It is slow, but for a one-off task like hardening security - it is perfect. It will give us ability to implement platform-independed tools to determine installed software, execute recipes, and so on - most of the work will be done here.

In short: We will use sh commands to do the actual hardening, and we will use python scripts to execute needed commands. 

Thus, I can see this structure of a recipe:

+ Metafile - here we will store the recipe info (like, which files does it use to do what, name, author, version and so on), and more importantly - the recipes it depends upon (more on that later)
+ Main script - the actual script that will take commands in some form (for example: ```secctl firewall open port 42\tcp```)
+ Include script - the script, which can be used by other recipes. This will be needed, for example, to allow mysql recipes to open/close ports.
+ Folder with sh scripts this recipe will use (for example - firewall commands will need to determine which firewall is installed first - firewalld or iptables, and then run commands)
+ Also, both main and include scripts will need to allow a command to audit everything. For example, ```secctl firewall audit port 42\tcp```

_Note_: Roles are just recipes which only use and control other recipes. For example, mysql_role recipe can include firewall and mysql recipes

The core functionality would be - getting the repo info, getting/creating scripts, checking whether the software is present and active, and so on - all the periferal things.

As we will use python for the main tool too - we can use yum's code for the repo of recipes, and we can integrate with it to look up software (will need to be changed when it is ported to non-rpm based distros, which is rather possible with all the other tooling that was selected, but that won't be hard to do).

I propose this schedule:
# Schedule

+ Week 1 - Think through most of the functions the core module will need, think through all the metafile format and propose a set of common commands for scripts, propose a common format for the include script for ease of usage. (This will require a lot of communication with community, and thus I give a week for it, and it might continue throughout all summer, and even after that (I am thinking about supporting this project after GSoC as well). But 80% will be done in the beginning).
+ Week 1 - 5. Code the functions for the core - repo communication, recipe creation, read software from yum, and so on. Examine yum's code to see whether there are parts to take from it. It is good and well-tested code, and that will simplify repo management as well - it can be a modification of some tool which is now in use (modifying it is also included in this period). If that is not possible or is too hard - create this tool and create the repo format.
+ Week 6 - 9 - Write some recipes, and ajust the core functions to match the actual needs of the recipes.
+ Week 10 - 12 - Test everything through - create tests, verify them, and so on.

Week 1 interlaps in the shedule, because I can create the formats for everything and exploring yum```'s code in parallel. I can also implement some obvious features that way.

# Ways to do more if time allows

If anything is finished faster than in schedule - these features can get implemented as well:
+ If there are many kinds of the same kind of software installed, give some generalized way to check which one is active right now (e.g. CentOS 7 has both ```iptables``` and ```firewalld``` after installation)
+ Make a yum plugin for recipe installation (It is good to have all the installation done by one tool, thus - this is a good idea)
+ Simplify role creation
+ Explore ways to avoid using sh for commands (see Shortcomings)
+ Code more recipes

# Shortcomings

+ Using sh might be an overkill for this task - it is too flexible. Maybe there is a way to circument creating a macro language, which won't need quite some unneeded code.
+ Creating an open repo with recipes can make a security problem in itself - there is no control over what content is in the package.

I do not see any other major shortcomings.

