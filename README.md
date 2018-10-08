# Installing Ansible Tower

## Step 1: Change Directory
```
cd /tmp
```

## Step 2: Download Red Hat Ansible Tower
Download the latest Ansible Tower package
```
curl -O https://releases.ansible.com/ansible-tower/setup/ansible-tower-setup-latest.tar.gz
```

## Step 3: Untar and unzip the package file
```
tar xvfz /tmp/ansible-tower-setup-latest.tar.gz
```

## Step 4: Edit Inventory file
```
cd /tmp/ansible-tower-setup-*/
vi inventory
```
### Identify and update variables
Fill a few variables out in an inventory file: admin_password, pg_password, rabbitmq_password
```
[tower]
localhost ansible_connection=local

[database]

[all:vars]
admin_password='ansibleWS'

pg_host=''
pg_port=''

pg_database='awx'
pg_username='awx'
pg_password='ansibleWS'

rabbitmq_port=5672
rabbitmq_vhost=tower
rabbitmq_username=tower
rabbitmq_password='ansibleWS'
rabbitmq_cookie=cookiemonster

# Needs to be true for fqdns and ip addresses
rabbitmq_use_long_name=false
```

## Step 5: Run setup
Run the Ansible Tower setup script
```
sudo ./setup.sh
```
