# Challenge my-ansible-provison-all-one
This project is the result of a challenge to create an entire environment using Ansible on AWS. **It's not using the best praticies of immutable infrastructure**, but it's a good opportunity to show the power of Ansible.

This module creates an environment with:

- Docker-compose environment with:
  - Magento store
  - Wordpress blog
  - An static page
  - Tomcat configuration
- Reverse proxy using NGINX and exposing these pages:
  - http://<ip-address>/store
  - http://<ip-address>/blog
  - http://<ip-address>/site
  - http://<ip-address>/tomcat


## Requirements:

- Python
  
| Name | Version |
|------|---------|
| python3 | = 3.8.5 |
| pip3 | = 20.0.2 |
| ansible | = 2.10.5 |
| ansible-lint | = 4.3.7 |
| boto | = 2.49.0 |
| boto3 | = 1.17.49 |

- Ansible modules

| Name | Version |
|------|---------|
| nginxinc.nginx | = 0.19.1 |
| geerlingguy.docker | = 3.1.1 |
| geerlingguy.pip | = 2.0.0 |


## How to run the playbook

Install Python 3

 - Install [Python 3](https://www.python.org/downloads/)
 - Install ansible using venv module:

```
python3 -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
```

Install Ansible roles using the command:

```
ansible-galaxy install -r requirements.yml
```

Also, configure AWS credentials to run the playbook. For this test, this information will be stored in variables on the file `group_vars/local.yml`. Please, inform your credentials on the fields:

```yml
# aws credentials
ec2_access_key: ""
ec2_secret_key: ""
```

> Ref: https://docs.ansible.com/ansible/latest/scenario_guides/guide_aws.html#authentication


Finally, run the playbook using _hosts_ file as inventory.

```
ansible-playbook -i hosts main.yml
```

## How to access applications

Identify the public IP of the AWS instance on Ansible execution resume:

```
PLAY RECAP *******************************************************************************************************
3.215.22.35 (this value) ...
localhost                ...
```

Access the following pages:
- http://<IP_PUBLICO>/site
- http://<IP_PUBLICO>/store
- http://<IP_PUBLICO>/blob
- http://<IP_PUBLICO>/tomcat

## TODO

Some improvements are needed to this project:
- [ ] Encrypt AWS credentials, using ansible-vault
- [ ] Improve the playbook idempotence
- [ ] Create a CI pipeline with molecule tests
- [ ] Create a playbook to clean the provision
