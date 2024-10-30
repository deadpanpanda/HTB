Target machine IP - 10.10.10.245

Performed 3 nmap scans

nmap -T4 -p- [10.10.10.245] -oN nmap_p.txt
nmap -T4 -p[21,22,80] -A [10.10.10.245] -oN nmap_pa.txt
nmap -T4 -sU [10.10.10.245] -oN nmap_su.txt

3 ports identified as open. 21, 22, and 80. Second scan performed more aggressive scan on target ports to gather more information.

Attempted anonymous login on FTP, unsuccessful.

Attempted banner grab on SSH, no new information.

Opened 10.10.10.245 in browser, found security dashboard with user Nathan logged in. Explored security dashboard, found multiple directories. One was titled security snapshot, which creates a .pcap file. 

Performed FFUF against http://10.10.10.245/data/FUZZ to check for alternative pcap files. FFUF results came back with many positives. 

http://10.10.10.245/data/0 caught my attention as first possible capture. Downloaded .pcap and opened in Wireshark, applying filters to investigate for useful information. FTP filter discovered a login and password.

FTP login success using details. Though no further success, FTP dead end. Considering original available ports, SSH is next option.

SSH with username and password successful. Able to secure user flag.

To access root, would need to find vulnerability. Given that OS is linux, resorted to linpeas upload to server to find vulnerability. Copied linpeas script into directory, created python3 -m http.server to upload to target.

curl on target allowed to transfer of linpeas script, which started running. Found python3.8 setuid vulnerability and utilised that to change to root.

Redirected to root directory and found root flag.
