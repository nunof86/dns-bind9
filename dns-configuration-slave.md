# DNS Configuration - Slave

## Named.conf.local Configuration

1. In the <mark style="color:red;">**Slave**</mark> configuration you only need to modify the <mark style="color:red;">`/etc/bind/named.conf.local`</mark> and specify the <mark style="color:red;">`zone`</mark> previously created, the <mark style="color:red;">`type`</mark> of the DNS, in that case <mark style="color:red;">**slave**</mark>, and the <mark style="color:red;">**IP address**</mark> of the <mark style="color:red;">**masters DNS**</mark>.

```bash
sudo nano /etc/bind/named.conf.local
```

```dns-zone-file
zone "projeto.com" {
    type slave;
    file "/var/cache/bind/projeto.com.zone";
    masters { 192.168.117.154; 131};
};
```

## Resolv.conf Configuration

1. Modify the <mark style="color:red;">`/etc/resolv.conf`</mark> configuration file to match the <mark style="color:red;">**IP**</mark> address of the <mark style="color:red;">**DNS Master**</mark> machine.

```bash
nano /etc/resolv.conf
nameserver 192.168.117.131
```

## Restart the BIND9 Service

```bash
systemctl restart bind9
```
