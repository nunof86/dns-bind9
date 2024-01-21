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
