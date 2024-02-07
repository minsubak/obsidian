# 0 initalize
```bash
vim /etc/ssh/sshd_config
# edit sshd_config

systemctl restart sshd
# restart sshd service
```

sshd_config
```
...
AllowTcpForwarding yes
GatewayPorts yes
...
```

# 1 local port forwarding
```bash
ssh -fN -L [port1]:[client address]:[port2] [username]@[server address]
# ssh tunneling:: local port forwarding work

ssh root@[server address]
# connect to the server with ssh tunneling
```

이하 생략
2 remote port forwarding
3 dynamic port forwarding

