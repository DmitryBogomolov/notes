# SSH Setup

Generate keys; private key *id_rsa* is for client, public key *id_rsa.pub* is for server.
```bash
ssh-keygen -t rsa
```

## Server

Append public key to the *authorized_keys*.
```bash
cat /path/to/id_rsa.pub >> ~/.ssh/authorized_keys
chmod 600 ~/.ssh/authorized_keys
rm /path/to/id_rsa.pub
```

Backup */etc/ssh/sshd_config*.
```bash
sudo cp /etc/ssh/sshd_config /etc/ssh/sshd_config.ORIGINAL
```

Update */etc/ssh/sshd_config*.
```
Protocol 2
AllowUsers user1 user2
PermitRootLogin no
PasswordAuthentication no
LoginGraceTime 60
MaxStartups 2
PermitEmptyPasswords no
```

Restart service.
```bash
sudo service ssh restart
```

View access log.
```bash
cat /var/log/auth.log | grep sshd
```

## Client

Connect to server.
```bash
ssh -i /path/to/id_rsa <user>@<host>
```

Update *~/.ssh/config* file to use alias.
```
Host <alias>
  HostName <host>
  User <user>
  IdentityFile /path/to/id_rsa
```

Connect.
```bash
ssh <alias>
```

## Links

- [How to Secure an SSH Server in Ubuntu 14.04](https://www.maketecheasier.com/secure-ssh-server-ubuntu/)
- [How to add private key to SSH?](https://serverfault.com/questions/83516/how-to-add-private-key-to-ssh/)
- [How can I permanently add my SSH private key to Keychain so it is automatically available to ssh?](https://apple.stackexchange.com/questions/48502/how-can-i-permanently-add-my-ssh-private-key-to-keychain-so-it-is-automatically/)
