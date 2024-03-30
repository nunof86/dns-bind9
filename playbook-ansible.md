# Playbook - Ansible

## Ansible Playbook

### DNS Master

1. The script bellow is the configuration for the DNS Master machine.
2. Change the content of the file <mark style="color:red;">`/etc/hostname`</mark> and <mark style="color:red;">`/etc/hosts`</mark> to fit you case, as well the IP address and zones files in the <mark style="color:red;">`/etc/bind/named.conf.local`</mark> and <mark style="color:red;">`/etc/bind/zones/projeto.com.zone`</mark>. To finish the Master configuration, change the <mark style="color:red;">`/etc/resolv.conf`</mark> file to match the IP address of the DNS Master machine(should be the same as the IP of the <mark style="color:red;">`/etc/hosts`</mark> and <mark style="color:red;">`/etc/bind/named.conf.local`</mark> and the first A record in the <mark style="color:red;">`/etc/bind/zones/projeto.com.zone`</mark>. The second A record should match the IP of the Slave machine).
3. Replace the <mark style="color:red;">`hosts:`</mark> option to <mark style="color:red;">**localhost**</mark> or other <mark style="color:red;">**host**</mark> of your choice.

```yaml
---

- name: DNS Master Configuration
  hosts: dns_master

  tasks:
    - name: System Update
      apt:
        update_cache: yes
      changed_when: false

    - name: Dist Upgrade
      apt:
        upgrade: dist
      register: upgrade_output

    - name: Replace the /etc/hostname Configuration File
      become: yes
      copy:
        content: 'ns1.projeto.com'
        dest: /etc/hostname
      diff: yes
      ignore_errors: yes

    - name: Replace the /etc/hosts Configuration File
      become: yes
      copy:
        content: 'your_ip_address ns1.projeto.com ns1'
        dest: /etc/hosts
      diff: yes
      ignore_errors: yes

    - name: Install BIND9 Package
      apt:
        name: bind9
        state: present

    - name: Configuration of the named.conf.local Configuration File
      copy:
        content: |
          zone "projeto.com" {
            type master;
            file "/etc/bind/zones/projeto.com.zone";
            allow-transfer { your_ip_address; };
          };
        dest: /etc/bind/named.conf.local

    - name: Create the zones Directory
      file:
        path: /etc/bind/zones
        state: directory

    - name: Create the projeto.com.zone Configuration File
      copy:
        content: |
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
        dest: /etc/bind/zones/projeto.com.zone

    - name: Replace the /etc/resolv.conf Configuration File
      become: yes
      copy:
        content: 'nameserver your_ip_address'
        dest: /etc/resolv.conf
      diff: yes
      ignore_errors: yes

    - name: Restart the BIND9 Service
      systemd:
        name: bind9
        state: restarted
```

### DNS Slave

1. The script bellow is the configuration for the DNS Slave machine.
2. Change the content of the file <mark style="color:red;">`/etc/hostname`</mark> and <mark style="color:red;">`/etc/hosts`</mark> to fit you case, as well the IP address of the file <mark style="color:red;">`/etc/bind/named.conf.local`</mark>(the IP should be the Masters IP). To finish the Slave configuration, change the <mark style="color:red;">`/etc/resolv.conf`</mark> file to match the IP address of the DNS Master machine.
3. Replace the <mark style="color:red;">`hosts:`</mark> option to <mark style="color:red;">**localhost**</mark> or other <mark style="color:red;">**host**</mark> of your choice.

```yaml
---

- name: DNS Slave Configuration
  hosts: dns_slave

  tasks:
    - name: Replace the /etc/hostname Configuration File
      become: yes
      copy:
        content: 'ns2.projeto.com'
        dest: /etc/hostname
      diff: yes
      ignore_errors: yes

    - name: Replace the /etc/hosts Configuration File
      become: yes
      copy:
        content: 'your_ip_address ns1.projeto.com ns2'
        dest: /etc/hosts
      diff: yes
      ignore_errors: yes

    - name: Install BIND9 Package
      apt:
        name: bind9
        state: present

    - name: Configuration of the named.conf.local Configuration File
      copy:
        content: |
          zone "projeto.com" {
              type slave;
              file "/var/cache/bind/projeto.com.zone";
              masters { your_ip_address; 131};
          };
        dest: /etc/bind/named.conf.local

    - name: Restart the BIND9 Service
      systemd:
        name: bind9
        state: restarted
```
