## Basic

* [POSIX.1-2008](http://pubs.opengroup.org/onlinepubs/9699919799/)
* [Linux Foundation Referenced Specifications](http://refspecs.linuxfoundation.org/)
* [Filesystem Hierarchy Standard](http://refspecs.linuxfoundation.org/fhs.shtml)
  * [Hierarchy Standard 3.0](http://refspecs.linuxfoundation.org/FHS_3.0/fhs-3.0.html) (June 3, 2015)
  * [Hierarchy Standard 2.3](http://refspecs.linuxfoundation.org/FHS_2.3/fhs-2.3.html) (January 29, 2004)
* [The Linux man-pages project](https://www.kernel.org/doc/man-pages/)
* [Linux kernel source](https://github.com/torvalds/linux)
* [Cygwin User's Guide](http://cygwin.com/cygwin-ug-net/cygwin-ug-net.html)

* [Device file](http://en.wikipedia.org/wiki/Device_node)
* [Device mapper](http://en.wikipedia.org/wiki/Device_mapper)
* [16 commands to check hardware information on Linux](http://www.binarytides.com/linux-commands-hardware-info/) (Apr 8, 2014)
* [Logical Volume Manager](http://en.wikipedia.org/wiki/LVM2)
  * <code>Mounting Point</code>/<code>Logical Volume</code>/<code>Physical Partition</code>/<code>Physical Volume</code>
    * [Relationship between various elements of the LVM (Wikipedia)](http://upload.wikimedia.org/wikipedia/commons/thumb/e/e6/Lvm.svg/600px-Lvm.svg.png)
* [Disk Partitioning](http://en.wikipedia.org/wiki/Disk_partitioning)
* [JBOD](http://en.wikipedia.org/wiki/Non-RAID_drive_architectures#JBOD)
* [How to View Linux System Reboot Date and Time](http://www.thegeekstuff.com/2011/10/linux-reboot-date-and-time/)
* [How to resolve symbolic links in a shell script](https://stackoverflow.com/questions/7665/how-to-resolve-symbolic-links-in-a-shell-script) (Aug 11 '08)
* [How to Set Limits on User Running Processes in Linux](https://www.tecmint.com/set-limits-on-user-processes-using-ulimit-in-linux/) (May 13, 2016)

* [What is UMASK and how to set UMASK in Linux/Unix?](http://www.linuxnix.com/2011/12/umask-define-linuxunix.html)
* [Find Out Current Working Directory Of A Process Running on Linux/Unix](http://www.cyberciti.biz/tips/linux-report-current-working-directory-of-process.html) (November 14, 2007)
* [What's the best way to check that environment variables are set in Unix shellscript](http://stackoverflow.com/questions/307503/whats-the-best-way-to-check-that-environment-variables-are-set-in-unix-shellscr)(Nov 21 '08)
* [How to get IP Address using shell script?](http://unix.stackexchange.com/questions/119269/how-to-get-ip-address-using-shell-script)(Mar 12 '14) : <code>realpath</code>

* [6 Examples To Get Linux Hardware Details](http://linoxide.com/linux-how-to/few-command-helps-to-get-linux-hardware-details/)
* [Linux List Hardware Information Command](http://www.cyberciti.biz/faq/linux-list-hardware-information/)

### User Management

* [Howto: Linux Add User To Group](http://www.cyberciti.biz/faq/howto-linux-add-user-to-group/)
* [How do I remove a user from a group?](http://unix.stackexchange.com/questions/29570/how-do-i-remove-a-user-from-a-group)(Jan 20 '12)
* [How To Create a Sudo User on CentOS](https://www.digitalocean.com/community/tutorials/how-to-create-a-sudo-user-on-centos-quickstart) (March 29, 2016)
* [How do I remove a user from a group?](http://unix.stackexchange.com/questions/29570/how-do-i-remove-a-user-from-a-group) (Jan 20 '12) : <code>gpasswd</code>

### Service Control

* [[http://en.wikipedia.org/wiki/Init](http://en.wikipedia.org/wiki/Init) <code>init</code>]
* [[https://en.wikipedia.org/wiki/Systemd](https://en.wikipedia.org/wiki/Systemd) <code>systemd</code>] : *provides a system and service manager that runs as PID 1 and starts the rest of the system*
* [[https://en.wikipedia.org/wiki/Upstart](https://en.wikipedia.org/wiki/Upstart) <code>Upstart</code>] : *an event-based replacement for the /sbin/init daemon*
* [[https://en.wikipedia.org/wiki/Inetd](https://en.wikipedia.org/wiki/Inetd) <code>inetd</code>]
* [[http://en.wikipedia.org/wiki/Xinetd](http://en.wikipedia.org/wiki/Xinetd) <code>xinetd</code>] : *a secure replacement for <code>inetd</code>*

* **<code>init</code>**
  * [[https://www.centos.org/docs/5/html/5.1/Installation_Guide/s2-boot-init-shutdown-init.html](https://www.centos.org/docs/5/html/5.1/Installation_Guide/s2-boot-init-shutdown-init.html) The <code>/sbin/init</code> Program of CentOS 5]
  * [How to check a current runlevel of your Linux system](http://linuxconfig.org/how-to-check-a-current-runlevel-of-your-linux-system)
  * [[https://www.ghacks.net/2009/04/04/get-to-know-linux-the-etcinitd-directory/](https://www.ghacks.net/2009/04/04/get-to-know-linux-the-etcinitd-directory/) Get To Know Linux: The <code>/etc/init.d</code> Directory] (April 4, 2009)
    * <code>/etc/init.d/<i>command</i> start|stop|reload|restart</code>

* **<code>systemd</code>**
  * [systemd](https://freedesktop.org/wiki/Software/systemd/)
  * [The Story Behind ‘init’ and ‘systemd’: Why ‘init’ Needed to be Replaced with ‘systemd’ in Linux](http://www.tecmint.com/systemd-replaces-init-in-linux/) (January 27, 2017)

* **<code>Upstart</code>**
  * [Upstart](http://upstart.ubuntu.com/)
  * [Upstart Intro, Cookbook and Best Practises](http://upstart.ubuntu.com/cookbook/)

* [CentOS 6/Controlling Access to Services](https://www.centos.org/docs/5/html/5.2/Deployment_Guide/ch-services.html)
* [[https://access.redhat.com/site/documentation/en-US/Red_Hat_Enterprise_Linux/6/html/Deployment_Guide/s2-services-chkconfig.html](https://access.redhat.com/site/documentation/en-US/Red_Hat_Enterprise_Linux/6/html/Deployment_Guide/s2-services-chkconfig.html) RHEL 6/Using the <code>chkconfig</code> Utility]
### Job Control

* [Job Control](http://web.mit.edu/gnu/doc/html/features_5.html)
* [Job Control on UNIX systems](http://acms.ucsd.edu/info/jobctrl.html)
### Networking

* [TCP Keepalive HOWTO](http://tldp.org/HOWTO/TCP-Keepalive-HOWTO/) (2007-05-04)
* [How to Configure Linux TCP keepalive Setting](http://www.ehowstuff.com/configure-linux-tcp-keepalive-setting/) (January 30, 2016)
* [How to interpret the output of netstat -o / netstat --timers](https://superuser.com/questions/240456/how-to-interpret-the-output-of-netstat-o-netstat-timers) (Feb 1 '11)
  * interpretation of "ESTABLISHED keepalive (8.23/0/3)"
* [TIME_WAIT and its design implications for protocols and scalable client server systems](http://www.serverframework.com/asynchronousevents/2011/01/time-wait-and-its-design-implications-for-protocols-and-scalable-servers.html) (January 21, 2011)
* [How to reduce number of sockets in TIME_WAIT?](https://serverfault.com/questions/212093/how-to-reduce-number-of-sockets-in-time-wait) (Dec 13 '10)
* [HowTo: Verify My NTP Working Or Not](http://www.cyberciti.biz/faq/linux-unix-bsd-is-ntp-client-working/)
* [TCP listen() Backlog](http://www.linuxjournal.com/files/linuxjournal.com/linuxjournal/articles/023/2333/2333s2.html)
* [What is “backlog” in TCP connections?](https://stackoverflow.com/questions/36594400/what-is-backlog-in-tcp-connections) (Apr 13 '16)

* [[https://linux.die.net/man/7/ip](https://linux.die.net/man/7/ip) <code>ip</code>]
* [[https://linux.die.net/man/7/tcp](https://linux.die.net/man/7/tcp) <code>tcp</code>]
* [[https://linux.die.net/man/7/udp](https://linux.die.net/man/7/udp) <code>udp</code>]
* [[https://linux.die.net/man/7/socket](https://linux.die.net/man/7/socket) <code>socket</code>]

* [[https://github.com/torvalds/linux/blob/v4.11/include/uapi/linux/in.h](https://github.com/torvalds/linux/blob/v4.11/include/uapi/linux/in.h) <code>linux/in.h</code>]
* [[https://github.com/torvalds/linux/blob/v4.11/include/uapi/linux/tcp.h](https://github.com/torvalds/linux/blob/v4.11/include/uapi/linux/tcp.h) <code>linux/tcp.h</code>]
* [[https://github.com/torvalds/linux/blob/v4.11/include/uapi/asm-generic/socket.h](https://github.com/torvalds/linux/blob/v4.11/include/uapi/asm-generic/socket.h) <code>asm-generic/socket.h</code>]
### System Monitoring

* [SUSE System Monitoring Utilities](https://www.suse.com/documentation/sles11/book_sle_tuning/data/cha_util.html)
* [[https://www.centos.org/docs/5/html/5.1/Deployment_Guide/s1-proc-topfiles.html](https://www.centos.org/docs/5/html/5.1/Deployment_Guide/s1-proc-topfiles.html) CentOS / Top-level Files within the <code>proc</code> File System]
* [5 commands to check memory usage on Linux](http://www.binarytides.com/linux-command-check-memory-usage/)
* [[https://access.redhat.com/solutions/406773](https://access.redhat.com/solutions/406773) Interpreting <code>/proc/meminfo</code> and free output for Red Hat Enterprise Linux 5, 6 and 7](May 27 2016)
* [[http://www.linuxatemyram.com/](http://www.linuxatemyram.com/) **Linux ate my RAM!**]
* [5 commands to check memory usage on Linux](http://www.binarytides.com/linux-command-check-memory-usage/)
## Shell

* [Ken Thompson's original documentation for the Unix shell](http://steve-parker.org/sh/thompson.shtml)
* [This is an HTMLized version of Steve Bourne's original shell tutorial](http://steve-parker.org/sh/bourne.shtml)
* [Comparison of command shells](http://en.wikipedia.org/wiki/Comparison_of_command_shells)

* [Here document](https://en.wikipedia.org/wiki/Here_document)
  * a file literal or input stream literal generally starting with <code><< EOM</code>
* [How can I pretty-print JSON?](http://stackoverflow.com/questions/352098/how-can-i-pretty-print-json)
### Bash

* [Bash Reference Manual](http://www.gnu.org/software/bash/manual/bash.html)
  * [Shell Expansions](http://www.gnu.org/software/bash/manual/bash.html#Shell-Expansions)
* [Bash Guide for Beginners](http://tldp.org/LDP/Bash-Beginners-Guide/html/index.html)
* [Bash Shell Scripting](http://en.wikibooks.org/wiki/Bash_Shell_Scripting)
* [An A-Z Index of the Bash command line for Linux](http://ss64.com/bash/)
* [Bash Hackers Wiki Frontpage](http://wiki.bash-hackers.org/start)
* [Google's Shell Style Guide](https://google.github.io/styleguide/shell.xml)

* [Bash Startup Files](https://www.gnu.org/software/bash/manual/html_node/Bash-Startup-Files.html)
* [Bash: about .bashrc, .bash_profile, .profile, /etc/profile, etc/bash.bashrc and others](http://stefaanlippens.net/bashrc_and_others)
* [My bashrc, bash aliases, profile and other files](http://stefaanlippens.net/my_bashrc_aliases_profile_and_other_stuff)
* [Shell initialization files](http://www.tldp.org/LDP/Bash-Beginners-Guide/html/sect_03_01.html)
* [System-wide environment variables](https://help.ubuntu.com/community/EnvironmentVariables#System-wide_environment_variables)
* [How To Use Bash History Commands and Expansions on a Linux VPS](https://www.digitalocean.com/community/tutorials/how-to-use-bash-history-commands-and-expansions-on-a-linux-vps)
* [Hiding output of a command](http://askubuntu.com/questions/474556/hiding-output-of-a-command) (May 30 '14)
  * <code><i>command</i> > /dev/null 2>&amp;1</code>
* [How do I reload .bashrc without logging out and back in?](https://stackoverflow.com/questions/2518127/how-do-i-reload-bashrc-without-logging-out-and-back-in) (Mar 25 '10)
  * <code>source ~/.bashrc</code>, <code>. ~/.bashrc</code>, <code>source ~/.profile</code>

* **Bash programming**
  * [How to escape a single quote in single quote string in Bash?](http://stackoverflow.com/questions/8254120/how-to-escape-a-single-quote-in-single-quote-string-in-bash)(Nov 24 '11)
  * [Bash Bits: Debug Logging](https://raymii.org/s/snippets/Bash_Bits_Debug_Logging.html)(15-09-2013)
  * [[http://www.cyberciti.biz/faq/bash-for-loop-array/](http://www.cyberciti.biz/faq/bash-for-loop-array/) **Bash For Loop Array: Iterate Through Array Values**](APRIL 13, 2008)
  * [The classic for-loop](http://wiki.bash-hackers.org/syntax/ccmd/classic_for)
  * [Any way to exit bash script, but not quitting the terminal](http://stackoverflow.com/questions/9640660/any-way-to-exit-bash-script-but-not-quitting-the-terminal) (Mar 9 '12)
    * <code>return</code> instead of <code>exit</code>
  * [Variables](http://tldp.org/LDP/Bash-Beginners-Guide/html/sect_03_02.html)
    * Global variables, Local variables, Exporting variables, Reserved variables, Special parameters
  * [How to define hash tables in Bash?](https://stackoverflow.com/questions/1494178/how-to-define-hash-tables-in-bash) (Sep 29 '09)

* **Shell expansions**
  * [Command Substitution](https://www.gnu.org/software/bash/manual/bashref.html#Command-Substitution)
    * <code>`echo $CODE` | base64`</code>
  * [Bash Conditional Expressions](http://www.gnu.org/software/bash/manual/bash.html#Bash-Conditional-Expressions) (*<code>-f file, -d file, -eq, -ne, </code>*...)
  * [Shell Parameters](http://www.gnu.org/software/bash/manual/bash.html#Shell-Parameters)
  * [Special Parameters](https://www.gnu.org/software/bash/manual/bashref.html#Special-Parameters) : <code>$*, $@, $#, $?, $-, $$, $0, $!, $_</code>
  * [Shell Parameter Expansion](http://www.gnu.org/software/bash/manual/bash.html#Shell-Parameter-Expansion)
    * <code>${parameter:-word}, ${parameter:=word}, ${parameter:?word}, ${parameter:+word}, ${parameter##word}, ${parameter%%word}</code>, ...
  * [Filename Expansion](https://www.gnu.org/software/bash/manual/bash.html#Filename-Expansion)
    * Pattern syntax in file name expansion is different with general regex patterns.
  * [Pattern Matching in Filename Expansion](http://www.gnu.org/software/bash/manual/bash.html#Pattern-Matching)
  * [History Expansion](https://www.gnu.org/software/bash/manual/bashref.html#History-Interaction) : <code>!!, !-1, !!:$</code>
    * [15 Examples To Master Linux Command Line History](http://www.thegeekstuff.com/2008/08/15-examples-to-master-linux-command-line-history/) (AUGUST 11, 2008)
  * [Using “${a:-b}” for variable assignment in scripts](https://unix.stackexchange.com/questions/122845/using-a-b-for-variable-assignment-in-scripts) (Apr 3 '14)

* **If statement**
  * [[http://tldp.org/LDP/Bash-Beginners-Guide/html/sect_07_01.html](http://tldp.org/LDP/Bash-Beginners-Guide/html/sect_07_01.html) **Introduction to if**]
    * <code>if [ -d /var/lib/httpd ], if [ ! -d /var/lib/httpd ], if [ -d /var/lib/httpd -a -d /var/log/httpd ]</code>
  * [if statements](https://en.wikibooks.org/wiki/Bash_Shell_Scripting#if_statements)
  * [Test if element is in array in bash](http://superuser.com/questions/195598/test-if-element-is-in-array-in-bash)(Oct 4 '10)

* **getopts**
  * [Small getopts tutorial](http://wiki.bash-hackers.org/howto/getopts_tutorial)
  * [Different ways to implement flags in bash](https://jonalmeida.com/posts/2013/05/26/different-ways-to-implement-flags-in-bash/) (May 26, 2013)
  * [bash getopts with multiple and mandatory options](https://stackoverflow.com/questions/11279423/bash-getopts-with-multiple-and-mandatory-options) (Jul 1 '12)
## X11

* [X Window System](https://en.wikipedia.org/wiki/X_Window_System) (on Wikipedia)
* [X window manager](https://en.wikipedia.org/wiki/X_window_manager) (on Wikipedia)
* [Desktop environment](https://en.wikipedia.org/wiki/Desktop_environment) (on Wikipedia)

* [Documentation for the X Window System Version 11 Release 7.7 (X11R7.7)](http://www.x.org/releases/X11R7.7/doc/)
* [Windows Managers for X](http://xwinman.org/)
* [Cygwin/X User's Guide](http://x.cygwin.com/docs/ug/cygwin-x-ug.html)
* [Cygwin/X Frequently Asked Questions](http://x.cygwin.com/docs/faq/cygwin-x-faq.html)

* [Login to remote sessions using XDMCP](http://x.cygwin.com/docs/ug/using-remote-session.html)
* [Granting access to specific hosts for X server](http://osr507doc.sco.com/en/GECG/X_Disp_ProcAccessByHost.html)
* [How do I access my remote Ubuntu server via X-windows from my Mac?](http://askubuntu.com/questions/163155/how-do-i-access-my-remote-ubuntu-server-via-x-windows-from-my-mac) (Jul 13 '12)

* **X11 forwarding**
  * [Using X11 forwarding in SSH](http://the.earth.li/~sgtatham/putty/0.63/htmldoc/Chapter3.html#using-x-forwarding)
  * [How to use X11 forwarding with PuTTY](http://superuser.com/questions/119792/how-to-use-x11-forwarding-with-putty) (Mar 14 '10)
  * [Displaying remote clients](https://x.cygwin.com/docs/ug/using-remote-apps.html) : <code>ssh</code> or <code>xhost</code>

* [Comparison of X Window System desktop environments](https://en.wikipedia.org/wiki/Comparison_of_X_Window_System_desktop_environments)
### GNOME

* [GNOME Desktop System Administration Guide - 2.32.0](http://library.gnome.org/admin/system-admin-guide/2.32/system-admin-guide.html)
* [GNOME Display Manager Reference Manual - 2.32.1](http://library.gnome.org/admin/gdm/2.32/)

* [Desktop files: putting your application in the desktop menus](https://developer.gnome.org/integration-guide/stable/desktop-files.html.en)
## Linux Distributions

* [List of Linux distributions](https://en.wikipedia.org/wiki/List_of_Linux_distributions)
* [Comparison of Linux distributions](https://en.wikipedia.org/wiki/Comparison_of_Linux_distributions)
### CentOS

* [CentOS releases](http://en.wikipedia.org/wiki/CentOS#CentOS_releases)
* [CentOS-5 Documentation](http://www.centos.org/docs/5/)

* [CentOS 6.5 Installation on VirtualBox](http://www.youtube.com/watch?v=7pQIIQ-mY90)(Jan 23, 2014)
* [Installing Guest Additions](http://wiki.centos.org/HowTos/Virtualization/VirtualBox/CentOSguest#head-a0aa0ed2ffd3d99a27125c4a9ee0ec3d2cbf0e2c)
* [Available Repositories for CentOS](http://wiki.centos.org/AdditionalResources/Repositories)
* [Enable Bash completion on CentOS 6.2 / SL 6.2](http://linux-bsd-sharing.blogspot.kr/2012/04/tip-enable-bash-completion-on-centos-62.html)
* [Initial Server Setup with CentOS 7](https://www.digitalocean.com/community/tutorials/initial-server-setup-with-centos-7) (July 21, 2014)
* [[https://www.centos.org/docs/5/html/5.1/Installation_Guide/s2-boot-init-shutdown-init.html](https://www.centos.org/docs/5/html/5.1/Installation_Guide/s2-boot-init-shutdown-init.html)  The <code>/sbin/init</code> Program]
  * [Running Additional Programs at Boot Time](https://www.centos.org/docs/5/html/5.1/Installation_Guide/s1-boot-init-shutdown-run-boot.html) : <code>/etc/rc.d/rc.local</code>

* [Installing Subversion on CentOS](http://subversion.apache.org/packages.html#centos)
* [Installing Maven on CentOS](http://maven.apache.org/download.cgi#Unix-based_Operating_Systems_Linux_Solaris_and_Mac_OS_X)
* [Installing Node.js on CentOS](https://github.com/nodesource/distributions#enterprise-linux-based-distributions)
* [How to install Google Chrome 28+ on RHEL/CentOS 6 or 7](http://chrome.richardlloyd.org.uk/)
* [CentOS SSH Installation And Configuration](https://www.cyberciti.biz/faq/centos-ssh/) (MARCH 14, 2009)
* [HowTo: Install ssh In Linux](https://www.cyberciti.biz/faq/how-to-installing-and-using-ssh-client-server-in-linux/) (APRIL 20, 2012)
#### Utilities

##### <code>journalctl</code>

* [ How To Use Journalctl to View and Manipulate Systemd Logs](https://www.digitalocean.com/community/tutorials/how-to-use-journalctl-to-view-and-manipulate-systemd-logs) (February 5, 2015)
* [Logging With Journald In RHEL7/CentOS7](https://www.unixmen.com/logging-journald-rhel7centos7/)
### Ubuntu

* [Official Ubuntu Documentation](https://help.ubuntu.com/)
* [Ubuntu 14.04 Server Documentation](https://help.ubuntu.com/14.04/serverguide/index.html)
* [Ubuntu 14.04 Desktop Documentation](https://help.ubuntu.com/14.04/ubuntu-help/index.html)

* [How to install minimal Ubuntu desktop](http://ask.xmodulo.com/install-minimal-ubuntu-desktop.html) (November 11, 2013)
* [open a GUI window using terminal](https://ubuntuforums.org/showthread.php?t=1484530) (May 16th, 2010)
* [[http://askubuntu.com/questions/419610/permission-of-a-desktop-file](http://askubuntu.com/questions/419610/permission-of-a-desktop-file) Permission of a <code>.desktop</code> file] (Feb 11 '14)
* [System-wide environment variables](https://help.ubuntu.com/community/EnvironmentVariables#System-wide_environment_variables) : <code>/etc/environment</code>, <code>/etc/profile.d/*.sh</code>
* [Session-wide environment variables](https://help.ubuntu.com/community/EnvironmentVariables#Session-wide_environment_variables) : <code>~/.profile</code>

* [How To Install Java on Ubuntu with Apt-Get](https://www.digitalocean.com/community/tutorials/how-to-install-java-on-ubuntu-with-apt-get) (February 13, 2014)
* [How to Install The Latest Eclipse in Ubuntu 16.04, 15.10](http://ubuntuhandbook.org/index.php/2016/01/how-to-install-the-latest-eclipse-in-ubuntu-16-04-15-10/) (January 13, 2016)
* [Installing Node.js via package manager - Debian and Ubuntu based Linux distributions](https://nodejs.org/en/download/package-manager/#debian-and-ubuntu-based-linux-distributions)
* [How To Install Node.js on an Ubuntu 14.04 server](https://www.digitalocean.com/community/tutorials/how-to-install-node-js-on-an-ubuntu-14-04-server) (May 12, 2014)
* [Insalling golang on Ubuntu](https://github.com/golang/go/wiki/Ubuntu)
* [Get Docker for Ubuntu](https://docs.docker.com/engine/installation/linux/ubuntu/)
* [How do I install a .deb file via the command line?](https://askubuntu.com/questions/40779/how-do-i-install-a-deb-file-via-the-command-line) (May 6 '11)
* [How can I verify the speed of my NIC in ubuntu?](https://askubuntu.com/questions/431911/how-can-i-verify-the-speed-of-my-nic-in-ubuntu) (Mar 9 '14)
* [Ubuntu 16 – how to increase maximum file open limit ( ulimit -n )](https://codingweb.lj.studio/ubuntu-16-increase-maximum-file-open-limit-ulimit-n/) (2016-08-30)
#### Repositories

* [Repositories/Ubuntu](https://help.ubuntu.com/community/Repositories/Ubuntu)
* [Ubuntu Packages Search](http://packages.ubuntu.com/)
* [PPA(Personal Package Archives) for Ubuntu](https://launchpad.net/ubuntu/+ppas)
  * a software repository for uploading source packages to be built and published as an APT repository by Launchpad
#### Commands

* [[http://manpages.ubuntu.com/manpages/xenial/man8/apt-get.8.html](http://manpages.ubuntu.com/manpages/xenial/man8/apt-get.8.html) <code>apt-get</code>]
  * [25 Useful Basic Commands of APT-GET and APT-CACHE for Package Management](http://www.tecmint.com/useful-basic-commands-of-apt-get-and-apt-cache-for-package-management/) (March 13, 2013)
* [[http://manpages.ubuntu.com/manpages/xenial/man8/apt-cache.8.html](http://manpages.ubuntu.com/manpages/xenial/man8/apt-cache.8.html) <code>apt-cache</code>]
* [[http://manpages.ubuntu.com/manpages/xenial/en/man1/apt-file.1.html](http://manpages.ubuntu.com/manpages/xenial/en/man1/apt-file.1.html) <code>apt-file</code>] : searching files in packages for the APT package management system.
* [[http://manpages.ubuntu.com/manpages/xenial/man8/apt.8.html](http://manpages.ubuntu.com/manpages/xenial/man8/apt.8.html) <code>apt</code>]
  * provides a high-level commandline interface for the package management system.
  * cann't be used inside script file.
* [[http://manpages.ubuntu.com/manpages/xenial/man7/tcp.7.html](http://manpages.ubuntu.com/manpages/xenial/man7/tcp.7.html) <code>tcp</code>]
  *  <code>/proc/sys/net/ipv4/</code>
### Fedora

* [Fedora Documentation](http://docs.fedoraproject.org/en-US/index.html)
* [Services and Daemons of Fedora 17](http://docs.fedoraproject.org/en-US/Fedora/17/html/System_Administrators_Guide/ch-Services_and_Daemons.html)
* [Fedora RPM Guide](http://docs.fedoraproject.org/en-US/Fedora_Draft_Documentation/0.1/html/RPM_Guide/index.html)

* Storage
  * [Disk Partitions and Mount Points](http://docs.fedoraproject.org/en-US/Fedora/20/html/Installation_Guide/ch-partitions-x86.html#s2-partitions-mt-points-x86)
  * [Recommended Partitioning Scheme](http://docs.fedoraproject.org/en-US/Fedora/20/html/Installation_Guide/s2-diskpartrecommend-x86.html)
  * [Partition Naming Scheme](http://docs.fedoraproject.org/en-US/Fedora/20/html/Installation_Guide/ch-partitions-x86.html#s2-partitions-part-name-x86)
  * [Understanding LVM](http://docs.fedoraproject.org/en-US/Fedora/20/html/Installation_Guide/sn-partitioning-lvm.html)

* [Fedora and Red Hat Enterprise Linux](https://fedoraproject.org/wiki/Red_Hat_Enterprise_Linux?rd=RHEL#History)
### Red Hat Enterprise Linux

* [RHEL Documentation Home](https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/)
* RHEL 6
  * [Installation Guide](https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/6/html/Installation_Guide/index.html)
  * [Recommended Partitioning Scheme](https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/6/html/Installation_Guide/s2-diskpartrecommend-x86.html)
## UNIX Systems

### AIX

* [Virtualization Concepts](https://www.ibm.com/developerworks/wikis/display/WikiPtype/Virtualization+Concepts)
* [AIX Toolbox for Linux Applications](http://www-03.ibm.com/systems/power/software/aix/linux/)
#### AIX Commands

* [oslevel](http://pic.dhe.ibm.com/infocenter/aix/v7r1/topic/com.ibm.aix.cmds/doc/aixcmds4/oslevel.htm)
  * Reports the latest installed level (technology level, maintenance level and service pack) of the system.
* [lsdev](http://pic.dhe.ibm.com/infocenter/aix/v7r1/topic/com.ibm.aix.cmds/doc/aixcmds3/lsdev.htm)
  * Displays devices in the system and their characteristics.
* [prtconf](http://pic.dhe.ibm.com/infocenter/aix/v7r1/topic/com.ibm.aix.cmds/doc/aixcmds4/prtconf.htm)
  * Displays system configuration information.
* [lparstat](http://pic.dhe.ibm.com/infocenter/aix/v7r1/topic/com.ibm.aix.cmds/doc/aixcmds3/lparstat.htm)
  * Reports logical partition (LPAR) related information and statistics.
* [no](http://pic.dhe.ibm.com/infocenter/aix/v7r1/topic/com.ibm.aix.cmds/doc/aixcmds4/no.htm)
  * Manages network tuning parameters.
### Solaris

* [solaris - Getting around truncated “ps](http://stackoverflow.com/questions/4892516/getting-around-truncated-ps)
## Commands/Utilities

* [List of Unix commands](https://en.wikipedia.org/wiki/List_of_Unix_commands)
### Common

Command

Description

Readings

**<code>find</code>**

find files

[[http://pubs.opengroup.org/onlinepubs/9699919799/utilities/find.html](http://pubs.opengroup.org/onlinepubs/9699919799/utilities/find.html) POSIX.1-2008/Utilities/<code>find</code>]

**<code>id</code>**

return user identity

[http://en.wikipedia.org/wiki/Id_(Unix](http://en.wikipedia.org/wiki/Id_(Unix))

**<code>source</code>**

read and execute ex commands from file

[[http://pubs.opengroup.org/onlinepubs/9699919799/utilities/ex.html#tag_20_40_13_39](http://pubs.opengroup.org/onlinepubs/9699919799/utilities/ex.html#tag_20_40_13_39) POSIX.1-2008/Utilities/<code>ex/source</code>]

**<code>.</code>**

evaluates commands in a computer file in the current execution context

[https://en.wikipedia.org/wiki/Dot_(command)](https://en.wikipedia.org/wiki/Dot_(command))

**<code>sudo</code>**

allows users to run programs with the security privileges of another user (normally the superuser, or root)

[http://en.wikipedia.org/wiki/Sudo](http://en.wikipedia.org/wiki/Sudo)

**<code>wc</code>**

word, line, and byte or character count

[[http://pubs.opengroup.org/onlinepubs/9699919799/utilities/wc.html](http://pubs.opengroup.org/onlinepubs/9699919799/utilities/wc.html) POSIX.1-2008/Utilities/<code>wc</code>]

**<code>grep</code>**

search a file for a pattern

[[http://pubs.opengroup.org/onlinepubs/9699919799/utilities/grep.html](http://pubs.opengroup.org/onlinepubs/9699919799/utilities/grep.html) POSIX.1-2008/Utilities/<code>grep</code>]

**<code>tee</code>**

reads standard input and writes it to both standard output and one or more files, effectively duplicating its input.

[[https://en.wikipedia.org/wiki/Tee_(command](https://en.wikipedia.org/wiki/Tee_(command)) <code>tee</code>]

**<code>curl </code>**

command line tool and library for transferring data with URLs

[Manual](https://curl.haxx.se/docs/manual.html)
[man page](https://curl.haxx.se/docs/manpage.html)

**<code>wget</code>**

non-interactive download of files from the Web

[GNU Wget Manual](http://www.gnu.org/software/wget/manual/html_node/)

* [-maxdepth option after a non-option AND find: paths must precede expression](http://stackoverflow.com/questions/12443689/maxdepth-option-after-a-non-option-and-find-paths-must-precede-expression)(Sep 16 '12)
* [Using sudo with for loop](http://stackoverflow.com/questions/10889072/using-sudo-with-for-l) (Jun 4 '12)
* [[https://daniel.haxx.se/docs/curl-vs-wget.html](https://daniel.haxx.se/docs/curl-vs-wget.html) <code>curl</code> vs <code>wget</code>]
#### <code>su</code>

* [Linux Run Command As Another User](https://www.cyberciti.biz/open-source/command-line-hacks/linux-run-command-as-different-user/) (JULY 20, 2012)
* [Bash cannot act as nobody and nogroup?](http://unix.stackexchange.com/questions/222602/bash-cannot-act-as-nobody-and-nogroup) (Aug 11 '15) : <code>su -s /bin/bash nobody '-c echo Hi'</code> for account with <code>/sbin/nologin</code>
#### <code>sudo</code>

* [sudo 1.8.20 manual](https://www.sudo.ws/man/1.8.20/sudo.man.html)
* [how to run two commands in sudo?](http://stackoverflow.com/questions/5560442/how-to-run-two-commands-in-sudo) (Apr 6 '11)
#### <code>tee</code>

* [[https://en.wikipedia.org/wiki/Tee_(command](https://en.wikipedia.org/wiki/Tee_(command)) <code>tee</code>]
* [[http://pubs.opengroup.org/onlinepubs/9699919799/utilities/tee.html](http://pubs.opengroup.org/onlinepubs/9699919799/utilities/tee.html) POSIX.1-2008/Utilities/<code>tee</code>]
#### <code>curl</code>

* [Manual](https://curl.haxx.se/docs/manual.html)
* [man page](https://curl.haxx.se/docs/manpage.html)
* [Using curl to automate HTTP jobs](https://curl.haxx.se/docs/httpscripting.html)
* [[https://news.ycombinator.com/item?id=7596375](https://news.ycombinator.com/item?id=7596375) <code>curl</code> with heredoc]
* [Use HTTP status codes from curl](https://coderwall.com/p/taqiyg/use-http-status-codes-from-curl)(December 19, 2016)
  * <code>curl -s -o /dev/null -w '%{http_code}' http://www.google.com/</code>
### Editing

#### <code>sed</code>

* [sed at Wikipedia](http://en.wikipedia.org/wiki/Sed)
* [[http://www.grymoire.com/Unix/Sed.html](http://www.grymoire.com/Unix/Sed.html) **Sed - An Introduction and Tutorial by Bruce Barnett**]
* [[http://www.gnu.org/software/sed/manual/sed.html](http://www.gnu.org/software/sed/manual/sed.html) GNU <code>sed</code> Manual]
* [[http://pubs.opengroup.org/onlinepubs/9699919799/utilities/sed.html](http://pubs.opengroup.org/onlinepubs/9699919799/utilities/sed.html) POSIX.1-2008/Utilities/<code>sed</code>]
* [POSIX.1-2008 Regular Expressions](http://pubs.opengroup.org/onlinepubs/9699919799/basedefs/V1_chap09.html)

* [How can I remove the first line of a text file using bash/sed script?](http://stackoverflow.com/questions/339483/how-can-i-remove-the-first-line-of-a-text-file-using-bash-sed-script) (Dec 4 '08)
* [Delete First line of a file](http://unix.stackexchange.com/questions/96226/delete-first-line-of-a-file)(Oct 16 '13)
* [SED COMMAND IN LINUX - APPEND AND INSERT LINES TO A FILE](http://www.yourownlinux.com/2015/04/sed-command-in-linux-append-and-insert-lines-to-file.html)(APRIL 19, 2015)
* [How to convert DOS/Windows newline (CRLF) to Unix newline (\n) in bash script?](http://stackoverflow.com/questions/2613800/how-to-convert-dos-windows-newline-crlf-to-unix-newline-n-in-bash-script)(Apr 10 '10)
  * <code># sed -i 's/\r$//g' filename</code>
* [Unix Sed Tutorial: Append, Insert, Replace, and Count File Lines](http://www.thegeekstuff.com/2009/11/unix-sed-tutorial-append-insert-replace-and-count-file-lines/) (NOVEMBER 9, 2009)
#### <code>awk</code>

* [Awk - A Tutorial and Introduction](http://www.grymoire.com/Unix/Awk.html)
* [awk at Wikipedia](https://en.wikipedia.org/wiki/AWK)
### Administration

#### <code>useradd</code>

* [[https://linux.die.net/man/8/useradd](https://linux.die.net/man/8/useradd) <code>useradd</code> Man Page]
* [The Complete Guide to “useradd” Command in Linux – 15 Practical Examples](http://www.tecmint.com/add-users-in-linux/) (March 28, 2014)
#### clusterssh

* [https://github.com/duncs/clusterssh](https://github.com/duncs/clusterssh)
* Desc. : cluster administration tool

* Readings
  * [Automatically run commands over SSH on many servers](https://unix.stackexchange.com/questions/19008/automatically-run-commands-over-ssh-on-many-servers) (Aug 19 '11)
#### parallel

* [http://www.gnu.org/software/parallel/](http://www.gnu.org/software/parallel/)
* Desc. :  shell tool for executing jobs in parallel using one or more computers
### Monitoring & Diagnosis

* [Linux Troubleshooting Cheatsheet: strace, htop, lsof, tcpdump, iftop &amp; sysdig](https://sysdig.com/blog/linux-troubleshooting-cheatsheet/) (April 13, 2016)

Command

Description

Misc

**<code>top</code>**

**<code>netstat</code>**

**<code>lsof</code>**

displays information about files open to Unix processes

**<code>strace</code>**

**<code>tcpdump</code>**

**<code>sysctl</code>**

a tool for examining and changing kernel parameters at runtime

#### <code>top</code>

* [top man page](https://linux.die.net/man/1/top)
#### <code>lsof</code>

* [[https://people.freebsd.org/~abe/](https://people.freebsd.org/~abe/) <code>lsof</code> homepage]
  * *The free, open-source, Unix administrative tool lsof (for LiSt Open Files) displays information about files open to Unix processes.*
* [[https://en.wikipedia.org/wiki/Lsof](https://en.wikipedia.org/wiki/Lsof) <code>lsof</code> on Wikipedia]
* [[http://manpages.ubuntu.com/manpages/xenial/man8/lsof.8.html](http://manpages.ubuntu.com/manpages/xenial/man8/lsof.8.html) <code>lsof</code> in Ubuntu]

* [Using strace and lsof to debug blocked processes](https://gist.github.com/tonyc/1384523)
* [Exploring System Internals with lsof and strace](http://www.myhowto.org/solving-problems/7-exploring-system-internals-with-lsof-and-strace/) (20 July 2007)
* [Linux super-duper admin tools: lsof](http://www.dedoimedo.com/computers/lsof.html) (May 12, 2010)
#### <code>strace</code>

* [[https://strace.io/](https://strace.io/) <code>strace</code> homepage]
* [[https://en.wikipedia.org/wiki/Strace](https://en.wikipedia.org/wiki/Strace) <code>strace</code> on Wikipedia]

* [STrace : Third Eye of a System Admin](http://adminlogs.info/2012/05/05/strace-third-eye-of-a-system-admin/) (May 5, 2012)
* [Why won't strace/gdb attach to a process even though I'm root?](https://askubuntu.com/questions/143561/why-wont-strace-gdb-attach-to-a-process-even-though-im-root) (May 29 '12)
* [Strace attach problem](https://linux-tips.com/t/strace-attach-problem/99) (Mar 4, '15)
* [[https://unix.stackexchange.com/questions/220709/gdb-attach-permission-question-ptrace-scope-is-read-only](https://unix.stackexchange.com/questions/220709/gdb-attach-permission-question-ptrace-scope-is-read-only) **gdb attach permission question (ptrace_scope is read-only)**] (Aug 7 '15)
  * *In docker you can now use the --privileged option*
* [Continuously monitor files opened/accessed by a process](https://superuser.com/questions/348738/continuously-monitor-files-opened-accessed-by-a-process) (Oct 20 '11)
#### <code>netstat</code>

* [[https://en.wikipedia.org/wiki/Netstat](https://en.wikipedia.org/wiki/Netstat) <code>netstat</code> on Wikipedia]

* [How to interpret the output of netstat -o / netstat --timers](https://superuser.com/questions/240456/how-to-interpret-the-output-of-netstat-o-netstat-timers) (Feb 1 '11)
#### <code>tcpdump</code>

* [[http://www.tcpdump.org/](http://www.tcpdump.org/) <code>tcpdump</code> homepage] : a powerful command-line packet analyzer
* [[https://en.wikipedia.org/wiki/Tcpdump](https://en.wikipedia.org/wiki/Tcpdump) <code>tcpdump</code> on Wikipedia]

* [12 Tcpdump Commands – A Network Sniffer Tool](https://www.tecmint.com/12-tcpdump-commands-a-network-sniffer-tool/) (September 13, 2012)
* [Capture Packets with Tcpdump](https://support.rackspace.com/how-to/capturing-packets-with-tcpdump/) (2013-04-25)
* [Using tcpdump to see HTTP requests and responses](https://texnoblog.wordpress.com/2010/04/17/using-tcpdump/) (17 April, 2010)
* [How to filter tcpdump output based on packet length](https://stackoverflow.com/questions/9874093/how-to-filter-tcpdump-output-based-on-packet-length) (Mar 26 '12)
  * <code>tcpdump -n -i eth0 -A -x dst port 443 and greater 100</code>

* [[https://linux.die.net/man/7/pcap-filter](https://linux.die.net/man/7/pcap-filter) <code>pcap-filter</code> man page] : explains filter expressions
#### <code>sysctl</code>

* [[https://en.wikipedia.org/wiki/Sysctl](https://en.wikipedia.org/wiki/Sysctl) <code>sysctl</code> on Wikipedia]
* [[https://wiki.archlinux.org/index.php/sysctl](https://wiki.archlinux.org/index.php/sysctl) <code>sysctl</code>]
### Networking

* [Deprecated Linux networking commands and their replacements](https://dougvitale.wordpress.com/2011/12/21/deprecated-linux-networking-commands-and-their-replacements/)
  * <code>arp, ifconfig, iptunnel, iwconfig, nameif, netstat, route</code> -> <cod>ip, ss</code>
#### <code>traceroute</code>

* [[https://linux.die.net/man/8/traceroute](https://linux.die.net/man/8/traceroute) <code>traceroute</code> man page]
* [traceroute](https://en.wikipedia.org/wiki/Traceroute) (on Wikipedia)
* [what does “***” mean when traceroute](https://serverfault.com/questions/334029/what-does-mean-when-traceroute)(Nov 23 '11)
* [Understanding the Ping and Traceroute Commands](http://www.cisco.com/c/en/us/support/docs/ios-nx-os-software/ios-software-releases-121-mainline/12778-ping-traceroute.html) (November 29, 2006)
* [5 UNIX / Linux Traceroute Command Examples](http://www.thegeekstuff.com/2012/05/traceroute-examples) (MAY 9, 2012)
#### <code>iptables</code>

* [[https://en.wikipedia.org/wiki/Iptables](https://en.wikipedia.org/wiki/Iptables) <code>iptables</code> on Wikipedia]
  * *allows a system administrator to configure the tables[2] provided by the Linux kernel firewall (implemented as different Netfilter modules) and the chains and rules it stores.*
* [Iptables Essentials: Common Firewall Rules and Commands](https://www.digitalocean.com/community/tutorials/iptables-essentials-common-firewall-rules-and-commands) (August 10, 2015)
* [How To List and Delete Iptables Firewall Rules](https://www.digitalocean.com/community/tutorials/how-to-list-and-delete-iptables-firewall-rules) (August 14, 2015)
#### <code>conntrack</code>

* [conntrack-tools homepage](http://conntrack-tools.netfilter.org/)
  * a set of free software userspace tools for Linux that allow system administrators interact with the Connection Tracking System, which is the module that provides stateful packet inspection for iptables.
* [The conntrack-tools user manual](http://conntrack-tools.netfilter.org/manual.html)
* [Netfilter's conntrack-tools](https://terry.im/wiki/terry/Netfilter%2Bconntrack-tools.html)
### misc

#### <code>watch</code>

* [[https://en.wikipedia.org/wiki/Watch_(Unix](https://en.wikipedia.org/wiki/Watch_(Unix)) <code>watch</code> on Wikipedia]
* [The watch Command](http://www.linfo.org/watch.html)
#### <code>screen</code>

* [GNU Screen](https://en.wikipedia.org/wiki/GNU_Screen)
* [Screen User's Manual](https://www.gnu.org/software/screen/manual/screen.html)
## Tips

### At 1st Login

#### Identifying System Configuration

##### IBM AIX

* [prtconf Command](http://pic.dhe.ibm.com/infocenter/aix/v6r1/topic/com.ibm.aix.cmds/doc/aixcmds4/prtconf.htm) : displays system configuration information.
#### Identifying the product of Linux installed

For Linux, **<code>/etc/issues</code>** file contains more detailed information on what Linux product it is.

<syntaxhighlight lang="bash" enclose='div'>
$ cat /etc/issue
Red Hat Enterprise Linux Server release 5.5 (Tikanga)
Kernel \r on an \m
</syntaxhighlight>

#### Identifying kernel parameters

<syntaxhighlight lang="bash" enclose='div'>
$ sysctl -a | more
</syntaxhighlight>

#### Identifying the memory capacity and usage

<syntaxhighlight lang="bash" enclose='div'>
$ cat /proc/meminfo
MemTotal:        3922904 kB
MemFree:         3037280 kB
...
</syntaxhighlight>

#### Identifying the disk capacity and usage

<syntaxhighlight lang="bash" enclose='div'>
$ df
...
</syntaxhighlight>

#### Identifying TCP/IP ports currently in use.

You can identify TCP/IP ports currently in use using **<code>netstat</code>** command. The options of **<code>netstat</code>** is slightly different among operating systems.

For UNIX,

<syntaxhighlight lang="bash" enclose='div'>
$ netstat -na
</syntaxhighlight>

For Linux,

<syntaxhighlight lang="bash" enclose='div'>
$ netstat -nap
</syntaxhighlight>

You need root privilege to take effect of **<code>-p</code>** option
To find out whether a given port is being used or not, use <code>grep</code> command.

<syntaxhighlight lang="bash" enclose='div'>
$ netstat -nap | grep -E '(^Proto)|(8080)'
</syntaxhighlight>

For Windows,

<syntaxhighlight lang="bash" enclose='div'>
$ netstat -nao
</syntaxhighlight>

For more about **<code>netstat</code>**, refer [topics in Wikipedia](http://en.wikipedia.org/wiki/Netstat).

#### Identifying the shell of your current login

To identify what shell a user is set to use by default, you can check **<code>SHELL</code>** variable.

<syntaxhighlight lang="bash" enclose='div'>
$ echo $SHELL
/bin/bash
$ bin/ksh
$ echo $SHELL
/bin/bash
</syntaxhighlight>

As the above example shows, **<code>SHELL</code>** variable contains the login default shell type not the one currently in use.

#### Readings

* [Linux Check Memory Usage](http://www.cyberciti.biz/faq/linux-check-memory-usage/)
### Shell Commands

#### Hiding the output of command

To hide both the normal output and error output, redirect <code>stdout</code> and <code>stderr</code> to null device

<syntaxhighlight lang="bash" enclose='div'>
% npm ls -g json >/dev/null 2>&amp;1
% #or

1. npm ls -g y18n &amp;>/dev/null

</syntaxhighlight>

To hide only the error output, redirect <code>stderr</code> to null device

<syntaxhighlight lang="bash" enclose='div'>
% npm ls -g json 2>/dev/null
</syntaxhighlight>

* [Hiding output of a command](http://askubuntu.com/questions/474556/hiding-output-of-a-command) (May 30 '14)
#### Listing files using <code>find</code> command excluding files with <code>'Permission denied'</code>

When executing **<code>find</code>** command in simplest format, you may get lots of lines just saying that <code>'Permission denied'</code>. Most cases, those are not what you want, and lots of permission denied lines can disturb you identifying the wanted result.

You can use **<code>stderr</code>** redirection to cut out permission denied files (or directories).

<syntaxhighlight lang="bash" enclose='div'>
% find / -name '*.jar' 2>/dev/null
</syntaxhighlight>

* [How can I exclude all “permission denied”-messages from “find .”?](http://stackoverflow.com/questions/762348/how-can-i-exclude-all-permission-denied-messages-from-find) (from stackoverflow)
* [Redirection on Wikipedia](http://en.wikipedia.org/wiki/Redirection_(computing))
#### Finding files having specified name with full path

If you want to find files with extension of 'jar' and print them with full path, use find command with **<code>-exec</code>** operator like the following.

<syntaxhighlight lang="bash" enclose='div'>
% find . -name '*.jar' -exec ls -l {} \;
</syntaxhighlight>

For more about **<code>find</code>** command and **<code>-exec</code>** operator including strange '{}' or '\;' in the above example, refer the followings.

* [http://www.opengroup.org/onlinepubs/9699919799/utilities/find.html](http://www.opengroup.org/onlinepubs/9699919799/utilities/find.html)
* [http://docstore.mik.ua/orelly/unix3/upt/ch09_01.htm](http://docstore.mik.ua/orelly/unix3/upt/ch09_01.htm)
#### Finding files containing the specified word

<syntaxhighlight lang="bash" enclose='div'>
% find /home |xargs grep "password"
</syntaxhighlight>

For more about <code>**xargs**</code>, refer the followings.

* [[http://en.wikipedia.org/wiki/Xargs](http://en.wikipedia.org/wiki/Xargs) <code>**xargs**</code> from Wikipedia]
#### Finding large files

To find large files(not directories) under current directory and list them in pages, use the following command.

<syntaxhighlight lang="bash" enclose='div'>
% find . -type f -exec du -k {} 2>/dev/null \; | sort -nr | more
</syntaxhighlight>

To filter out small files, you can use **<code>size</code>** option with **<code>find</code>** command, or to filter out some subdirectories you can redirect the result to **<code>grep</code>** command.
The following command will list files whose size are more than 1 mega-byte under current directory recursively except the subdirectories starting with 'svn' in order of their size.

<syntaxhighlight lang="bash" enclose='div'>
% find . -type f -size +1000000c -exec du -k {} 2>/dev/null \; | sort -nr | grep -E "\./svn.*" -v
</syntaxhighlight>

#### Listing distinct file extensions of all files under a directory

<syntaxhighlight lang="bash" enclose='div'>
% find . -type f -name "*.*" | sed -r 's/^.+(\.\w+)$/\1/' | sort | uniq
</syntaxhighlight>

#### Counting files under a directory recurssively

<syntaxhighlight lang="bash" enclose='div'>
% find . -type f -print | wc -l
</syntaxhighlight>

#### Counting files in a tar file

<syntaxhighlight lang="bash" enclose='div'>
% tar -tvf archive.tar | grep "^-.*" | wc -l
</syntaxhighlight>

#### Inverse matching with <code>grep</code> command

To find lines not matching the specified patterns in a file, you can use **<code>-v</code>** option with **<code>grep</code>** command.

<syntaxhighlight lang="bash" enclose='div'>
$ svn list -R [http://.../repos1](http://.../repos1) | grep -v -E '(.*java|.*/)'
</syntaxhighlight>

You don't need to be bothered to find out how to use complex negative patterns with regex.

#### Viewing files in octal or hexadecimal format - <code>od</code>

You can view non ascii base files in hexadecimal format using **<code>od</code>** command.

<syntaxhighlight lang="bash" enclose='div'>
% od -A d -x journal.log
</syntaxhighlight>

For more about **<code>od</code>**, refer the following.

* [http://www.opengroup.org/onlinepubs/9699919799/utilities/od.html](http://www.opengroup.org/onlinepubs/9699919799/utilities/od.html)
* [http://publib.boulder.ibm.com/infocenter/pseries/v5r3/index.jsp?topic=/com.ibm.aix.cmds/doc/aixcmds4/od.htm](http://publib.boulder.ibm.com/infocenter/pseries/v5r3/index.jsp?topic=/com.ibm.aix.cmds/doc/aixcmds4/od.htm)
#### Viewing file contents without line wrapping - <code>less -S</code>

<syntaxhighlight lang="bash" enclose='div'>
% less -S known_hosts
</syntaxhighlight>

#### Viewing the result of <code>ps</code> command without line wrapping

You can redirect the result to <code>cat</code> or <code>less</code> command, or use <code>ww</code> flag.

<syntaxhighlight lang="bash" enclose='div'>
% ps auxf | cat
...
% ps auxfww
...
% ps auxf | less -+S
</syntaxhighlight>

#### Viewing file contents without comments lines (starting with <code>#</code>)

<syntaxhighlight lang="bash" enclose='div'>
% cat /etc/apt/sources.list | grep -P '^[^#].*'
</syntaxhighlight>

#### Sorting the file system usage result from the <code>du</code> command

You can sort the output of **<code>du</code>** command applying pipe to **<code>sort</code>** command.

<syntaxhighlight lang="bash" enclose='div'>
% du -m | sort -n
</syntaxhighlight>

For more about **<code>du</code>** and **<code>sort</code>**, read the followings.

* [du - estimate file space usage](http://www.opengroup.org/onlinepubs/9699919799/utilities/du.html)
* [sort - sort, merge, or sequence check text files](http://www.opengroup.org/onlinepubs/9699919799/utilities/sort.html)
#### Getting multiple files form the target URL using <code>wget</code> command

**<code>wget</code>** provide **<code>--accept</code>** or **<code>-A</code>** switch which can represent multiple files using **comma separated list**, **wild card**, or **character class**.  But it's not that <code>-A</code> switch support regular expression.

<syntaxhighlight lang="bash" enclose='div'>
$ su - hdfs -c "(cd ~; wget -x -P samples/flight/rawdata -A '198[7-9].csv.bz2' [http://stat-computing.org/dataexpo/2009/](http://stat-computing.org/dataexpo/2009/))"
$ su - hdfs -c "(cd ~; wget -x -P samples/flight/rawdata -A '199[0-9].csv.bz2' [http://stat-computing.org/dataexpo/2009/](http://stat-computing.org/dataexpo/2009/))"
$ su - hdfs -c "(cd ~; wget -x -P samples/flight/rawdata -A '200[0-8].csv.bz2' [http://stat-computing.org/dataexpo/2009/](http://stat-computing.org/dataexpo/2009/))"
$ su - hdfs -c "(cd ~; wget -x -P samples/flight/rawdata -A 'airports.csv, carriers.csv, plane-data.csv' [http://stat-computing.org/dataexpo/2009/](http://stat-computing.org/dataexpo/2009/))"
</syntaxhighlight>

For more, refer the following

* [[http://www.gnu.org/software/wget/manual/html_node/Types-of-Files.html](http://www.gnu.org/software/wget/manual/html_node/Types-of-Files.html) <code>wget</code> manual/Type of Filters]
#### Capturing incoming HTTP request using <code>tcpdump</code>

<syntaxhighlight lang="bash" enclose='div'>
sudo tcpdump -s 0 -A -i eth1 dst port 80
</syntaxhighlight>

### Bash Programming

#### 2 dimentional array

<syntaxhighlight lang="bash" enclose='div'>

declare -a m
m[0]='a b c d'
m[1]='e f g h'
m[2]='i j k l'
m[3]='m n o p'
m[4]='q r s t'

for r in "${m[@]}"; do

 echo $r
 for c in $r; do
   echo $c
 done

done
</syntaxhighlight>

### Managing Packages

#### Ubuntu

##### Installing a new software package

1. Update the package information
2. Search the software package
3. Install or upgrade the software package
4. Confirm the installation of the software package
5. (Optionally) Confirm all the files installed by the package

Using <code>apt</code>

<syntaxhighlight lang="bash" enclose='div'>
$ sudo apt update   # update
...
$ sudo apt list *golang-1.8* -a   # search
...
$ sudo apt install golang-1.8   # upgrade
...
$ sudo apt show golang-1.8   # confirm
...
$ sudo apt-file list golang-1.8 # confirm all the files installed
</syntaxhighlight>

Not using <code>apt</code>

<syntaxhighlight lang="bash" enclose='div'>
$ sudo apt-get update   # update
...
$ sudo apt-cache search golang-1.8  # search
...
$ sudo apt-get install golang-1.8   # upgrade
...
$ sudo apt-cache show golang-1.8 # confirm

...
$ sudo apt-file list golang-1.8 # confirm all the files installed
</syntaxhighlight>

* References
  * [[http://manpages.ubuntu.com/manpages/xenial/man8/apt-get.8.html](http://manpages.ubuntu.com/manpages/xenial/man8/apt-get.8.html) <code>apt-get</code>]
  * [[http://manpages.ubuntu.com/manpages/xenial/man8/apt-cache.8.html](http://manpages.ubuntu.com/manpages/xenial/man8/apt-cache.8.html) <code>apt-cache</code>]
  * [[http://manpages.ubuntu.com/manpages/xenial/en/man1/apt-file.1.html](http://manpages.ubuntu.com/manpages/xenial/en/man1/apt-file.1.html) <code>apt-file</code>] : searching files in packages for the APT package management system.
  * [[http://manpages.ubuntu.com/manpages/xenial/man8/apt.8.html](http://manpages.ubuntu.com/manpages/xenial/man8/apt.8.html) <code>apt</code>] : provides a high-level commandline interface for the package management system.
##### Installing a software package specifying version with wildcard

<syntaxhighlight lang='bash' enclose='div'>
$ sudo apt-get install nodejs=6.10.2*
</syntaxhighlight>

##### Listing all installed packages

<syntaxhighlight lang="bash" enclose='div'>
$ sudo apt list --installed
</syntaxhighlight>

</div>

