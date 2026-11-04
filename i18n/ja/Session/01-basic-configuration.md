# 01. Basic Configuration

[한국어](../../../Session/01-basic-configuration.md) | [English](../../en/Session/01-basic-configuration.md) | **日本語** | [简体中文](../../zh-CN/Session/01-basic-configuration.md) | [Español](../../es/Session/01-basic-configuration.md) | [Français](../../fr/Session/01-basic-configuration.md)

### [ All Devices ]

```bash
apt install -y vim ssh curl nmap rsyslog
echo -e 'set nu\nset ic\nset si' >> /etc/vim/vimrc
echo 'permitrootlogin yes' >> /etc/ssh/sshd_config && systemctl restart ssh
read hostname && echo $hostname > /etc/hostname && hostname $hostname && exit
```

`read hostname` で FW、DNS、WEB1、WEB2、DB、MGMT、CLIENT、REMOTE のいずれかを入力します。

> `/etc/hosts` の `127.0.1.1` の行も新しいホスト名に変更します。そのままにすると、apache2 などを再起動するときに名前が見つからないという警告が出ます。
