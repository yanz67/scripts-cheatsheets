## Linux Cheat Sheet

1. Disable sleep when closing the laptop lid [StackOverflow Solution](https://askubuntu.com/questions/141866/keep-ubuntu-server-running-on-a-laptop-with-the-lid-closed/594417#594417)
    - edit `/etc/systemd/logind.conf`
    - set `HandleLidSwitch=ignore`
    - set `LidSwitchIgnoreInhibited=no`
    - run `sudo service systemd-logind restart`
 
 2. Disable SSH password login [NixCraft Tutorial](https://www.cyberciti.biz/faq/how-to-disable-ssh-password-login-on-linux/)
    - add user to sudo group `usermod -aG sudo $USERNAME`
        - on CentOS / RHEL `usermod -aG wheel $USERNAME`
    - confirm the user is added `id vivek`
    - add your ssh public key to `~/.ssh/authorized_keys`
    - disable root login and password based login `sudo vi /etc/ssh/sshd_config`
        - `ChallengeResponseAuthentication no`
        - `PasswordAuthentication no`
        - `UsePAM no`
        - `PermitRootLogin no`
    - restart ssh server `sudo systemctl reload ssh`
        - restart ssh on CentOS/RHEL `/etc/init.d/sshd reload`
 
 3. Run sudo commands without password [NixCarft Tutorial](https://www.cyberciti.biz/faq/linux-unix-running-sudo-command-without-a-password/)
    - add a file with configuration to `/etc/sudoers.d/` directory
    - `sudo vim /etc/sudoers.d/98-user-sudo-no-pass`
    - add to the file:
      ```
      # User rules for ubuntu -- first parameter is the username (ubuntu)
      ubuntu ALL=(ALL) NOPASSWD:ALL
      ```
    - sudo will read each file in /etc/sudoers.d, skipping file names that end in ‘~’ or contain a ‘.’ character to avoid causing problems with 
      package manager or editor temporary/backup files. Files are parsed in sorted lexical order. That is, /etc/sudoers.d/01_first will be parsed before /etc/sudoers.d/10_second. 
      Be aware that because the sorting is lexical, not numeric,

 4. Mounting USB drive
    - Create a folder to use for mount
        - `mkdir /mnt/usb`
    - Show Device
        - `lsblk`
    - What kind of device `sdb`
        - `sudo lshw`
    - Mount device
        - `sudo mount -t exfat /dev/sdb1 /media/usb`
 5. Install `.deb` file
    - `udo dpkg -i file.deb`
 
 6. Check Nvidia GPU usage
    - Query Info
        - `nvidia-smi --help-query-gpu`
    - Query Memory Used
        - `nvidia-smi --format=csv --query-gpu=memory.used`

## Compress using tar
    tar czvf name-of-archive.tar.gz /path/to/directory-or-file
    -c: Create an archive.
    -z: Compress the archive with gzip.
    -v: Display progress in the terminal while creating the archive, also known as “verbose” mode. The v is always optional in these commands, but it’s helpful.
    -f: Allows you to specify the filename of the archive.
