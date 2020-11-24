# docker-ansible-lab

## Prérequis

Le seul prérequis est **docker**, sous Linux :

```bash
curl -fsSL https://get.docker.com -o get-docker.sh && sh get-docker.sh
```

Vous pouvez aussi passer par [http://play-with-docker.com](http://play-with-docker.com) (Cliquez sur "+ ADD NEW INSTANCE" et choisissez ce repo).

Docker Desktop est aussi disponible pour Windows [Install Docker Desktop on Windows](https://docs.docker.com/docker-for-windows/install/) et pour Mac [Install Docker Desktop on Mac](https://docs.docker.com/docker-for-mac/install/).

## Se procurer le script

```
curl -s https://raw.githubusercontent.com/goffinet/docker-ansible-lab/master/startlab.sh > ./startlab.sh
chmod +x startlab.sh
```

## Comment lancer le lab


```bash
./startlab.sh
```

## Nettoyage

```bash
./startlab.sh --remove
```

## Conteneurs

`startlab.sh` démarre quatre conteneurs docker et vous connecte à l'environnement du "controller".

**ansible.controller** est un conteneur Alpine Linux dans lequel ansible est disponible. On trouve le [Dockerfile](https://github.com/goffinet/docker-ansible-lab/blob/master/images/ansible-controller/Dockerfile) dans ce même repo. C'est lui qui gère les trois autres noeuds.

**host0.example.org**, **host1.example.org** et **host2.example.org** sont les conteneurs basés Centos 8 qui agissent comme des noeuds exploitables. Ces noeuds ont déjà été approvisionnés avec la clé ssh du conteneur **ansible.controller**. Ainsi, vous n'avez pas à vous occuper de l'installation des clés. Cette image est disponible sur [https://hub.docker.com/repository/docker/goffinet/centos-8-ansible-docker-host](https://hub.docker.com/repository/docker/goffinet/centos-8-ansible-docker-host) et le [Dockerfile](https://github.com/goffinet/docker-ansible-lab/blob/master/images/centos-8-ansible-docker-host/Dockerfile) est dans ce repo.

## Port Mapping

Certains ports des conteneurs sont exposés comme ports hôtes comme suit :

Conteneur|Port du conteneur|Port de l'hôte
:---|:---:|:---:
host0.example.org|80|`$HOSTPORT_BASE`  
host1.example.org|80|`$HOSTPORT_BASE+1`
host2.example.org|80|`$HOSTPORT_BASE+2`
host0.example.org|8080|`$HOSTPORT_BASE+3`
host1.example.org|30000|`$HOSTPORT_BASE+4`
host2.example.org|443|`$HOSTPORT_BASE+5`

La variable `HOSTPORT_BASE` est fixée à la valeur `42726` par défaut et peut être changée en démarrant le lab comme suit :

```bash
./startlab.sh --remove # Make sure you shut down the previous ones
HOSTPORT_BASE=<some_other_value> ./startlab.sh
```

## Dossier de l'espace de travail Workspace

Un dossier `docker-ansible-lab/workspace` sur votre machine locale est monté en tant que `/root/workspace` dans le conteneur **ansible.controller**. Ainsi, vous pouvez utiliser votre éditeur favori sur votre machine locale pour éditer des fichiers. L'édition de fichiers n'est cependant pas nécessaire pour suivre utiliser le lab.
