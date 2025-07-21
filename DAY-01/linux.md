# Linux for Cloud - Complete Guide

## 1. File System Hierarchy and Permissions

### Essential Directory Structure
```bash
/           # Root directory
├── /bin    # Essential user binaries
├── /sbin   # System binaries
├── /etc    # Configuration files
├── /var    # Variable data (logs, databases)
├── /tmp    # Temporary files
├── /home   # User home directories
├── /usr    # User programs and data
├── /opt    # Optional software packages
└── /proc   # Process and kernel information
```

### File Permissions Deep Dive

**Permission Types:**
- `r` (read) = 4
- `w` (write) = 2  
- `x` (execute) = 1

**Permission Groups:**
- Owner (u) - First 3 bits
- Group (g) - Second 3 bits
- Others (o) - Third 3 bits

**Commands You Must Know:**
```bash
# View permissions
ls -la filename
stat filename

# Change permissions (numeric)
chmod 755 script.sh    # rwxr-xr-x
chmod 644 config.txt   # rw-r--r--

# Change permissions (symbolic)
chmod u+x script.sh    # Add execute for owner
chmod g-w file.txt     # Remove write for group
chmod o=r file.txt     # Set others to read only

# Change ownership
chown user:group filename
chown -R user:group directory/

# Set default permissions
umask 022              # Default: files 644, dirs 755
```

**Advanced Permission Concepts:**
```bash
# Special permissions
chmod +s filename      # Set SUID/SGID
chmod +t directory/    # Sticky bit

# Access Control Lists (ACLs)
getfacl filename
setfacl -m u:username:rwx filename
```

## 2. Process Management

### Process Monitoring Commands
```bash
# View running processes
ps aux                 # All processes, detailed
ps -ef                 # Full format listing
ps -u username         # Processes by user
pstree                 # Process tree view

# Real-time monitoring
top                    # Interactive process viewer
htop                   # Enhanced version of top
iotop                  # I/O monitoring
```

### Process Control
```bash
# Background/Foreground jobs
command &              # Run in background
jobs                   # List active jobs
fg %1                  # Bring job to foreground
bg %1                  # Send job to background
nohup command &        # Run immune to hangups

# Process signals
kill PID               # Terminate process (SIGTERM)
kill -9 PID            # Force kill (SIGKILL)
kill -HUP PID          # Hangup signal (reload config)
killall process_name   # Kill all instances
pkill -f pattern       # Kill by pattern matching

# Find processes
pgrep nginx            # Find process IDs
pidof nginx            # Get PID of running process
```

### Advanced Process Management
```bash
# Process priorities
nice -n 10 command     # Start with lower priority
renice 5 -p PID        # Change priority of running process

# Process monitoring
watch -n 1 'ps aux | grep nginx'  # Monitor every second
lsof -p PID            # Files opened by process
strace -p PID          # System calls made by process
```

## 3. System Monitoring and Performance

### Disk Usage and Management
```bash
# Disk space monitoring
df -h                  # Human readable disk usage
du -sh /path/*         # Directory sizes
du -h --max-depth=1    # One level deep

# Find large files
find /path -size +100M -type f -exec ls -lh {} \;
find /var/log -name "*.log" -size +50M

# Disk I/O monitoring
iostat -x 1            # Extended I/O statistics
iotop                  # Real-time I/O monitoring
lsblk                  # List block devices
```

### Memory Monitoring
```bash
# Memory usage
free -h                # Human readable memory info
cat /proc/meminfo      # Detailed memory information
vmstat 1               # Virtual memory statistics

# Memory-intensive processes
ps aux --sort=-%mem | head -10
```

### System Performance
```bash
# CPU monitoring
top -o %CPU            # Sort by CPU usage
mpstat 1               # CPU statistics
sar -u 1               # CPU utilization

# Load average
uptime                 # System uptime and load
w                      # Who is logged in and load

# Network monitoring
netstat -tuln          # Network connections
ss -tuln               # Modern replacement for netstat
iftop                  # Network bandwidth monitoring
```

## 4. Log Management

