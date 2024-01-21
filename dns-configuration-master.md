# DNS Configuration - Master

## Named.conf.local Configuration

1. Modify the file and change the <mark style="color:red;">`zone`</mark> name to fit your case and the <mark style="color:red;">**IP**</mark> to the <mark style="color:red;">**IP**</mark> of your machine.

```bash
sudo nano /etc/bind/named.conf.local
```

```dns-zone-file
zone "projeto.com" {
    type master;
    file "/etc/bind/zones/projeto.com.zone";
    allow-transfer { your_ip_address; };
};
```

## Zone Configuration

1. Create a <mark style="color:red;">`zones`</mark> directory.
2. Create a file with the name of the <mark style="color:red;">`zone`</mark> previously created and with extension <mark style="color:red;">`.zone`</mark>.

```bash
sudo mkdir /etc/bind/zones
sudo nano /etc/bind/zones/projeto.com.zone
```

3. Point the <mark style="color:red;">**IP**</mark> addresses of the <mark style="color:red;">**Master(**</mark><mark style="color:red;">`ns1`</mark><mark style="color:red;">**)**</mark> and <mark style="color:red;">**Slave(**</mark><mark style="color:red;">`ns2`</mark><mark style="color:red;">**)**</mark> machines in the <mark style="color:red;">**A record**</mark>.

```dns-zone-file
$TTL 86400
@       IN      SOA     ns1.projeto.com. admin.projeto.com. (
                            2023061401 ; Serial
                            3600       ; Refresh
                            1800       ; Retry
                            604800     ; Expire
                            86400      ; Minimum TTL
                    )
@       IN      NS      ns1.projeto.com.
@       IN      NS      ns2.projeto.com.
ns1     IN      A       ns1_ip_address
ns2     IN      A       ns2_ip_address
```

## Resolv.conf Configuration

1. Modify the <mark style="color:red;">`/etc/resolv.conf`</mark> configuration file to match the <mark style="color:red;">**IP**</mark> address of the <mark style="color:red;">**DNS Master**</mark> machine.

```bash
nano /etc/resolv.conf
nameserver your_ip_address
```

## Restart the BIND9 Service

```bash
systemctl restart bind9
```
