# Documentation

## See the full documentation on https://documentation-com.gitbook.io/dns-master-and-slave-installation/

# Prerequisites

## System Update

```bash
sudo apt update && sudo apt full-upgrade -y
```

## Bind9 Install

```bash
sudo apt install bind9 -y
```

## Ansible Installation

```bash
sudo apt install ansible -y
```

# Initial Configurations

## Hostname Configuration

### Master Configuration

1. Change the <mark style="color:red;">**name**</mark> to fit you best.

```bash
sudo nano /etc/hostname
ns1.projeto.com
```

### Slave Configuration

1. Change the <mark style="color:red;">**name**</mark> to fit you best.

```bash
sudo nano /etc/hostname
ns2.projeto.com
```

## Hosts Configuration

### Master Configuration

1. Change the <mark style="color:red;">`your_ip_address`</mark> to your <mark style="color:red;">IP</mark>.

```bash
sudo nano /etc/hosts
your_ip_address ns1.projeto.com ns1
```

### Slave Configuration

1. Change the <mark style="color:red;">`your_ip_address`</mark> to your <mark style="color:red;">IP</mark>

```bash
nano /etc/hosts
your_ip_address ns2.projeto.com ns2
```

# Initial Configurations

## Hostname Configuration

### Master Configuration

1. Change the <mark style="color:red;">**name**</mark> to fit you best.

```bash
sudo nano /etc/hostname
ns1.projeto.com
```

### Slave Configuration

1. Change the <mark style="color:red;">**name**</mark> to fit you best.

```bash
sudo nano /etc/hostname
ns2.projeto.com
```

## Hosts Configuration

### Master Configuration

1. Change the <mark style="color:red;">`your_ip_address`</mark> to your <mark style="color:red;">IP</mark>.

```bash
sudo nano /etc/hosts
your_ip_address ns1.projeto.com ns1
```

### Slave Configuration

1. Change the <mark style="color:red;">`your_ip_address`</mark> to your <mark style="color:red;">IP</mark>

```bash
nano /etc/hosts
your_ip_address ns2.projeto.com ns2
```

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
    masters { your_ip_address; 131};
};
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