### Essential Log Locations
```bash
/var/log/messages      # General system messages
/var/log/syslog        # System log
/var/log/auth.log      # Authentication logs
/var/log/kern.log      # Kernel messages
/var/log/cron.log      # Cron job logs
/var/log/nginx/        # Web server logs
/var/log/mysql/        # Database logs
```

### Log Analysis Commands
```bash
# View logs
tail -f /var/log/messages          # Follow log in real-time
tail -n 100 /var/log/auth.log      # Last 100 lines
head -n 50 logfile                 # First 50 lines
less /var/log/syslog               # Paginated view

# Search logs
grep "ERROR" /var/log/application.log
grep -i "failed" /var/log/auth.log
grep -v "DEBUG" /var/log/app.log   # Exclude DEBUG lines
zgrep "pattern" /var/log/old.log.gz # Search compressed logs

# Log rotation
logrotate -d /etc/logrotate.conf   # Dry run
logrotate -f /etc/logrotate.conf   # Force rotation
```

### systemd Journaling
```bash
# View system journal
journalctl                         # All logs
journalctl -f                      # Follow logs
journalctl -u nginx                # Specific service
journalctl --since "2024-01-01"   # Since date
journalctl --since "1 hour ago"   # Relative time
journalctl -p err                  # Error priority only
journalctl --disk-usage           # Journal disk usage
```

## 5. Package Management

### APT (Debian/Ubuntu)
```bash
# Update package lists
apt update

# Upgrade packages
apt upgrade                # Upgrade installed packages
apt full-upgrade          # Handle changing dependencies

# Package installation/removal
apt install package_name
apt remove package_name
apt purge package_name    # Remove with config files
apt autoremove           # Remove unused dependencies

# Package information
apt list --installed     # List installed packages
apt search keyword       # Search packages
apt show package_name    # Package details
dpkg -l                  # List all packages
```

### YUM/DNF (Red Hat/CentOS/Fedora)
```bash
# Update system
yum update               # RHEL/CentOS
dnf update               # Fedora

# Package management
yum install package_name
yum remove package_name
yum search keyword
yum info package_name

# Repository management
yum repolist
yum-config-manager --enable repo_name
```

## 6. SSH and Remote Access

### SSH Basics
```bash
# Connect to remote server
ssh user@hostname
ssh -p 2222 user@hostname         # Custom port
ssh -i ~/.ssh/private_key user@host # Specific key

# SSH configuration
~/.ssh/config                     # User SSH config
/etc/ssh/sshd_config             # Server SSH config

# Example SSH config
Host myserver
    HostName 192.168.1.100
    User admin
    Port 2222
    IdentityFile ~/.ssh/mykey
```

### SSH Key Management
```bash
# Generate SSH keys
ssh-keygen -t rsa -b 4096 -C "your_email@domain.com"
ssh-keygen -t ed25519 -C "your_email@domain.com"

# Copy public key to server
ssh-copy-id user@hostname
# Manual method
cat ~/.ssh/id_rsa.pub | ssh user@hostname 'cat >> ~/.ssh/authorized_keys'

# SSH agent
ssh-agent bash           # Start SSH agent
ssh-add ~/.ssh/id_rsa   # Add key to agent
ssh-add -l              # List loaded keys
```

### SSH Security
```bash
# Disable password authentication (in sshd_config)
PasswordAuthentication no
PubkeyAuthentication yes
PermitRootLogin no
AllowUsers specific_user

# Restart SSH service
systemctl restart sshd
```

## 7. Cron Jobs and Task Scheduling

### Cron Syntax
```bash
# Format: minute hour day month day-of-week command
# * * * * * command
# | | | | |
# | | | | +-- Day of week (0-7, Sunday = 0 or 7)
# | | | +---- Month (1-12)
# | | +------ Day of month (1-31)
# | +-------- Hour (0-23)
# +---------- Minute (0-59)
```

### Cron Management
```bash
# Edit crontab
crontab -e               # Edit current user's crontab
crontab -l               # List current user's crontab
crontab -r               # Remove current user's crontab
crontab -u username -e   # Edit another user's crontab

# System-wide cron
/etc/crontab            # System crontab
/etc/cron.d/            # Additional cron files
/etc/cron.daily/        # Daily scripts
/etc/cron.hourly/       # Hourly scripts
```

