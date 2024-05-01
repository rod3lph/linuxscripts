- Install fail2ban
- copy fail2ban.conf to fail2ban.local and jail.conf to jail.local 
  ``` 
  cd /etc/fail2ban/
  grep -o '^[^#]*' fail2ban.conf > fail2ban.local
  grep -o '^[^#]*' jail.conf > jail.local
  ```
- modify ssh jail portion /etc/fail2ban/jail.local (24h = 3600x24 ; if permanent change bantime to -1)
  ``` 
  [sshd]
  enabled = true
  port = ssh
  filter = sshd
  logpath = /var/log/auth.log
  maxretry = 2
  findtime = 300
  bantime = 86400
  ignoreip = 127.0.0.1
  
  ```
- enable and run fail2ban service 
  ``` 
  service enable fail2ban now
  ```
- Check fail2ban status 
  ``` 
  [root@SFTP-ALMA-SLC01 fail2ban]# fail2ban-client status
  Status
  |- Number of jail:      1
  `- Jail list:   sshd
  
  ```
- Check list of banned IP addresses 
  ``` 
  [root@SFTP-ALMA-SLC01 fail2ban]# iptables -S | grep f2b
  -N f2b-sshd
  -A INPUT -p tcp -m multiport --dports 22 -j f2b-sshd
  -A f2b-sshd -s 61.177.172.185/32 -j REJECT --reject-with icmp-port-unreachable
  -A f2b-sshd -s 61.177.172.136/32 -j REJECT --reject-with icmp-port-unreachable
  -A f2b-sshd -s 218.92.0.76/32 -j REJECT --reject-with icmp-port-unreachable
  -A f2b-sshd -s 218.92.0.34/32 -j REJECT --reject-with icmp-port-unreachable
  -A f2b-sshd -s 218.92.0.31/32 -j REJECT --reject-with icmp-port-unreachable
  -A f2b-sshd -s 218.92.0.29/32 -j REJECT --reject-with icmp-port-unreachable
  -A f2b-sshd -s 218.92.0.27/32 -j REJECT --reject-with icmp-port-unreachable
  -A f2b-sshd -s 218.92.0.22/32 -j REJECT --reject-with icmp-port-unreachable
  -A f2b-sshd -s 218.92.0.113/32 -j REJECT --reject-with icmp-port-unreachable
  -A f2b-sshd -s 180.191.190.160/32 -j REJECT --reject-with icmp-port-unreachable
  -A f2b-sshd -s 180.101.88.247/32 -j REJECT --reject-with icmp-port-unreachable
  -A f2b-sshd -j RETURN
  
  ```
- Un-ban IP address 
  ``` 
  fail2ban-client set sshd unbanip 10.11.10.51
  ```
- Recent fail2ban actions 
  ``` 
   tail -f /var/log/fail2ban.log
  ```
-