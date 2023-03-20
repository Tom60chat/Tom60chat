# 16 - SSH

1. `echo "Authorized uses only. All activity may be monitored and reported." > /etc/issue`
   `cat /etc/issue > /etc/issue.net`

2. `groupadd sshusers`
   `groupadd sftpusers`

3. `usermod -a -G sshusers root`

4. `useradd -c "httpw1" -d /var/www/vhosts/w1.XXX.lprgi.u13.org -s /usr/sbin/nologin  -g sftpusers httpw1`
   `useradd -c "httpw2" -d /var/www/vhosts/w2.XXX.lprgi.u13.org -s /usr/sbin/nologin  -g sftpusers httpw2`

5. `nano /etc/ssh/sshd_config`
   ```apache
   AllowGroups sshusers sftpusers
   ```

6. `nano /etc/ssh/sshd_config`
   ```
   Banner /etc/issue.net
   ```

7. `passwd httpw1`
   `passwd httpw2`

8. `nano /etc/ssh/sshd_config`
   ```
    Match Group sftpusers
    ChrootDirectory %h
    ForceCommand internal-sftp
    X11Forwarding no
    AllowTcpForwarding no
    GatewayPorts no 
   ```

9. `sudo service sshd restart`	
   FileZilla time

10. `ssh-keygen -t ed25519 -f ~/.ssh/lprgi`

11. `ssh-copy-id -i .ssh/lprgi.pub x12345678@etud.dptinfo.iut-amiens.fr`

12. `/bin/nano etud-w.sh`
    ```bash
    # !bin/bash

    ssh -i .ssh/lprgi x12345678@etud.dptinfo.iut-amiens.fr w
    ```