### Cron Examples
```bash
# Every minute
* * * * * /path/to/script.sh

# Daily at 2:30 AM
30 2 * * * /backup/daily_backup.sh

# Every Monday at 9 AM
0 9 * * 1 /scripts/weekly_report.sh

# Every 15 minutes
*/15 * * * * /monitoring/check_service.sh

# First day of month at midnight
0 0 1 * * /scripts/monthly_cleanup.sh
```

## 8. systemd Services

### Service Management
```bash
# Service control
systemctl start service_name
systemctl stop service_name
systemctl restart service_name
systemctl reload service_name     # Reload config without restart

# Service status
systemctl status service_name
systemctl is-active service_name
systemctl is-enabled service_name

# Enable/disable services
systemctl enable service_name     # Start at boot
systemctl disable service_name
systemctl mask service_name       # Prevent starting
```

### Creating Custom Services
```bash
# Service file location
/etc/systemd/system/myapp.service

# Example service file
[Unit]
Description=My Application
After=network.target

[Service]
Type=simple
User=myuser
WorkingDirectory=/opt/myapp
ExecStart=/opt/myapp/start.sh
ExecReload=/bin/kill -HUP $MAINPID
Restart=always

[Install]
WantedBy=multi-user.target

# Reload systemd and start service
systemctl daemon-reload
systemctl enable myapp.service
systemctl start myapp.service
```

## 9. Text Processing and Command Line Tools

### Essential Text Tools
```bash
# grep - pattern matching
grep "pattern" file.txt
grep -r "pattern" /directory/     # Recursive search
grep -i "pattern" file.txt        # Case insensitive
grep -v "pattern" file.txt        # Invert match
grep -n "pattern" file.txt        # Show line numbers

# sed - stream editor
sed 's/old/new/g' file.txt        # Replace all occurrences
sed '1,5d' file.txt               # Delete lines 1-5
sed -n '10,20p' file.txt          # Print lines 10-20

# awk - text processing
awk '{print $1}' file.txt         # Print first column
awk -F: '{print $1}' /etc/passwd  # Custom delimiter
awk '/pattern/ {print $0}' file.txt # Print matching lines

# cut - extract columns
cut -d: -f1 /etc/passwd           # First field, colon delimiter
cut -c1-10 file.txt               # Characters 1-10

# sort and uniq
sort file.txt                     # Sort lines
sort -n numbers.txt               # Numeric sort
sort -k2 file.txt                 # Sort by second column
uniq file.txt                     # Remove duplicates
sort file.txt | uniq -c           # Count occurrences
```

### Advanced Text Processing
```bash
# Find and replace across files
find /path -name "*.conf" -exec sed -i 's/old/new/g' {} \;

# Extract information from logs
awk '/ERROR/ {print $1, $2, $NF}' /var/log/app.log

# Process CSV files
awk -F, '{sum+=$3} END {print "Total:", sum}' data.csv

# Complex grep patterns
grep -E "(error|ERROR|Error)" /var/log/messages
grep -P "\d{1,3}\.\d{1,3}\.\d{1,3}\.\d{1,3}" access.log
```

## 10. Network Troubleshooting

### Network Diagnostics
```bash
# Connectivity testing
ping hostname                     # Basic connectivity
ping -c 4 8.8.8.8                # Send 4 packets
traceroute hostname              # Route tracing
mtr hostname                     # Continuous traceroute

# DNS resolution
nslookup hostname
dig hostname
dig @8.8.8.8 hostname           # Specific DNS server
host hostname

# Port testing
telnet hostname port
nc -zv hostname port             # Netcat port check
nmap -p 80,443 hostname         # Port scanning
```

### Network Configuration
```bash
# Network interface info
ip addr show                     # IP addresses
ip route show                    # Routing table
ip link show                     # Network interfaces

# Legacy commands (still useful)
ifconfig                         # Interface configuration
route -n                         # Routing table
netstat -rn                      # Routing table

# Network connections
netstat -tuln                    # Listening ports
ss -tuln                         # Modern alternative
lsof -i :80                      # What's using port 80
```

