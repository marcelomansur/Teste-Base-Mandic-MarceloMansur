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


## Requisitos:

- Python
  
| Name | Version |
|------|---------|
| python3 | = 3.8.5 |
| pip3 | = 20.0.2 |
| ansible | = 2.10.5 |
| ansible-lint | = 4.3.7 |
| boto | = 2.49.0 |
| boto3 | = 1.17.49 |

- Módulos Ansible

| Name | Version |
|------|---------|
| nginxinc.nginx | = 0.19.1 |
| geerlingguy.docker | = 3.1.1 |
| geerlingguy.pip | = 2.0.0 |


## Como executar o playbook

Instale o Python 3

 - Instale o [Python 3](https://www.python.org/downloads/)
 - Instale o ansible usando venv:

```
python3 -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
```

Primeiro é necessário instalar as roles necessárias com o comando:

```
ansible-galaxy install -r requirements.yml
```

Também é necessário configurar as credenciais AWS para executar o playbook. Para fins de teste, essas variáveis ficarão armazenadas no arquivo `group_vars/local.yml`. Favor informar suas credenciais no campo:

```yml
# aws credentials
ec2_access_key: ""
ec2_secret_key: ""
```

> Referência: https://docs.ansible.com/ansible/latest/scenario_guides/guide_aws.html#authentication


Depois, execute o playbook usando o arquivo _hosts_ como inventário.

```
ansible-playbook -i hosts main.yml
```

## Como acessar as aplicações

Identificar o IP público da instância através do resumo da execução do Ansible:

```
PLAY RECAP *******************************************************************************************************
3.215.22.35 (este valor) ...
localhost                ...
```

Testar o acesso às URLs a seguir:
- http://<IP_PUBLICO>/site
- http://<IP_PUBLICO>/store
- http://<IP_PUBLICO>/blob
- http://<IP_PUBLICO>/tomcat

## TODO

Para fins de melhorias, existem tarefas a serem implementadas no projeto:
- [ ] Criar script para encriptar credenciais AWS, usando ansible-vault
- [ ] Melhorar idempotencia do playbook
- [ ] Criar pipeline CI com testes automatizados (molecule)
- [ ] Criar playbook para limpar o ambiente provisionado (ou usar Terraform para provisionamento)
