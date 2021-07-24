# Integrate Keycloak with HashiCorp Vault

This is an example Terraform implementation of a Keycloak Vault integration.<br/> 
The project refers to a [medium post on this topic](https://pascal-euhus.medium.com/integrate-keycloak-with-hashicorp-vault-5264a873dd2f). 

:warning: <br/>  
Note for Mac user :apple: to work with the Terraform code you need to add to your ```/etc/hosts```; <br/>
```
127.0.0.1 keycloak
``` 
:warning: <br/>  

## Usage
Use makefile: <br/>
```make [help | up | down | init | provision | deprovision | destroy]```

1. Start the local environment (Docker) ````make up````
2. Initialize Terraform  ````make init````
3. Apply the Terraform configuration ````make provision````
4. Shutdown the local environment (Docker) ````make down````

### Makefile 
| Command        | Description           |
| ------------- |:-------------:|
| up      | start docker container |
| down      | stop docker container      |
| init | terraform init    |
| provision | terraform apply     |
| deprovision | terraform destroy     |
| destroy |  terraform destroy and remove all terraform related files/states   |


# troubleshoot before run
## must install docker, docker compose, terraform before
My version:
OS: Centos7
Docker: Docker version 20.10.7, build f0df350
Docker Compose: docker-compose version 1.25.4, build 8d51620a
Terraform: Terraform v1.0.3

## uninstall the keycloak by make up and recreate it with docker run
$ docker run -d -p 8080:8080 --name=keycloak-vault_keycloak_1 jboss/keycloak

## create first user and disable ssl keycloak 
############ Create first user admin ########################
docker exec -it {contaierID} /bin/bash
/opt/jboss/keycloak/bin/add-user-keycloak.sh -u root -p root

############ stop SSL when access console ###################
docker exec -it {contaierID} /bin/bash
cd keycloak/bin
/opt/jboss/keycloak/bin/kcadm.sh config credentials --server http://localhost:8080/auth --realm master --user admin
/opt/jboss/keycloak/bin/kcadm.sh update realms/master -s sslRequired=NONE
