- [[MySQL/MariaDB]]
- [[Docker]]
- [[KVM Bridging]]
- [[Proxmox]]
- [[OpenWRT]]
- [[Openvpn]]
- [[File Server]]
- [[OpenSSL]]
- [[Easyrsa]]
  collapsed:: true
	- Openssl easyrsa cycle [[Generating certificate]]
- [[Imagemagick]]
- [[Add new disk]]
- [[DNS]]
- [[NFS]]
- [[Networking]]
- [[KVM]]
- [[Disk Quota]]
- [[Secure PDF]]
- [[Nmap]]
- [[Alpine]]
- [[LED control]]
- [[Image files]]
- [[UbuntuServer]]
- [[S2S VPN]]
- [[Move /var to another disk]]
- [[Clone linux users to another linux box]]
- [[Login banner]]
- [[SSH Cert Auth]]
- [[SSH Key Authentication]]
- [[SSH Home Folder Jail]]
- [[Sample script to make/remove user accounts]]
- [[Fail2ban]]
- [[Firewalld]]
- [[Cloudflare]]
- [[Security]]
- [[Linux User Administration]]
- [[dietpi]]
- [[Nextcloud]]
- [[Simple router]]
- [[Simple Server]]
- [[Replicating Folders]]
- [[etc-network-interfaces]]
- [[Youtube and Media]]
- [[Apt Repo]]
- [[journalctl]]
- Ansible Note (temporary. This will have it's own page)
	- Hostkey checking - usually a remote server will have it's fingerprint registered in known_hosts file. This confirms the connection is made from a legitimate source. This is a safeguard against MIM attacks but does not prevent the connection to be established but leaves a trace in known_hosts.
	- Ansible may run into an issue on instances where the remote machine has been reinstalled and the fingerprint has changed. to circumvent this issue we may add the parameter below on ansible.cfg 
	  ``` 
	  [defaults]
	  host_key_checking = False
	  ```
- Inode (check inode limit if unable to write file to directory `ENOSPC` Disk full error) 
  ```
  # check block device inode
  > df -i
  Filesystem       Inodes  IUsed    IFree IUse% Mounted on
  tmpfs           2028949   1606  2027343    1% /run
  efivarfs              0      0        0     - /sys/firmware/efi/efivars
  /dev/sdb2      15630336 895512 14734824    6% /
  tmpfs           2028949   1099  2027850    1% /dev/shm
  tmpfs           2028949      6  2028943    1% /run/lock
  tmpfs           2028949      1  2028948    1% /run/qemu
  /dev/sdb1             0      0        0     - /boot/efi
  /dev/sdb3       7815168  68623  7746545    1% /persist
  /dev/sdb5      23483088 570737 22912351    3% /home
  tmpfs            405789    267   405522    1% /run/user/1000
  
  # show inodes used by directory
  > ls -id /home/rodel
  19042561 /home/rodel
  
  > ls -i /home/rodel
  19097409 '2023-12-07 12-33-02.mkv'
  20232331  apps
  19101365  cgrun
  19042582  Desktop
  21933485  docker
  19042586  Documents
  19042583  Downloads
  ```
	- Nifty trick: create empty files and use up all available inodes (try only on a vm) (To Verify: if I mount this "filled" disk on another VM), should result to `ENOSPC` ("No space left on device")
	  ``` 
	  n=0; while :; do touch $n; let n=n+1; done

- Confirm MTU size 
  ``` 
  ping 192.168.200.24 -c 2 -M do -s $((1500-28))
  ```
- Show Linux info 
  ``` 
  clear; neofetch; echo "My Python version :";python3 -V; echo "";echo "My Network Addresses :"; ip -f inet addr show| awk '/inet / {print $2}'; echo ""; echo "My Public IP :";curl icanhazip.com; echo ""; lsblk -p; echo ""
  ```
- Public IP 
  ``` 
  ip=$(curl -s ifconfig.me/ip);echo "My Public IP: $ip"
  ```
- Disable root login 
  #+BEGIN_QUOTE
  Does not affect su,sudo
  Change from
  root:x:0:0:root:/root:/bin/bash
  to
  root:x:0:0:root:/root:/sbin/nologin
  #+END_QUOTE
- rhel check install date 
  ``` 
  
  ```
- remove remarks on script 
  ``` 
  grep -o '^[^#]*' file
  ```
- Change ip address 
  ``` 
  $ ifconfig enp0s3 192.168.178.32/24
  $ ifconfig enp0s3 192.168.178.32 netmask 255.255.255.0
  ```
- Get machine details and serial number
  ``` 
  sudo neofetch; sudo dmidecode -s system-serial-number; sudo lsblk
  ```
- check SD card or USB 
  ``` 
  sudo dnf install f3
  f3write /run/media/your/disk/
  f3read /run/media/your/disk/
  ```
- list latest 10 files 
  ``` 
  ls -alt <folder> | head -n 11
  ```
- check port open 
  ``` 
  nc -zvw10 192.168.0.1 22
  date; hostname -i | awk '{print $1}'; nc -vzw10 rpmwhsbidw 1433
  
  # using curl
  curl -v telnet://10.140.70.10:443
  
  ```
- Azure hosted ubuntu - make sure that AD domain resolution works (modify file /etc/systemd/resolved.config) (no issue with debian)
  ``` 
  [Resolve]
  DNS=10.0.11.xxx 10.0.12.xxx
  FallbackDNS=10.xx.x.xxx 168.63.129.16
  Domains=domain.net
  ```
- fix fat32 
  ``` 
  sudo dosfsck -w -r -l -a -v -t /dev/sdx
  ```
- fix ext3/ext4 
  ``` 
  #make sure disk is not mounted
  fsck.ext4 /dev/sdb1
  fsck.ext4 -p /dev/sdd1
  fsck.ext4 -fvy /dev/sdb1
  ```
- create dynamic qcow2 disk 
  ``` 
  sudo qemu-img create -f qcow2 -o preallocation=off dynamic.qcow2 60G
  ```
- convert virtualbox .vdi to qcow2 
  ``` 
  qemu-img convert -f vdi -O qcow2 vm.vdi vm.qcow2
  ```
- convert jpg to pdf  (Install ImageMagick first)
  ``` 
  convert file.jpg file.pdf
  ```
- make picture file size smaller 
  ``` 
  convert 1657145825113.jpg -quality 95 selfiesss.jpg
  ```
- make video smaller 
  ``` 
  ffmpeg -i inputfile.mkv MaeganLaguisma_readingpt.mp4
  
  #convert all mkv files to smaller mp4
  for file in *.mkv; do ffmpeg -i "$file" "${file%.mkv}.mp4"; done
  ```
- merge pdf files 
  ``` 
  pdftk recipe-1.pdf recipe-2.pdf recipe-3.pdf recipe-4.pdf recipe-5.pdf cat output recipes.pdf
  pdftk recipe-*.pdf cat output recipes.pdf
  
  #Install poppler to use pdfunite <sudo dnf install poppler-utils>
  pdfunite recipe-*.pdf recipes.pdf
  
  convert pdf1.pdf pdf3.pdf pdf2.pdf final_pdf.pdf
  # Merge specific pages from PDFs
  convert pdf1.pdf[0-3] pdf2.pdf[5-7] final_pdf.pdf
  ```
	- #+BEGIN_QUOTE
	  There are several popular desktop apps to merge PDF files. Some apps include PDF Arranger, LibreOffice Draw, PDF Chain, PDFSam, PDF Shuffler, and PDFmod.
	  
	  flatpak install flathub com.github.jeromerobert.pdfarranger
	  flatpak install flathub net.sourceforge.pdfchain
	  flatpak run net.sourceforge.pdfchain
	  
	  #+END_QUOTE
- Extract pages into multiple PDFs with pdfseparate command 
  ``` 
  pdfseparate final_pdf.pdf final_pdf-page_%d.pdf
  # You can also export a range of pages
  pdfseparate -f 25 -l 31 FOSSBook.pdf FOSSBook-page_%d.pdf
  ```
- Flatpak update app 
  ``` 
  sudo flatpak update -y com.wps.Office
  ```
- youtubedl-gui 
  ``` 
  flatpak install flathub io.github.JaGoLi.ytdl_gui
  ```
- create 1GB file 
  ``` 
  dd if=/dev/urandom of=file.txt bs=1048576 count=1000
  dd if=/dev/zero of=testfile.img bs=512M count=2
  ```
- delete specific files in subdirectories 
  ``` 
  find . -name \*.swp -type f -delete
  
  find . -name \*.swp -type f -print0  | xargs -0 rm -f
  
  Piping the output to xargs is used form more complex per-file commands than is possible 
  with -exec. The option -print0 tells find to separate matches with ASCII NULL instead 
  of a newline, and -0 tells xargs to expect NULL-separated input. 
  This makes the pipe construct safe for filenames containing whitespace. 
  ```
- fstab entry for CIFS 
  ``` 
  //awfsinf02pv/FileEncrypt/CompassAnalytics /home/rpmadmin/mount cifs rw,vers=3.0,sec=ntlm,cache=strict,username=fencrypt,domain=ROUNDPOINT,password=MboS1603*,uid=0,noforceuid,gid=0,noforcegid,addr=10.140.4.22,file_mode=0777,dir_mode=0777,nounix,serverino,mapposix,rsize=61440,wsize=65536,echo_interval=60,actimeo=1 0 0
  ```
- fstab entry using UUID 
  ``` 
  UUID=b28464db-0f38-4471-963d-61a18a571da0 /disks/mount ext4 rw,relatime,defaults,errors=remount-ro 0 0
  UUID=1ce070e0-f04f-4407-b2b4-2f9d3cde806d /disks/media ext4 rw,relatime,defaults,errors=remount-ro 0 0
  ```
- fstab entry for NFS 
  ``` 
  192.168.200.21:/disks/mount /disks/mount nfs4 rw,relatime,vers=4.2,rsize=1048576,wsize=1048576,namlen=255,hard,proto=tcp,timeo=600,retrans=2,sec=sys,clientaddr=192.168.200.24,local_lock=none,addr=192.168.200.21 0 0
  ```
- fedora repo list 
  ``` 
  # dnf repolist --all
  # dnf config-manager --set-enabled <repo-name>  ...
  # dnf config-manager --set-disabled <repo-name> ...
  ```
- create and restore image dd 
  ``` 
  sudo dd if=/dev/sdb1 bs=128k conv=sync,noerror status=progress | gzip -c > mountainlion-usb.gz
  
  #Restore to usb
  gunzip -c centos-core-7.gz | dd of=/dev/da0
  
  #Store to remote server
  dd if=/dev/da0 conv=sync,noerror bs=128K | gzip -c | ssh vivek@server1.cyberciti.biz dd of=centos-core-7.gz
  
  ### Imaaging disk MBR
  dd command for two discs with different size partitions
  # dd if=/dev/sda of=/tmp/mbrsda.bak bs=512 count=1
  
  Now to restore the image to any sdb:
  # dd if=/tmp/mbrsda.bak of=/dev/sdb bs=446 count=1
  
  To Backup /dev/sda MBR, enter:
  # dd if=/dev/sda of=/tmp/backup-sda.mbr bs=512 count=1
  
  Next, backup entries of the extended partitions:
  # sfdisk -d /dev/sda > /tmp/backup-sda.sfdisk
  
  Copy /tmp/backup-sda.sfdisk and /tmp/backup-sda.mbr to USB pen or somewhere else safe over the network based nas server.
  
  Task: Restore MBR and Extended Partitions Schema
  To restore the MBR and the extended partitions copy backup files from backup media and enter:
  # dd if=backup-sda.mbr of=/dev/sda
  # sfdisk /dev/sda < backup-sda.sfdisk
  
  ```
- Install mssql client on ubuntu 
  ``` 
  sudo su
  curl https://packages.microsoft.com/keys/microsoft.asc | apt-key add -
  curl https://packages.microsoft.com/config/ubuntu/22.04/prod.list > /etc/apt/sources.list.d/mssql-release.list
  exit
  sudo apt-get update
  sudo ACCEPT_EULA=Y apt-get install msodbcsql17 mssql-tools
  echo 'export PATH="$PATH:/opt/mssql-tools/bin"' >> ~/.bash_profile
  echo 'export PATH="$PATH:/opt/mssql-tools/bin"' >> ~/.bashrc
  source ~/.bashrc
  sudo apt-get install unixodbc-dev
  
  #enable call command anywhere
  sudo ln -sfn /opt/mssql-tools/bin/sqlcmd /usr/bin/sqlcmd
  #test connection
  sqlcmd -S <remoteip> -U <user> -p -Q "SELECT @@VERSION"
  ```
- mkdir with chmod 
  ``` 
  mkdir -m a=rwx [directories]
  mkdir -m 777 <file>
  ```
- mkdir including parent directories(if not exist) 
  ``` 
  mkdir -p foo/bar/zoo/andsoforth
  ```
- find folders less than 80MB and delete 
  ``` 
  while read -r -d '' size dir; do [[ $size -lt 80 ]] && echo rm -rf "$dir"; done < <(find -depth -type d -exec du -0sm {} \;)
  ```
