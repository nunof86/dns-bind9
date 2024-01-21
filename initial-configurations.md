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

1. Change the <mark style="color:red;">**IP**</mark> to the <mark style="color:red;">**IP**</mark> of your machine.

```bash
sudo nano /etc/hosts
192.168.117.131 ns1.projeto.com ns1
```

### Slave Configuration

1. Change the <mark style="color:red;">**IP**</mark> to the <mark style="color:red;">**IP**</mark> of your machine.

```bash
nano /etc/hosts
192.168.117.132 ns2.projeto.com ns2
```
