# SFTP Connection Setups
Ansible playbook for SFTP connection setups.

## With this playbook, 
- Create users for SFTP connection (who are not allowed to shell login)
- Create group `sftp` for SFTP users 
- Users in `sftp` group are only allowed to SFTP connect
- `/etc/ssh/sshd_config` will be changed as below

before
```
Subsystem sftp	/usr/libexec/openssh/sftp-server
```
after
```
Subsystem sftp internal-sftp

# BEGIN SFTP-Server sftp block
Match Group sftp
    ChrootDirectory /
    AllowTCPForwarding no
    X11Forwarding no
    ForceCommand internal-sftp
    AuthorizedKeysFile /home/%u/.ssh/authorized_keys
# END SFTP-Server sftp block
```

## Preparation
Change `inventory` and `group_vars/sftp_server.yml` as you like

## Command
```
ansible-playbook playbook_sftp.yml
```
