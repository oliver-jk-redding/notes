#Notes on using Unix

####Command to find and print any files on system with SETUID enabled
find / -xdev \( -perm -4000 \) -type f -print0 | xargs -0 ls -l

####Command to find and print any files on system with SETGID enabled
find / -xdev \( -perm -2000 \) -type f -print0 | xargs -0 ls -l