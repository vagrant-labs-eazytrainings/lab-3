# Lab 3 : Création et Gestion d'une VM CentOS avec Nginx

## 🎯 Objectif

L'objectif de ce laboratoire est de :

- Créer une machine virtuelle **CentOS** avec **Vagrant**.
- Utiliser des variables pour le **Vagrantfile** afin de rendre la configuration flexible.
- Allouer des ressources (RAM et CPU) de manière classique.
- Installer un serveur **Nginx** manuellement.
- Résoudre les difficultés liées aux dépôts CentOS obsolètes depuis 2004.

---

## 🧮 Prérequis

Assure-toi d'avoir les outils suivants installés :

- **Vagrant**
- **VirtualBox**
- Connexion Internet fonctionnelle

---

## 🚀 Étapes du Lab

### 1. Initialisation de la VM avec CentOS 7

Créez une machine virtuelle basée sur CentOS 7 :

```bash
vagrant init centos/7
```

### 2. Configuration du Vagrantfile avec des variables

Voici le **Vagrantfile** utilisé pour ce lab :

```ruby
# Variables pour la configuration
RAM_MACHINE = "2048"
CPU_MACHINE = 2
OS_BOX = "centos/7"
VERSION_BOX = "2004.01"

# Configuration de la machine virtuelle
Vagrant.configure("2") do |config|
  config.vm.box = OS_BOX
  config.vm.box_version = VERSION_BOX

  # Configuration des ressources
  config.vm.provider "virtualbox" do |vb|
    vb.memory = RAM_MACHINE
    vb.cpus = CPU_MACHINE
  end

  # Configuration réseau
  config.vm.network "private_network", type: "dhcp"
end
```

### 3. Déploiement et connexion à la VM

1. **Démarrez la VM** :

   ```bash
   vagrant up
   ```

2. **Connectez-vous à la VM** :

   ```bash
   vagrant ssh
   ```

### 4. Installation manuelle de Nginx

1. **Modifier les dépôts pour résoudre les problèmes obsolètes** :

   ```bash
   vi /etc/yum.repos.d/CentOS-Base.repo
   ```
   remplace contenu
   
   ```bash
   [base]
    name=CentOS-$releasever - Base
    baseurl=http://vault.centos.org/7.9.2009/os/$basearch/
    gpgcheck=1
    gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-7
    
    [updates]
    name=CentOS-$releasever - Updates
    baseurl=http://vault.centos.org/7.9.2009/updates/$basearch/
    gpgcheck=1
    gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-7
    
    [extras]
    name=CentOS-$releasever - Extras
    baseurl=http://vault.centos.org/7.9.2009/extras/$basearch/
    gpgcheck=1
    gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-7
    
    [centosplus]
    name=CentOS-$releasever - Plus
    baseurl=http://vault.centos.org/7.9.2009/centosplus/$basearch/
    gpgcheck=1
    enabled=0
    gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-7
    ```

2. **Mettre à jour les paquets et installer Nginx** :

   ```bash
   sudo yum clean all && sudo yum update -y
   sudo yum install -y epel-release
   sudo yum install -y nginx
   ```

3. **Démarrer et activer Nginx** :

   ```bash
   sudo systemctl start nginx
   sudo systemctl enable nginx
   ```

### 5. Tester le serveur Nginx

1. **Vérifiez l'adresse IP** de la VM :

   ```bash
   ip a
   ```

2. Depuis votre navigateur sur l'hôte, accédez à :

   ```
   http://<IP_DE_LA_VM>
   ```

3. Vous devriez voir la page par défaut de **Centos**.

---

## 🛠️ Difficultés Rencontrées

### Problème des dépôts CentOS obsolètes

Depuis **2004**, les dépôts **mirrorlist.centos.org** ne fonctionnent plus pour CentOS 7.

- **Solution** : Remplacement des URLs **mirrorlist** par des **baseurl** pointant vers **vault.centos.org**. Merci à [ServerFault](https://serverfault.com/questions/904304/could-not-resolve-host-mirrorlist-centos-org-centos-7) pour l'astuce.

---

## 🔍 Résumé des Commandes Utiles

| Commande                               | Description                          |
|----------------------------------------|--------------------------------------|
| `vagrant init centos/7`                | Initialise le projet avec CentOS 7.  |
| `vagrant up`                           | Démarre la machine virtuelle.        |
| `vagrant ssh`                          | Accède à la VM via SSH.             |
| `sudo yum install -y nginx`            | Installe le serveur Nginx.           |
| `sudo systemctl start nginx`           | Démarre le service Nginx.            |
| `ip a`                                 | Affiche l'adresse IP de la VM.       |

---

## 📊 Conclusion

Ce lab a permis de :

- Configurer une machine virtuelle CentOS avec des ressources définies.
- Installer et configurer **Nginx** manuellement.
- Surmonter les problèmes liés aux dépôts obsolètes de CentOS.

🚀 **Félicitations ! Votre serveur Nginx fonctionne sur CentOS.** 🚀

---

## 🌟 Crédits

Merci à **StackOverflow** et **ServerFault** pour les solutions apportées ! 😊
