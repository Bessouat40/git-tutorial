# Tuto Git

Dans ce tuto, nous allons voir comment se servir de git.

## Table of contents

1. [Fonctionnement en remote et en local](#fonctionnement-en-remote-et-en-local)

2. [Bonnes pratiques](#bonnes-pratiques)

3. [Initialiser un projet depuis un dossier local](#initialiser-un-projet-depuis-un-dossier-local)

4. [Cloner un repo sur son ordinateur](#cloner-un-repo-sur-son-ordinateur)

5. [Readme](#readme)

6. [Branches](#branches)

   1. [Créer une nouvelle branche](#créer-une-nouvelle-branche)

   2. [Changer de branche](#changer-de-branche)

   3. [Merger deux branche](#merger-deux-branches)

7. [Enregistrer son travail au fur et à mesure](#enregistrer-son-travail-au-fur-et-à-mesure)

8. [Gitlab-Runner](#gitlab-runner)

9. [Liste de commandes utiles](#liste-de-commandes-utiles)

## Fonctionnement en remote et en local

Lorsqu'on effectue des certaines commandes git, elles peuvent avoir un effet au niveau local ou bien en remote.

Il se peut que dans certains cas, on effectue une commande mais on ne voit pas ses effets sur gitlab en ligne.

**_Ex :_** _Il se peut qu'on veuille créer une branche pour travailler dessus. Si on ne fait pas attention, on peut la créer uniquement en local, et elle n'apparaitra donc pas sur gitlab en ligne._

Il faut donc faire attention à l'endroit impacté par nos commandes.

## Bonnes pratiques

### Bonnes pratiques générales

- Dans gitlab, on ne met pas de code contenant des informations sensibles

      ***Ex :*** *Si on utilise des appels à des bases de données dans notre code, on ne push pas de code contenant les usernames, passwords, ports,... On renseigne ces informations dans un fichier

  `.env` lorsqu'on travaille sur son ordinateur. Lorsqu'on push le code, on envoie un template nommé `.env.example` et on indique comment s'en servir dans le readme\*

- Le code présent dans un repo gitlab concerne le même projet, tout dans le repo sert au même projet et est utile à sa mise en production.

      ***Ex :*** *Si on code une API permettant de faire de la classification d'images mais qu'elle sera déployée seule en production, on en fait un repo à part. Si on déploie une application

  permettant de faire de la gestion de stock, on va mettre dans ce repo tout ce dont on a besoin : les API spécifiques, le code pour déployer la base de données si nécessaire,...\*

- Le code sur la branche main est le code mis en production. On ne le modifie pas.

Le code sur la branche develop est le code fonctionnel mais pas forcément déployé en production. Lorsqu'on veut merger notre travail, on le fait sur la branche develop.

- On assigne un reviewer lorsqu'on fait une merge request.

### Bonnes pratiques de nommage

- Nommage des projets : on choisit un titre en anglais qui est parlant et décrit au mieux le projet qu'on crée (ex : python-text-extraction). On ne met pas d'espaces dans les noms de projets.

- Nommage des branches : utilisation des issues Git ou Jira. Deux possibilité : notre branche correspond au `développement d'une nouvelle fonctionnalité` associée à l'issue 46, dans ce cas la
  branche se nommera : `#feat46/functionality-name`. Notre branche correspond à la `modification/l'amélioration d'un code existant`, dans ce cas la branche se nommera : `#edit/amelioration-name`.

- Nommage des issues : nom court, explicite. Si besoin, ajouter une description dans l'issue. On peut ajouter des labels (priority (high, medium, low), personne assignée, ...)

## Initialiser un projet depuis un dossier local

Si on souhaite créer un repo gitlab à partir d'un dossier existant sur notre ordinateur, il faut suivre ces étapes :

- Créer un repo vide sur gitlab portant le nom de notre dossier

- Ouvrir un invité de commande et se placer dans notre dossier

- `git init`

- `git add .`

- `git commit -m "First commit"`

- `git remote add origin <Github repository URL>`

- `git remote -v`

- `git push --set-upstream origin master`

Aller sur git faire une merge request :

![merge1](./screens/merge1.png)

Ensuite crée la merge request puis on résout les conflits :

![merge2](./screens/merge2.png)

![merge3](./screens/merge3.png)

Maintenant il ne reste plus qu'à merger :

![merge4](./screens/merge4.png)

Maintenant nous allons faire de la branche main la branche principale :

- `git checkout main`

- `git branch --set-upstream-to=origin/main`

- `git branch -d master`

Maintenant nous allons créer notre branche develop :

`git checkout -b develop main && git push -u origin develop`

**_Ressources :_**

- [Create Existing Directory as Repository in GIT](https://stackoverflow.com/questions/37627160/create-existing-directory-as-repository-in-git)

## Cloner un repo sur son ordinateur

- Aller sur le repo et cliquer sur clone1 :

![clone1](./screens/clone1.png)

- Copier le lien https :

![clone2](./screens/clone2.png)

Puis `commit on source branch`.

- Ouvrir un invité de commande et se placer dans le répertoire où on souhaite cloner le repo

- `git clone <lien https copié>`

## Readme

La rédaction d'un readme pour chaque projet est obligatoire.

Si possible, le readme doit être rédigé en anglais.

Dans le readme, on doit :

- Décrire les pré-requis pour le lancement du projet (librairies, configuration de certains logiciels, ...)

- Décrire précisément comment lancer le projet

### Exemple de readme propre

[Readme d'un projet exemple](https://aforge-frdr.biz.nstd.fr.airbusds.corp/gitlab/platform/fasapi-example)

## Branches

### Créer une nouvelle branche

Lorsqu'on souhaite travailler sur une nouvelle fonctionnalité ou juste éditer le code, on peut vouloir créer une nouvelle branche de travail.

Pour cela, on va utiliser cette commande :

`git checkout -b <new branch> <develop> && git push -u origin <new branch>`

De cette maniète, on va créer une nouvelle branche en local et en remote.

### Changer de branche

Lorsqu'on veut changer de branche de travail, on va utiliser cette commande :

`git checkout <nom de la branch>`

### Merger deux Branches

**_Attention : On ne merge sur main que ce qui est mis en prod. Sinon on merge sur la branche develop._**

Lorsqu'on veut merger deux branches on va faire comme ceci :

Imaginons qu'on souhaite merger la branche B sur la branche A (en supprimant la branche B) :

- `git checkout A`

- `git merge B`

On associe ensuite un `reviewer` qui va vérifier la merge request et l'accepter.

**_Ressources :_**

- [Doc merge (utile pour les conflits)](https://www.atlassian.com/fr/git/tutorials/using-branches/git-merge)

## Enregistrer son travail au fur et à mesure

Lorsqu'on souhaite enregistrer son travail sur gitlab, il faut réaliser ces commandes :

- `git status` pour voir les fichiers modifiés en local par rapport aux fichiers en remote

- `git add *` on ajoute tous les fichiers modifiés ( \* pour ajouter tous les fichiers, sinon on peut ajouter seulement certains fichiers)

- `git commit -m "message"`

- `git push`

## Gitlab-Runner

In order to execute CI/CD, you need to run runners that take your jobs.

To do that :

- You need to install `Gitlab-Runner` from `https://docs.gitlab.com/runner/install/index.html`.

- Go to `C:\GitLab-Runner` and open a `powershell` :

```BASH

.\gitlab-runner verify

.\gitlab-runner run

```

Now your gitlab-runner is running on your device.

## Liste de commandes utiles

- `git clone`

- `git status`

- `git add`

- `git commit`

- `git push`

- `git checkout`

- `git pull origin <branch>` : récupérer le code en remote sur la branche voulue

- `git fetch` : mettre de côté ses modifications