## Sample Interview Questions and Answers

### 1. How do you find files consuming the most disk space?
```bash
# Method 1: Using du and sort
du -h /path | sort -hr | head -10

# Method 2: Find large files specifically  
find /path -size +100M -type f -exec ls -lh {} \; | sort -k5 -hr

# Method 3: For current directory
du -sh * | sort -hr
```

### 2. Explain the difference between soft and hard links
**Hard Link:**
- Points directly to inode
- Same file, different name
- Cannot cross file systems
- Cannot link directories
```bash
ln source_file hard_link
```

**Soft Link (Symbolic):**
- Points to filename
- Can cross file systems
- Can link directories
- Breaks if original deleted
```bash
ln -s source_file soft_link
```

### 3. How would you check which process is using port 8080?
```bash
# Method 1: netstat
netstat -tuln | grep :8080
netstat -tulp | grep :8080      # With process names

# Method 2: ss (modern approach)
ss -tuln | grep :8080
ss -tulp | grep :8080

# Method 3: lsof
lsof -i :8080

# Method 4: fuser
fuser 8080/tcp
```

### 4. What's the difference between systemctl and service commands?
**systemctl (systemd):**
- Modern init system
- More features and control
- Better dependency management
- Built-in logging with journald
```bash
systemctl start nginx
systemctl status nginx
```

**service (SysV init):**
- Legacy init system
- Simpler but limited
- Still works as wrapper on systemd systems
```bash
service nginx start
service nginx status
```

### 5. How do you set up password-less SSH authentication?
```bash
# Step 1: Generate key pair (on client)
ssh-keygen -t rsa -b 4096

# Step 2: Copy public key to server
ssh-copy-id user@server
# Or manually:
cat ~/.ssh/id_rsa.pub | ssh user@server 'cat >> ~/.ssh/authorized_keys'

# Step 3: Set proper permissions on server
chmod 700 ~/.ssh
chmod 600 ~/.ssh/authorized_keys

# Step 4: Configure SSH client (optional)
# ~/.ssh/config
Host myserver
    HostName server.example.com
    User myuser
    IdentityFile ~/.ssh/id_rsa
```

## Troubleshooting Scenarios

### High CPU Usage Investigation
```bash
# 1. Identify high CPU processes
top -o %CPU
ps aux --sort=-%cpu | head -10

# 2. Analyze specific process
strace -p PID                    # System calls
lsof -p PID                      # Open files
cat /proc/PID/status             # Process status

# 3. Historical data
sar -u                           # CPU utilization history
```

### Memory Issues Debugging
```bash
# 1. Check memory usage
free -h
cat /proc/meminfo

# 2. Find memory-hungry processes
ps aux --sort=-%mem | head -10
pmap PID                         # Memory map of process

# 3. Check for memory leaks
valgrind --leak-check=full program
```

### Disk Space Issues
```bash
# 1. Find large directories
du -h / | sort -hr | head -20

# 2. Find large files
find / -size +1G -type f 2>/dev/null

# 3. Check for deleted files still held open
lsof +L1                         # Files deleted but still open

# 4. Clean up common locations
# Clean logs
journalctl --vacuum-time=7d
# Clean package cache
apt clean
# Clean temporary files
find /tmp -type f -atime +7 -delete
```

## Best Practices for DevOps

### Security Hardening
- Disable root login via SSH
- Use key-based authentication
- Keep systems updated
- Configure firewall (ufw/iptables)
- Regular security audits
- Monitor logs for suspicious activity
- Use fail2ban for intrusion prevention

### System Administration
- Regular backups and test restores
- Monitor disk space and inodes
- Set up log rotation
- Document system changes
- Use configuration management
- Implement monitoring and alerting
- Maintain system documentation

### Performance Optimization
- Monitor system metrics regularly
- Optimize cron jobs timing
- Clean up unnecessary services
- Use appropriate file systems
- Implement proper caching
- Regular performance baselines
- Capacity planning

This comprehensive guide covers the essential Linux knowledge needed for DevOps. Practice these commands and concepts hands-on to build confidence and understanding.