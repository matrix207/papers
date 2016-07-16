---
layout: post
title: "Linux frequently used commands"
description:
category: tools
tags: [shell, git, svn, vim]
---

#### shell terminal
* `!!` the previous command (Actually the latest command list with command `history`)   
  `!-1` is equal to `!!`  
  `!-2` is second command list with command `history`   
  `!^`  first parameter  
  `!:1` first parameter   
  `!:n` the n parameter   
  `!:$` last parameter  

* `C-A/E` **jump to the head/end position**  
  `C-U/K` trim to head/tail  
  `C-P/N` **prev/next command**  
  `C-R`   **history command**  
  `C--`   zoom in  
  `C-L`   clear screen, same as `clear`  
  `C-C`   cancel current command  
  `C-&`   cancel modify  
  `C-T`   switch  
  `C-Y`   paste  
  `C-W`   **delete a word**  
  Below operation need to unset Enable menu access keys option of terminal.  
  `C-f/b` move cursor forward/backward a character  
  `M-f/b` move cursor forward/backward a word  
  `C-d`   delete a character forward  
  `C-h`   delete a character backward  
  `M-d`   delete a word froward  
  `C-M-h` **delete a word backward, the word was splited by any special symbols**  
  `C-w`   **delete a word backward, the word was splited by space symbol only**  
  `C-]`   search character forward  
  `C-M-]` **search character backward**  

* `<alt>+.` get the last parameter of previous command

#### common command
* `cd -` switch to last dir  
  `cd` switch to user home dir  

* `cp filename{,.bak}` a fast method to bakup a file
* `mv filename{.bak,}` a fast method to cut the suffix

* `time command` count the elapsed time

* `mount -t cifs -o username=***,passwd=*** //ip/path /mnt/test` 
  mount network sharing dir  
  `mount -t iso9660 /mnt/iso/FC17-DVD.iso /mnt/dvd` 
  use for mounting ISO file  
  `mount --bind /root/project/myprj /root/x86_64_build/source` 
  mount a dir to another, use for across-compilation mostly  
  `mount -t tmpfs -o size=1024m tmpfs /mnt/ram`  
  mount memory as a w/r dir (use for high speed I/O operation, and disk space not enough)  

* `tar xvf example.tar.gz -C /root/test` extract files to specific dir  
  `tar tvf example.tar.gz` list the contents of an archive  
  `tar -zcvf - stuff | openssl des3 -salt -k *** | dd of=stuff.des3` encryption  
  `tar cvfz chenxu.tar.gz dir --exclude dir/dir1 --exclude dir/dir2/dir3`  
  `tar -xvf 1425279081.tar.gz var/log/message` get only one file   
  `dd if=stuff.des3 | openssl des3 -d -k *** | tar zxf -` decryption  
  `7z a -t7z -ptestpsw123 mytest.7z -r testfolder/*` zip folder and encrypt it by 7z  

* `getconf LONG_BIT`           print the operator system bits  
  `grep -c "lm" /proc/cpuinfo` if output is not 0, it is 64bit cpu  

* `du -sh`   
  `du -s * |sort -n |tail`

* `free   bg   kill pid    killall proc    chmod 777 (r:4,w:2,x:1)(user,group,all)`

