
#  TP3

## EXERCICE 1

### Q1:
Mettre a jour le système : 
```
sudo apt full-upgrade
```

### Q2:
Compter le nombre de packages installés :
```
sudo apt list -i | wc -l
sudo dpkg -l | wc -l
```
le comptage avec apt nous donne 549 résultats et celui avec dpkg 553. Cette différence s'explique par le fait que certains packages ont été installés avec la commande dpkg c'est à dire qu'ils ne gèrent pas les dépendances.

### Q3:
La commande suivante permet de compter le nombre de packages disponibles au téléchargement :
```
sudo apt list | wc -l
```

### Q4 :

Création d'un alias de mise a jour

Il faut d'abord ajouter la ligne dans le fichier /.bash_aliases
```
alias maj='sudo apt update && sudo apt upgrade'
```
Puis pour prendre en compte ce changement taper la commande
```
source ~./bash_aliases
```

### Q5:

Installation de fortunes
```
sudo apt install fortunes
```


### Q6:

Installation du jeu sudoku

```
sudo apt install sudoku
```

### Q7:

Liste des derniers paquets installés explicitement avec la commande apt install

```
grep 'apt install' /var/log/apt/history.log

```

## Exercice 2

Recherche du paquet où est installée la commande ls 
```

which -a ls | xargs dpkg -S 2> /dev/null

```
On utilise  **xargs**  car la commande dpkg ne récupère pas en paramètre la sortie de la commande précédente. Car cette commande entre des valeurs en paramètres uniquement.  **-S**  lance une recherche sur les paquets installés.  **2> /dev/null**  redirige les erreurs dans ce répertoire qui est un répertoire de suppression.

Script prenant en argument le nom d’une commande, et indiquant quel paquet l’a installée.

```
#!/bin/bash
echo $(which -a $1 | xargs dpkg -S 2>/dev/null)
```

## Exercice 3

Commande qui affiche “INSTALLÉ” ou “NON INSTALLÉ” selon le nom et le statut du package spécifié dans cette commande.

```
#!/bin/bash
(dpkg -l | grep "$1" ) 1>/dev/null && echo "INSTALLÉ" || echo "NON INSTALLÉ"
```
## Exercice 4

Liste des programmes livrés avec coreutils.
```
dpkg -L coreutils
```
La commande "[" permet de tester, vérifier des types de fichier et de comparer des valeurs. Elle permet donc d'ajouter des conditions à la liste.

## Exercice 5

#  TP3

## EXERCICE 1

### Q1:
Mettre a jour le système : 
```
sudo apt full-upgrade
```

### Q2:
Compter le nombre de packages installés :
```
sudo apt list -i | wc -l
sudo dpkg -l | wc -l
```
le comptage avec apt nous donne 549 résultats et celui avec dpkg 553. Cette différence s'explique par le fait que certains packages ont été installés avec la commande dpkg c'est à dire qu'ils ne gèrent pas les dépendances.

### Q3:
La commande suivante permet de compter le nombre de packages disponibles au téléchargement :
```
sudo apt list | wc -l
```

### Q4 :

Création d'un alias de mise a jour

Il faut d'abord ajouter la ligne dans le fichier /.bash_aliases
```
alias maj='sudo apt update && sudo apt upgrade'
```
Puis pour prendre en compte ce changement taper la commande
```
source ~./bash_aliases
```

### Q5:

Installation de fortunes
```
sudo apt install fortunes
```


### Q6:

Installation du jeu sudoku

```
sudo apt install sudoku
```

### Q7:

Liste des derniers paquets installés explicitement avec la commande apt install

```
grep 'apt install' /var/log/apt/history.log

```

## Exercice 2

Recherche du paquet où est installée la commande ls 
```

which -a ls | xargs dpkg -S 2> /dev/null

```
On utilise  **xargs**  car la commande dpkg ne récupère pas en paramètre la sortie de la commande précédente. Car cette commande entre des valeurs en paramètres uniquement.  **-S**  lance une recherche sur les paquets installés.  **2> /dev/null**  redirige les erreurs dans ce répertoire qui est un répertoire de suppression.

Script prenant en argument le nom d’une commande, et indiquant quel paquet l’a installée.

```
#!/bin/bash
echo $(which -a $1 | xargs dpkg -S 2>/dev/null)
```

## Exercice 3

Commande qui affiche “INSTALLÉ” ou “NON INSTALLÉ” selon le nom et le statut du package spécifié dans cette commande.

```
#!/bin/bash
(dpkg -l | grep "$1" ) 1>/dev/null && echo "INSTALLÉ" || echo "NON INSTALLÉ"
```
## Exercice 4

Liste des programmes livrés avec coreutils.
```
dpkg -L coreutils
```
La commande "[" permet de tester, vérifier des types de fichier et de comparer des valeurs. Elle permet donc d'ajouter des conditions à la liste.

## Exercice 6

Installation de la version Oracle de Java (avec l’ajout des PPA)

```
sudo add-apt-repository ppa:linuxuprising/java
sudo apt update
sudo apt install oracle-java11-installer
```

## Exercice 7

Création d'un dépôt personnalisé

### 1) Préparation du dossier

Dans le dossier scripts créé lors du TP 2, créez un sous-dossier origine-commande où vous créerez un sous-dossier DEBIAN, ainsi que l’arborescence usr/local/bin où vous placerez le script écrit à l’exercice 2 

### 2)  Création du fichier control

