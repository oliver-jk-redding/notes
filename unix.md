# Notes on using Unix

## Finding stuff

### Command to find and print any files on system with SETUID enabled
```bash
find / -xdev \( -perm -4000 \) -type f -print0 | xargs -0 ls -l
```
### Command to find and print any files on system with SETGID enabled
```bash
find / -xdev \( -perm -2000 \) -type f -print0 | xargs -0 ls -l
```

## Sudo

### Command to login as sudo
```bash
sudo -i
```
### Command to login as different user
```bash
sudo -u <user> -i
```

## Permissions

### Set 755 (drwxr-xr-x) permissions on all directories
```bash
find /path/ -type d -exec chmod 755 {} \;
```

### Set 644 (-rw-r--r--) permissions on all files
```bash
find /path/ -type f -exec chmod 644 {} \;
```
