# 01. Basic Configuration

[한국어](../../../Session/01-basic-configuration.md) | [English](../../en/Session/01-basic-configuration.md) | [日本語](../../ja/Session/01-basic-configuration.md) | **简体中文** | [Español](../../es/Session/01-basic-configuration.md) | [Français](../../fr/Session/01-basic-configuration.md)

### [ All Devices ]

```bash
apt install -y vim ssh curl nmap rsyslog
echo -e 'set nu\nset ic\nset si' >> /etc/vim/vimrc
echo 'permitrootlogin yes' >> /etc/ssh/sshd_config && systemctl restart ssh
read hostname && echo $hostname > /etc/hostname && hostname $hostname && exit
```

在 `read hostname` 处输入 FW、DNS、WEB1、WEB2、DB、MGMT、CLIENT、REMOTE 之一。

> 同时将 `/etc/hosts` 中 `127.0.1.1` 那一行改为新的主机名。如果保持不变，重启 apache2 等服务时会出现无法解析主机名的警告。
