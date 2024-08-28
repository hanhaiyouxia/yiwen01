nmap -sV -v --script vuln 192.168.2.4
nmap  192.168.2.4  -p 1-3307 -sS  -sV -T4 -A  --min-rate 9000

