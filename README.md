# ansible-role-etcd

It helps to install etcd cluster and create certificates.

## Install

```shell
ansible-galaxy role install git+https://github.com/sergelogvinov/ansible-role-etcd.git,main
```

## Usage

```ini
# etcd.ini

[etcd]
etc-1       ansible_host=192.168.1.1
etc-2       ansible_host=192.168.1.2

[etcd-cluster]
etc-1
etc-2
```

```yaml
# etcd.yml

# Create root CA and other certs automatically
#
- hosts: non-ca
  roles:
    - ansible-role-etcd

# Pass the root certificate and create other certs
#
- hosts: ca
  vars:
    etcd_ca_client_crt_data: |
      -----BEGIN CERTIFICATE-----
      -----END CERTIFICATE-----
    etcd_ca_client_key_data: |
      -----BEGIN EC PRIVATE KEY-----
      -----END EC PRIVATE KEY-----
    etcd_ca_peer_crt_data: "{{ etcd_ca_client_crt_data }}"
    etcd_ca_peer_key_data: "{{ etcd_ca_client_key_data }}"
  roles:
    - ansible-role-etcd
```

```shell
ansible-playbook -Dv -i etcd.ini etcd.yml
```
