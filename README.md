<h1>Linux CLI Lab</h1>



<h2>Languages and Utilities Used</h2>

- <b>Linux</b> 


<h2>Environments Used </h2>

- <b>Ubuntu terminals</b> 
- <b>VM Ware</b>

<h2>Program walk-through:</h2>

<p align="center">
In this Antisyphon lab (part of their fantastic SOC Core skills course with John Strand), I was looking at a backdoor using the Linux CLI.

I used a large number of basic commands to get a better understanding of what the backdoor is and what it does.

For this lab I ran three different Ubuntu terminals.

The first was where we run the backdoor.

The second was where we connect to it.

The third was where we ran the analysis.

On the Linux terminal, I used sudo su - to get a root prompt. This was so we could have a backdoor running as root and a connection from a different user account on the system.:<br/>
<img src="https://imgur.com/RD2kkQd.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />

  <p align="center">
I next needed to create a fifo backpipe:
    mknod backpipe p
Next, let's start the backdoor with the following command:

/bin/bash  0<backpipe | nc -l 2222 1>backpipe

This was complicated for me but this basically created a backdoor listening on port 2222 of the linux system. <br/>
<img src="[https://github.com/mdnorris1/LinuxCLILab/upload/main/ifconfig.png](https://github.com/mdnorris1/LinuxCLILab/blob/main/assets/ifconfig.png)" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />

<!--
 ```diff
- text in red
+ text in green
! text in orange
# text in gray
@@ text in purple (and bold)@@
```
--!>
