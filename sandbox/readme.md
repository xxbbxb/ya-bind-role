
### How to use role

Human friendly variable format See `group_vars/bind.yml`

```
bind_zones:
  - name: example.com
    records: 
      - 'www.example.com. IN A 184.168.221.54'
      - 'ttt.example.com. IN A 184.168.221.54'
      - 'zzz.example.com. IN A 184.168.221.54'
```

master/slave configured using ansible groups, inventory file:

```

[bind:children]
bind-master
bind-slaves

[bind-master]
ns1 ansible_ssh_host=172.17.0.3

[bind-slaves]
ns2 ansible_ssh_host=172.17.0.2
```

```bash
sudo make sandbox
# see Makefile for more details....
sudo ansible-playbook bind.yml #....
➜  sandbox git:(master) ✗ dig @172.17.0.3 zzz.example.com +short
184.168.221.54
```