* `find / -name "***"`  
  `find path \( -name "*.h" -or -name "*.c" \) -exec grep -in "***" {} \;`  
  `find ./ -name "*.*" -exec ls -l {} \; |awk '{print $5,$9}' |sort -n |tail` find the 10 biggest files.  
  `find / -type d -name "gedit"` search direactorys  
  `find . \( -path ./.git -o -path dir2 \) -prune -o -type d -print` search sub-dir, except .git and dir2  
  `find /usr/include/ -path /usr/include/boost -prune -o -name '*.h' -print >1.txt`  
  `find . -name ".svn" | xargs rm -rf` find .svn folder in current directory, and remove it.  
  `find ./ \( -name "*.h" -or -name "*.c" \) -exec wc -l {} \; | awk '{s+=$1} END {print s}'` count lines of specify files  
  `find ./ \( -name "*.h" -or -name "*.c" \) | xargs wc -l | tail -n 1` count lines of specify files  
  the upon two command can count lines of specify files, but what is the 
  difference of them, look this 
  [Difference xargs and exec](http://stackoverflow.com/questions/13651212/unix-difference-xargs-and-exec) 
  you will find the answer.  
  `find ./ -path ./perl -prune -o -name "*.so" -exec cp {} ../net-snmp-5.4.4-sdk/lib/ \;` 
  copy files which search by find, search exclude ./perl path  
  `find /usr -size +100M `  find file size larger than 100M  
  `find /home -mtime +120`  
  `find /var \! -atime -90`  
  `find <dir> -executable -type f` only find executable files  

* `grep -r "abc" /root/source`  
  `grep -r --include "*.h" "date" path`  
  `grep -m 1 "model name" /proc/cpuinfo` only display the first match line    
  `grep -i -E "abc|123"` match abc or 123, -i ignore, -E extended regular expression.  
  `grep -r -l "main" .` search all files under each directory, -l files-with-match, -L files-withou-match  
  `grep -w "linux" *.md` match word  
  `grep -rl --include=*.{h,cpp} "socket" .` search socket only with *.h *.cpp files  
  `grep --exclude-dir="_posts" --exclude-dir="_site"  -r "Dennis" ./`  exclude specify directory  
  `grep -rni '[^a-zA-Z]abc[^a-zA-Z]' ./`  match like "tt.abc.1234", not match like "ttabccd"

* `cut -d"" -f1`  
  e.g:  
  `[root@localhost ~]# sensors |grep "Core "`  
  Core 0:       +30.0°C  (high = +76.0°C, crit = +100.0°C)  
  Core 1:       +37.0°C  (high = +76.0°C, crit = +100.0°C)  
  `[root@localhost ~]# sensors |grep "Core " |cut -d"+" -f2 |cut -d" " -f1`  
  30\.0°C  
  37\.0°C  

* `history|awk '{print $2}'|awk 'begin {FS="|"} {print $1}'|sort|uniq -c|sort -rn|head -10`

* `ps aux | sort -nk +4 | tail`  
  `ps -eo pid,lstart,etime | grep 3208` view the start and running time of specify process.  
  `ps axjf` print process tree  
  `ps -ef` see every process   
  `ps -eLf` get about thread info  
  `ps -U root -u root u` see every process running as root  
  `pgrep firefox` print the procee id of firefox  

* `uname -a`

* `whereis cp`  
  `rpm -qf /usr/bin/cp`  
  `rpm -ivp ***.rpm`  

* `rpm -qpl packetname` list files  
  `rpm -i --relocate ` change install directory  
  `rpm -qa |grep XXX` check whether XXX was installed  
  `rpm -e $(rpm -qf $(which teamviewer))` remove software  

* `mkdir -p /test/dir1/dir2/dir3`

* `ftp 192.168.1.102 2121`  
  `pwd` view remote directory  
  `lcd` change to the local directory  
  `!ls` list local files  
  `put 1.jpg`  
  `get 2.jpg`  
  `mput *.jpg`  
  `mget *.jpg`download multiple files  
  `bye`  
* `wget ftp://IP:PORT/* --ftp-user=xxx --ftp-password=xxx -r` support download  
  multiple files and directories, good to replace `ftp`

#### awk and sed  
* awk  
  `awk FS'' 'condition1{operator1}condition2{operator2}...' filename`  
  NR:number row, NF:number field  
  `awk 'NR==1{print $0}'`  
  `arr=($(awk -F'#' '{print $1,$2,$3,$4}' $conf_file))`  
  `ps aux | awk 'NR==1{print $0}$3>10{print $0}'`  
  `awk -F'<|>' '{if(NF>3){print $2 ":" $3}}' /tmp/test.xml` parse xml file  
  `awk -F'=' '/HWADDR/{print $2}' /etc/sysconfig/network-scripts/ifcfg-eth0`

* sed  
  `sed [options] 'command' file(s)`  
  `sed [options] -f scriptfile file(s)`  
  `sed -i '/'$prj'/{s/\(.*#.*#\)[0-9]\+/\1'$rev_new'/}' $conf_file`  
  `sed -n 10,23p filepath` -n print the specific lines  
  `sed 'N;s/\n/ /' filepath` join two line into one  
  `echo -e "11\n22\n33\n" | sed -n '/22/{n;p}' ` print next line after match  
  `sed ':a;N;s/\n/ /;ba;' file` join all lines  

#### git and svn  
* git  
  `git clone url`  
  `git add .`  
  `git commit -m "***"`  
  `git commit file -m "***"`  
  `git push origin master`  
  `git status`  
  `git diff`  
  `git rm ***` remove file  
  `git rm -r ***` remove directory  
  `git pull`  
  `git mv *** ###`  
  `git remote -v` show remote repository info  
  `git reset <file>` undo git add  
  `git checkout HEAD /path/file` undo git rm on one file  
  `git rm $(git ls-files --deleted)` undo git rm multiple files  

* svn  
  `svn list url`  
  `svn co url`  
  `svn status`  
  `svn update`  
  `svn info url`  
  `svn di`  
  `svn di -r ver1:ver2`  
  `svn di -r ver1:ver2 path`  
  `svn revert [-R] path`  
  `svn merge -r ver2:ver1 path`  
  `svn log -r ver1:ver2`  
  `svn log -l5` show 5 latest logs  
  `svn add path`  
  `svn commit -m "***"`  
  `svn ci path -m "***"`  
  `ls ~/.subversion/auth/svn.simple` user information location  

#### network  
* `ip addr`  
  `cat /sys/class/net/eth{0,1}/address` display mac address  
  `ip addr add 172.16.60.69/24 dev eth1`  
* `ifconfig eth0 down; ifconfig eth0 hw ether MAC_ADDR; ifconfig eth0 up` change MAC address   
  `ifconfig eth2 192.168.1.102 netmask 255.255.255.0 up` add ip address for eth2  
  `ifconfig eth2:1 192.168.1.23 netmask 255.255.255.0 up` add another ip for eth2  
  `ifconfig eth0 mtu 6000` set MTU  
* `ping -I 192.16.4.23 192.16.4.70` set source address to specified interface address  
* `route add default gw 172.16.130.1 eth2`  
  `route del default gw 172.16.130.1 eth2`  
  `route add -host 192.168.168.110 dev eth0`  
  `route del -host 192.168.168.110 dev eth0`  
  `route add -net 172.16.130.0/24 gw 172.16.130.1 eth2`  
  `route del -net 172.16.0.0 netmask 255.255.0.0 dev eth0`  
  `ip route flush cache`  
* `python -m SimpleHTTPServer`  
  `python -m http.server` for windows  
* `ssh root@172.168.1.101`  
* `ssh -f -NC -D7070 user@shell.cjb.net` ssh tunnel  
  `ssh -f -NC -D7070 user@216.194.70.6` ssh tunnel 
* `scp abc.sh root@172.168.1.101:/root/test` upload file  
  `scp root@172.168.1.101:/root/test/abc.sh /root/mytest` download file  
  `sshpass -p passwd scp abc.sh root@172.168.1.101:/root/test`
* `nmap ip`  
  `nmap 192.168.1.1 -p 8000`  check the port is open or not  
  `nmap -v -sn 192.168.1.1/24`  
  `nmap -v -sn 192.168.1.1/24 |grep 'report' |grep -v 'down' |awk '{print $NF}'` scan local alive ip  
* `nc -z -w 1 IP PORT`  
  `nc -z -w 1 192.168.1.1 100`  
  `nc -z -w 1 -u 192.168.1.1 100`  
  `nc -z -w 1 192.168.1.100 1-65535` scan all ports from 1 to 65535  
* `lsof -i:111`  
  `netstat -apn | grep 111`  
  `iptables -L` list rules  
  `iptables -L INPUT --line-numbers`  
  `iptables -A INPUT -p tcp --dport 111 -j DROP` forbid specific port
  `iptables -A OUTPUT -p tcp --dport 111 -j DROP`  
  `iptables -A INPUT -p tcp --dport 8024 -j ACCEPT` all rule, allow port 8024  
  `iptables -I INPUT 5 -p tcp --dport 8024 -j ACCEPT` add rule to specify position(5)  
  `iptables -D INPUT -p tcp --dport 8024 -j ACCEPT` delete rule  
  `service iptables save`  
  `service iptables stop`  
  `service iptables start`  
* `curl ifconfig.me`  
  `curl ipinfo.io/IP_ADDRESS` get geographic location of an IP address  
* `geoiplookup IP_ADDRESS`  
* `dig domain`   `dig -x host`
* `netstat -nlp` view service and listen ports  
  `netstat -npt` view tcp connections  
  `netstat -s` display networking statistics  
* `wget url` download file  
  `wget -m -p -np -k -E http://site/path/` mirror the site  
  `wget -A pdf,jpg -m -p -np -k -E http://site/path/` only download pdf and jpg file  
  `wget url_file -O new_name.file` rename  
* `tcpdump -D` list available device(run as root)  
  `tcpdump -i p4p1 host 172.16.130.88 and port 80 -n` use specify interface "p4p1"   
  `tcpdump -i p4p1 host 172.16.130.88 and port 3260 -w tcpdump.pcap`  
  `tcpdump -l | tee dat`  
  `tcpdump -U host 172.16.130.88 and port 3260 -w tcpdump.pcap` flush each packet to packet-buffered  
  `tcpdump host 210.27.48.1 and \ (210.27.48.2 or 210.27.48.3 \)`  
  `tcpdump -i eth0 src host hostname`  
  `tcpdump -i eth0 dst host hostname`  
  `tcpdump -n -e -i eth0 icmp`  
  `tcpdump -n -e icmp`  
  `tcpdump -n -e arp`  
* `telnet 172.16.50.39 3128` check port is available or not
* `arp -a`  
* `squid -CNd1`  
* `traceroute www.baidu.com`  display all the route from my host to the website  

#### os
* Fedora  
  `Alt+Tab` switch windows  
  ``Alt+` ``  switch sub-windows  
  `Win` windows key, use to show all application  
  `Win` -> `Space` , search application  
* yum use local repository  
  - mount iso file to /mnt/cdrom
  - modify /etc/yum.repos.d/CentOS-Media.repo, add text "file:///mnt/cdrom/"  
  [c6-media]  
  name=CentOS-$releasever - Media  
  baseurl=file:///media/CentOS/  
          file:///media/cdrom/  
          file:///media/cdrecorder/  
          file:///mnt/cdrom/  
  gdgcheck=1  
  enabled=1  
  gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-6  
* `yum install XXX -y`  
  `yum search XXX `  
  `yum remove XXX `  
  `yum clean all`  
  `yum makecache`  
* Ubuntu  
  reset root password, press 'e' to edit grub boot option, change 'ro quiet splash' 
  to 'rw init=/bin/bash', then login terminal with root, use 'passwd' to reset.
* Ubuntu remove software
  - dpkg --get-selections | grep ‘software-name’
  - sudo apt-get remove --purge software_name
  - dpkg -l |grep ^rc|awk '{print $2}' |sudo xargs dpkg -P  

#### vimperator(Firfox plugin)
* vimperator  
  `:help`  Open help page  
  `:helpall`  Show all help on one page  
  `C-[`    cancel command mode  
  `d`      close current page  
  `u`      undo close current page  
  `/`      search
  `H`      Go back in the browser history  
  `gg`     Go to the top of the page 
  `G`      Go to the bottom of the page  
  `C-f`    forward page  
  `C-b`    backward page  
  `y`      Copy the current URL to clipboad  
  `p`      Open the URL from current clipboad contents in current buffer  
  `P`      Open the URL from current clipboad contents in a new tab buffer  
  `r`      Reload current page  
  `ZZ`     Quit and save the session  
  `C-n`    Go to the next buffer  
  `C-p`    Go to the previous buffer  
  `gi`     Go to input field  
  `g-u`    Go to the parent directory  
  `g-U`    Go to the root of the website  
  `c`      Start caret mode.Like normal mode of vim, use `h j k l` to move, and 
  press `v` to start visual mode, then press `y` to copy select text  
  `gi`     Use \[count\]gi to Focus to the \[count\]th input field  
  `C-l`    Go to URL editor and select it  
  `C-t`    Open a new tab  

#### program
* `man 2 sys_call_name` view system call function description  
  `man 2 open`, `man 2 read`, `man 2 write`, `man 2 fork`  
  `man 3 errno` view error number description  
  `man 3 printf`  
  `man ascii`  

#### Others  
* `vimdiff a.log b.log` diff two files
  `vim -b file` edit binary file, use `:%!xxd` and `:%!xxd -r`  
* `diff /tmp/test01 /tmp/test02` compare file in two directory  
  `diff -r /tmp/test01 /tmp/test02` compare file in directory and subdirectory  
  `diff /tmp/test01/1.c /tmp/test02/2.c` compare two files  
* patch  
  `diff a.c b.c > c.patch`  
  `patch a.c c.patch`  
* `vmstat iostat ifstat nload top hexdump od`
  `hexdump -C filename` display hex+ASCII of file  
  `iostat -x 2 5`  
  `ipcs` show all ipc info
  `nload` monitor network traffic, use `tab` key to switch to next interface, and `q` to quit.  
* `dd if=/dev/zero of=$test_file bs=1M count=$dev_size 2>> $log`  
  `dd of=/dev/sdh of=/dev/zero bs=1M count=500 oflag=direct` test the real hard disk speed  
  `dd if=path/Fedora-os.iso of=/dev/sdb` install fedora live os to U disk(make 
  sure the mount directory of U disk is /dev/sdb, if not should change the out file directory)  
* `> file.txt`
* `watch -n 5 command`
* `chroot .`
* `date "+%F %R:%S"`       
  `date "+%y%m%d%H%M%S"`  
  `date +%s` print timestamp of current time  
  `date -d @1420210697` translate timestamp to datetime   
  `date +%s -d"Jan 1, 1970 00:00:01"` translate datetime to timestamp   
* `which executefile` print directory of executefile  
  `ldd /usr/bin/executefile` print shared library dependencies  
  `nm /usr/lib64/libXXX.so` print interface  
* `cat /proc/uptime | awk -F. '{d=($1/86400);h=($1%86400)/3600;m=($1%3600)/60;s=($1%60);printf("system had run %d day %d hours %d mins %d secs\n",d,h,m,s)}'`
* `fuser -u /home`  
  `fuser -v -n tcp 7070`  
* `echo 1 > /proc/sys/kernel/sysrq` enable sysrq  
  `echo "b" > /proc/sysrq-trigger`  reboot  
  `echo "o" > /proc/sysrq-trigger`  shutdown  
  `shutdown -h now`                 shutdown  
  `shutdown -h +10` shutdown 10 minutes later  
  `shutdown -h 10:00` shutdown at ten clock  
* `dmidecode | more`    : view mainboard info   
  `cat /proc/cpuinfo`   : view CPU info  
  `cat /proc/pci`       : view pci info  
  `lspci `              : view PCI info  
  `cat /proc/meminfo`   : view memory info  
  `fdisk -l`            : view disk info  
  `df`                  : view disk space useage  
  `cat /proc/interrupts`: view interrupt request    
  `dmesg | more`        : view device debug message  
* `pdftk *.pdf cat output onelargepdfile.pdf` merge pdf to one  
  `pdftk test.pdf cat 1-3 6-20 22-end output net.pdf` cut page 4,5,21 from file  
  `qpdf --password=1234 --decrypt encrypted.pdf decrypted.pdf` decrypt pdf file  
  `qpdf --decrypt encrypted.pdf decrypted.pdf` for empty password   
* `vi -e -s -c ":%s/pattern/string/g" -c ":wq" file` execute vi comand in shell
* format u disk 
  `fdisk -l`            : find u disk  
  `umount /dev/sdb1`    : umount u disk  
  `mkfs.vfat /dev/sdb1` : format as fat  
* `history -c ` clean  
  `history -d offset` delete specify command  
* `convert -resize 1024x768 input.svg output.png` convert svg to png  
  `convert file.pdf file.png` convert pdf to images(file-XX.png)  
  `convert *.png file.pdf` convert images to one pdf  
  `convert -fill green -pointsize 40 -draw 'text 10,50 "funny day"' foo.png comment.png` add text to picture  
* `import foo.png` capture a select rectangular  
* `for i in `ls`; do mv -f $i `echo $i | sed 's/^........./iscsi/'`; done` rename 
    files, replace the first 5 characters with `iscsi`  
* `fc-list` list fonts  
* `strace -o 1.log -s 1024 -T -tt -p 1234` print the system call and time used for process 1234
* `printf` format and print data  
  `printf '%x\n' 1550` convert octal value 1550 to hex, output 60e   
  `printf '%d\n' 0x99` convert hex value 0x99 to octal, output 153   
* `echo "51200000/64/3“ |bc -l` calculate
* `echo 'abc 123dd   3xxx tt' |tr -s '[:space:]' '\n'` translate spaces to be line break symbol  
  `echo 'abc 123dT 3xXX tt' |tr '[:upper:]' '[:lower:]'`  
  `cat test.md |tr -s '[:space:]' '\n' |tr '[:upper:]' '[:lower:]' |sort |uniq -c| sort -nr |head -10`  
   find the top hot words for text document.
* `zip -Phq.hzy.zf.xf.hx.l@123 test.zip test` zip "test" file to be test.zip using password  
  `unzip -Phq.hzy.zf.xf.hx.l@123 test.zip` unzip zip file with password  
  `unzip -Z test.zip` unzip file without password  
  `cat abc.zip.00* >abc.zip` merge zip files  
* `pkill -9 -t pts/1`  
* `setenforce 0`  forbid Selinux temporary  
  `sed -i 's/SELINUX=.*/SELINUX=disabled/g' /etc/selinux/config` forbid Selinux, need reboot  
* `kill PID`  
  `kill -9 PID`  
  `kill -kill PID`  
* `vgchange -ay`  
* `lsof |grep '/core/messages'` find which application open the specify file
* `for x in `seq 1 1 10`; do ps -eo state,pid,cmd | grep "^D"; echo "----"; sleep 5; done`  
  print the processes in a "D" state every 5 seconds for 10 intervals 
* `echo export LANG=en_US.utf8 >>~/.profile`, set lang for os,  and checking with `echo $LANG` or `locale`
* `gstack threadID`
* `rm !(*.pdf)` remove files except pdf
* `[! -d /temp/test] && mkdir /tmp/test` if folder not exist, create it.

#### Reference
+ [你可能不知道的Shell](http://coolshell.cn/articles/8619.html)
+ [高效操作Bash](http://ahei.info/bash.htm)
+ [Bash readline 使用技巧](http://docs.huihoo.com/homepage/shredderyin/readline.html)
+ [Shell大杂烩](http://floss.zoomquiet.org/item20051011211622-frameset.html)
+ [每个Linux用户都应该知道的命令行技巧](http://blog.jobbole.com/54425/)
+ [Check network connection command](http://www.cyberciti.biz/faq/check-network-connection-linux/)
+ [50 Most Frequently Used UNIX / Linux Commands (With Examples)](http://www.thegeekstuff.com/2010/11/50-linux-commands/)
+ [Tcpdump how to – the linux network troubleshooter](http://albanianwizard.org/tcpdump-how-to-the-linux-troubleshooter.albanianwizard)
+ [20条Linux命令面试问答](http://linux.cn/article-4790-1.html)
+ [Linux tcpdump命令详解](http://www.cnblogs.com/ggjucheng/archive/2012/01/14/2322659.html)
+ [Nmap扫描原理与用法](http://blog.csdn.net/aspirationflow/article/details/7694274)
+ [Linux使用笔记: 更改RPM包的安装目录](http://easwy.com/blog/archives/change-rpm-package-installation-directories/)
