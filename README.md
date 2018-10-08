## Installing Ansible Tower

#### Step 1: Change Directory
```
cd /tmp
```

#### Step 2: Download Red Hat Ansible Tower
Download the latest Ansible Tower package
```
curl -O https://releases.ansible.com/ansible-tower/setup/ansible-tower-setup-latest.tar.gz
```

#### Step 3: Untar and unzip the package file
```
tar xvfz /tmp/ansible-tower-setup-latest.tar.gz
```

#### Step 4: Edit Inventory file
```
cd /tmp/ansible-tower-setup-*/
vi inventory
```
##### Identify and update variables
Fill a few variables out in an inventory file: **admin_password**, **pg_password**, **rabbitmq_password**
*Example:*
```
[tower]
localhost ansible_connection=local

[database]

[all:vars]
admin_password='<insert password>'

pg_host=''
pg_port=''

pg_database='awx'
pg_username='awx'
pg_password='<insert password>'

rabbitmq_port=5672
rabbitmq_vhost=tower
rabbitmq_username=tower
rabbitmq_password='<insert password>'
rabbitmq_cookie=cookiemonster

# Needs to be true for fqdns and ip addresses
rabbitmq_use_long_name=false
```

#### Step 5: Run setup
Run the Ansible Tower setup script
```
sudo ./setup.sh
```
#### Step 6: Log into the Ansible Tower interface and apply license

## Stage Demo project

#### Step 1: Change Directory
```
cd /tmp
git clone https://github.com/ajacocks/dp.git
cd /tmp/dp
```
#### Step 2: Create vaulted variables
```
ansible-vault encrypt_sting <ansible tower password>
New Vault password: <set vault password>
Confirm New Vault password: <set vault password>

ansible-vault encrypt_sting <aws key>
New Vault password: <set vault password - same as above>
Confirm New Vault password: <confirm vault password>

ansible-vault encrypt_sting <aws secret>
New Vault password: <set vault password - same as above>
Confirm New Vault password: <confirm vault password>

ansible-vault encrypt_sting <default workshop password>
New Vault password: <set vault password - same as above>
Confirm New Vault password: <confirm vault password>
```

#### Step 3: Edit configure Ansible Tower playbook
```
vi configure_tower.yml
```

##### Identify and update variables
Fill a few variables out in an inventory file: **AWS_KEY**, **AWS_SECRET**, **tower_pass**, **workshop_password**, **org**, **domain_name**
*Example:*
```
---
- hosts: localhost
  name: Configure Ansible Tower
  connection: local
  vars:
    AWS_KEY: !vault |
          $ANSIBLE_VAULT;1.1;AES256
          00000000000000000000000000000000000000000000000000000000000000000000000000000000
          00000000000000000000000000000000000000000000000000000000000000000000000000000000
          00000000000000000000000000000000000000000000000000000000000000000000000000000000
          00000000000000000000000000000000000000000000000000000000000000000000000000000000
          00000000000000000000000000000000000000000000000000000000000000000000000000000000
          0000000000
    AWS_SECRET: !vault |
          $ANSIBLE_VAULT;1.1;AES256
          00000000000000000000000000000000000000000000000000000000000000000000000000000000
          00000000000000000000000000000000000000000000000000000000000000000000000000000000
          00000000000000000000000000000000000000000000000000000000000000000000000000000000
          00000000000000000000000000000000000000000000000000000000000000000000000000000000
          00000000000000000000000000000000000000000000000000000000000000000000000000000000
          0000000000
    tower_host: localhost
    tower_user: admin
    tower_pass: !vault |
          $ANSIBLE_VAULT;1.1;AES256
          00000000000000000000000000000000000000000000000000000000000000000000000000000000
          00000000000000000000000000000000000000000000000000000000000000000000000000000000
          00000000000000000000000000000000000000000000000000000000000000000000000000000000
          00000000000000000000000000000000000000000000000000000000000000000000000000000000
          00000000000000000000000000000000000000000000000000000000000000000000000000000000
          0000000000
    workshop_password: !vault |
          $ANSIBLE_VAULT;1.1;AES256
          00000000000000000000000000000000000000000000000000000000000000000000000000000000
          00000000000000000000000000000000000000000000000000000000000000000000000000000000
          00000000000000000000000000000000000000000000000000000000000000000000000000000000
          00000000000000000000000000000000000000000000000000000000000000000000000000000000
          00000000000000000000000000000000000000000000000000000000000000000000000000000000
          0000000000
    org: "DreamPort"
    domain_name: redhatgov.io
```

#### Step 4: Run playbook
```
ansible-playbook configure_tower.yml
```