Dans le dossier DEBIAN, créez un fichier control avec les champs suivants :
```
Package: origine-commande #nom du paquet**

Version: 0.1 #numéro de version**

Maintainer: Foo Bar #votre nom**

Architecture: all #les architectures cibles de notre paquet (i386, amd64...)**

Description: Cherche l'origine d'une commande**

Section: utils #notre programme est un utilitaire**

Priority: optional #ce n'est pas un paquet indispendable**
```
### 3) Build du packet 

Revenez dans le dossier parent de origine-commande (normalement, c’est votre $HOME) et tapez la commande suivante pour construire le paquet :

```
dpkg-deb --build origine-commande
```

### 4) Création du dépôt personnel

- Dans votre dossier personnel, commencez par créer un dossier repo-cpe. Ce sera la racine de votre dépôt
- Ajoutez-y deux sous-dossiers : conf (qui contiendra la configuration du dépôt) et packages (qui contiendra nos paquets)
- Dans conf, créez le fichier distributions suivant :
 ```
Origin: Un nom, une URL, ou tout texte expliquant la provenance du dépôt
Label: Nom du dépôt
// Suite: stable
Codename: cosmic #le nom de la distribution cible
Architectures: i386 amd64 #(architectures cibles)
Components: universe #(correspond à notre cas)
Description: Une description du dépôt
```
-Dans le dossier repo-cpe, générez l’arborescence du dépôt avec la commande reprepro -b . export

-Copiez le paquet origine-commande.deb créé précédemment dans le dossier packages du dépôt, puis, à la racine du dépôt, exécutez la commande afin que votre paquet soit inscrit dans le dépôt :

```
reprepro -b . includedeb cosmic origine-commande.deb
```

- Il faut à présent indiquer à apt qu’il existe un nouveau dépôt dans lequel il peut trouver des logiciels. Pour cela, créez (avec sudo) dans le dossier /etc/apt/sources.list.d le fichier repo-cpe.list contenant :

```
deb file:/home/VOTRE_NOM/repo-cpe cosmic multiverse
```
- Lancez la commande sudo apt update. 
Féliciations ! Votre dépôt est désormais pris en compte ! ... Enfin, pas tout à fait... Si vous regardez la sortie d’apt update, il est précidé que le dépôt ne peut être pris en compte car il n’est pas signé. La signature permet de vérifier qu’un paquet provient bien du bon dépôt. On doit donc signer notre dépôt.

#### Signature du dépôt avec GPG

GPG est la version GNU du protocole PGP (Pretty Good Privacy), qui permet d’échanger des données de manière sécurisée. Ce système repose sur la notion de clés de chiffrement asymétriques (une clé publique et une clé privée)

- Commencez par créer une nouvelle paire de clés avec la commande :
```
gpg --gen-key
```

- Ajoutez à la configuration du dépôt (fichier distributions la ligne suivante : SignWith: yes

- Ajoutez la clé à votre dépôt :
 ```
reprepro --ask-passphrase -b . export
```
**Attention ! Cette méthode n’est pas sécurisée et obsolète ; dans un contexte professionnel, on utiliserait plutot un gpg-agent.**

- Ajoutez votre clé publique à votre dépôt avec la commande
```
gpg --export -a "auteur" > public.key
```

- Enfin, ajoutez cette clé à la liste des clés fiables connues de apt :
```
sudo apt-key add public.key
```

Nous pouvons donc maintenant installer notre paquet comme n'importe quel autre paquet.

## Exercice 8

 Installation d’un logiciel à partir du code source 

Exemple : installation de nudoku

 Lorsqu’un logiciel n’est disponible ni dans les dépôts officiels, ni dans un PPA, ou encore parce qu’on souhaite n’installer qu’une partie de ses fonctionnalités, on peut se tourner vers la compilation du code source. Malheureusement, cette installation ”à la main” fait qu’on ne propose pas des bénéfices de la gestion de paquets apportée par dpkg ou apt. Heureusement, il est possible de transformer un logiciel installé ”à la main” en un paquet, et de le gérer ensuite avec apt ; c’est ce que permet par exemple checkinstall.

### 1. Clonage du code source
```
git clone https://github.com/jubalh/nudoku
```

### 2. Lancer l'autoreconf

Rendez vous dans le dossier nudoku qui vient d’être créé et lancez la commande autoreconf -i (ainsi que spécifié dans le fichier README.md). A vous d’installer les éventuels paquets manquants (un peu d’aide : pour résoudre le problème de la macro AM_GNU_GETTEXT manquante, installez le paquet gettext). Relancez la commande  `autoreconf -i`  après chaque paquet installé jusqu’à ce qu’elle se termine sans erreur.

### 3. Executer le script configure

### 4. Installation 

Normalement, à cette étape on exécute la commande make install, qui procède à la compilation proprement dite et à l’installation (copie des fichiers compilés dans leur dossier de destination). Mais dans notre cas, on va demander à checkinstall de s’en charger et de créer un paquet au format .deb : sudo checkinstall Le logiciel est à présent installé (exécutez nudoku pour vous en assurer) ; on peut vérifier par exemple avec aptitude qu’il provient bien du paquet qu’on a créé avec checkinstall.

```
sudo checkinstall
```

Le logiciel est à présent installé (exécutez nudoku pour vous en assurer) ; on peut vérifier par exemple avec aptitude qu’il provient bien du paquet qu’on a créé avec checkinstall.