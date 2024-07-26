# Installation et Configuration des Outils Utilisés DevOps


## Table des Matières
- [Installation de Docker](#installation-de-docker)
  - [Mise à jour des paquets](#mise-à-jour-des-paquets)
  - [Installation des dépendances](#installation-des-dépendances)
  - [Ajout de la clé GPG Docker](#ajout-de-la-clé-gpg-docker)
  - [Ajout du dépôt Docker](#ajout-du-dépôt-docker)
  - [Installation de Docker](#installation-de-docker-1)
  - [Configuration des permissions Docker](#configuration-des-permissions-docker)
  - [Permissions supplémentaires](#permissions-supplémentaires)
- [Installation de JDK 17](#installation-de-jdk-17)
- [Installation de Jenkins](#installation-de-jenkins)
  - [Ajout de la clé Jenkins](#ajout-de-la-clé-jenkins)
  - [Ajout du dépôt Jenkins](#ajout-du-dépôt-jenkins)
  - [Installation de Jenkins](#installation-de-jenkins-1)
- [Installation de Trivy](#installation-de-trivy)
- [Installation de SonarQube](#installation-de-sonarqube)
- [Identifiants](#identifiants)
  - [Jenkins](#jenkins)
  - [SonarQube](#sonarqube)
  - [Root (machine Jenkins)](#root-machine-jenkins)
- [Liens Utiles](#liens-utiles)
  - [Lier Jenkins avec SonarQube](#lier-jenkins-avec-sonarqube)
  - [Snyk](#snyk)
- [Jenkinsfile Exemple](#jenkinsfile-exemple)
- [Installation et Configuration d'Ansible](#installation-et-configuration-dansible)
  - [Installation d'Ansible](#installation-dansible)
  - [Configurer sudo sans mot de passe pour Jenkins](#configurer-sudo-sans-mot-de-passe-pour-jenkins)
  - [Utiliser Ansible avec Jenkins](#utiliser-ansible-avec-jenkins)
  - [Problèmes avec Ansible](#problèmes-avec-ansible)

## Installation de Docker

Sachez qu'il faut avoir préalablement Vagrant et VMware. Ci-dessous les commandes à exécuter dans le terminal :

```bash
vagrant plugin install vagrant-vmware-desktop
vagrant plugin install vagrant-vmware-utility
## Mise à jour des paquets
sudo apt update
sudo apt upgrade -y
## Installation des dépendances
sudo apt install -y ca-certificates curl gnupg lsb-release
## Ajout de la clé GPG Docker
sudo mkdir -p /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
## Ajout du dépôt Docker
sudo sh -c 'echo "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" > /etc/apt/sources.list.d/docker.list'
sudo apt update
## Installation de Docker
sudo apt install -y docker-ce docker-ce-cli containerd.io docker-compose-plugin
## Configuration des permissions Docker
sudo groupadd docker
sudo usermod -aG docker $USER
sudo curl -L "https://github.com/docker/compose/releases/latest/download/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
sudo chmod +x /usr/local/bin/docker-compose
## Permissions supplémentaires
sudo groupadd docker
sudo usermod -aG docker ${USER}
sudo chmod 666 /var/run/docker.sock
sudo systemctl restart docker
## Ajout du dépôt Jenkins
echo "deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc] https://pkg.jenkins.io/debian binary/" | sudo tee /etc/apt/sources.list.d/jenkins.list > /dev/null
## Installation de Jenkins
sudo apt-get update
sudo apt-get install fontconfig openjdk-17-jre
sudo apt-get install jenkins
## Installation de Trivy
sudo apt-get install wget apt-transport-https gnupg lsb-release
wget -qO - https://aquasecurity.github.io/trivy-repo/deb/public.key | gpg --dearmor | sudo tee /usr/share/keyrings/trivy.gpg > /dev/null
echo "deb [signed-by=/usr/share/keyrings/trivy.gpg] https://aquasecurity.github.io/trivy-repo/deb $(lsb_release -sc) main" | sudo tee -a /etc/apt/sources.list.d/trivy.list
sudo apt-get update
sudo apt-get install trivy
## Installation de SonarQube
# Consulter la documentation : https://gist.github.com/dmancloud/0abf6ad0cb16e1bce2e907f457c8fce9
## Lier Jenkins avec SonarQube
https://sunilhari.medium.com/how-to-integrate-sonarqube-and-jenkins-721d5efd3cb6
```bash


