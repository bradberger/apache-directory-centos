Restrict access to the LDAP server with iptables.

To open the LDAP ports to any incoming request:

```bash
iptables -A INPUT -m state --state NEW -m tcp -p tcp --dport 10389 -j ACCEPT
iptables -A INPUT -m state --state NEW -m tcp -p tcp --dport 10636 -j ACCEPT
```

## Saving and Restoring Rules

Whatever the configuration, rules need to be saved. 

To save the rules:

```bash
sudo iptables-save > /etc/sysconfig/iptables
```

To restore the rules:

```bash
iptables-restore < /etc/sysconfig/iptables
```
