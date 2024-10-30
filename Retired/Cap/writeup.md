Target machine IP - 10.10.10.245

Performed 3 nmap scans

nmap -T4 -p- [10.10.10.245] -oN nmap_p.txt
nmap -T4 -p[21,22,80] -A [10.10.10.245] -oN nmap_pa.txt
nmap -T4 -sU [10.10.10.245] -oN nmap_su.txt

3 ports identified as open. 21, 22, and 80. Second scan performed more aggressive scan on target ports to gather more information.

Attempted anonymous login on FTP, unsuccessful.

Attempted banner grab on SSH, no new information.

Opened 10.10.10.245 in browser, found security dashboard with user Nathan logged in.
