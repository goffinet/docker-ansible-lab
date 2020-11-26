# docker-ansible-lab

## Topologie

![](https://lucid.app/publicSegments/view/9175d49c-c39c-4446-b245-b9e6829dc57a/image.png)

## 1. Prérequis

Le seul prérequis est d'installer **Docker**, ici sous Linux[^1] :

```bash
curl -fsSL https://get.docker.com -o get-docker.sh && sh get-docker.sh
```

[^1]: Docker Desktop est aussi disponible pour Windows [Install Docker Desktop on Windows](https://docs.docker.com/docker-for-windows/install/) et pour Mac [Install Docker Desktop on Mac](https://docs.docker.com/docker-for-mac/install/).

Vous pouvez aussi passer par [http://play-with-docker.com](http://play-with-docker.com) (Cliquez sur "+ ADD NEW INSTANCE").

## 2. Se procurer le script

```bash
curl -s https://raw.githubusercontent.com/goffinet/docker-ansible-lab/master/startlab.sh > ./startlab.sh
chmod +x startlab.sh
```

## 3. Lancer le lab

```bash
./startlab.sh
```

Et vous êtes directement dans le contrôleur dans le dossier `/root/workspace` avec un inventaire prêt à l'emploi.

## 4. Nettoyage du lab

```bash
./startlab.sh --remove
```

## 5. Conteneurs

`startlab.sh` démarre quatre conteneurs docker et vous connecte à l'environnement du "controller".

**ansible.controller** est un conteneur Alpine Linux dans lequel ansible est disponible. On trouve le [Dockerfile](https://github.com/goffinet/docker-ansible-lab/blob/master/images/ansible-controller/Dockerfile) dans ce même repo. C'est lui qui gère les trois autres noeuds.

**node0**, **node1** et **node2** sont les conteneurs basés Centos 8 qui agissent comme des noeuds exploitables. Ces noeuds ont déjà été approvisionnés avec la clé ssh du conteneur **ansible.controller**. Ainsi, vous n'avez pas à vous occuper de l'installation des clés. Cette image est disponible sur [Registre d'images de Docker](https://hub.docker.com/repository/docker/goffinet/centos-8-ansible-docker-host) et le [Dockerfile](https://github.com/goffinet/docker-ansible-lab/blob/master/images/centos-8-ansible-docker-host/Dockerfile) est dans ce repo.

## 6. Port Mapping

Certains ports des conteneurs sont exposés en tant que ports "exposés" sur l'hôte :

| Conteneur | Port du conteneur| Port de l'hôte |
| --- | --- | --- |
| node0 | 80 | `$HOSTPORT_BASE` |
| node1 | 80 | `$HOSTPORT_BASE+1` |
| node2 | 80 | `$HOSTPORT_BASE+2` |
| node0 | 8080 | `$HOSTPORT_BASE+3` |
| node1 | 30000 | `$HOSTPORT_BASE+4` |
| node2 | 443 | `$HOSTPORT_BASE+5` |

La variable `HOSTPORT_BASE` est fixée à la valeur `42726` par défaut et peut être changée en démarrant le lab comme suit :

```bash
./startlab.sh --remove # Make sure you shut down the previous ones
HOSTPORT_BASE=<some_other_value> ./startlab.sh

```

## 7. Dossier de l'espace de travail Workspace

Un dossier `docker-ansible-lab/workspace` sur votre machine locale est monté en tant que `/root/workspace` dans le conteneur **ansible.controller**. Ainsi, vous pouvez utiliser votre éditeur favori sur votre machine locale pour éditer des fichiers.


## 8. Fabriquer les images Docker

Cloner le code source et se rendre dans le dossier images :

```bash
git clone https://github.com/goffinet/docker-ansible-lab.git
cd docker-ansible-lab/images
make buil_all
```
