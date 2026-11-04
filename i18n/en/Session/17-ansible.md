# 17. Ansible

[한국어](../../../Session/17-ansible.md) | **English** | [日本語](../../ja/Session/17-ansible.md) | [简体中文](../../zh-CN/Session/17-ansible.md) | [Español](../../es/Session/17-ansible.md) | [Français](../../fr/Session/17-ansible.md)

### [ Host PC ] → [ MGMT ]

Send `jjong-idc-lab_Ansible.zip`, built with `scripts/make-ansible-zip.sh`, to MGMT (using the NAT adapter).

```powershell
scp .\jjong-idc-lab_Ansible.zip root@<MGMT_NAT_IP>:/root/
```

### [ MGMT ]

```bash
apt install -y unzip ansible
unzip -o /root/jjong-idc-lab_Ansible.zip -d /etc/
ssh-keyscan 10.10.30.254 10.10.10.10 10.10.10.21 10.10.10.22 10.10.20.10 >> /root/.ssh/known_hosts
ssh-keygen -t ed25519 -N "" -f /root/.ssh/id_ed25519
for h in 10.10.30.254 10.10.10.10 10.10.10.21 10.10.10.22 10.10.20.10; do ssh-copy-id root@$h; done
cd /etc/ansible
ansible all -m ping
ansible-playbook site.yml --check
ansible-playbook site.yml
```
