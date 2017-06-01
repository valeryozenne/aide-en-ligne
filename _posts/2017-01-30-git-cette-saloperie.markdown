---
layout: post
title:  "Git"
date:   2017-01-30 16:48:29 +0200
categories: jekyll update
lang: fr
---
> Ceci est une page en construction.

## 1) Sommaire

* [Présentation de l'environnement](#presentation)

* [Créer son répertoire local](#local)

* [Développement en routine (MEGA IMPORTANT)](#routine)

* [Synchroniser le code avec les changements du NIH](#nih)

* [Gérer les conflits](#conflits)

## 2) Présentation de l'environnement <a id="presentation"></a>

D'un point de vue architecture nous travaillons à différents niveaux.

* gadgetron le vrai (public) nommé NIH

* gadgetron-ihu (fork de NIH, privé, nommé FBU)

* gadgetron-ihu personnel (fork de FBU, privé, nommé VALERY, PIERRE, ...)

* en local (serveur de calcul et différentes stations, privé)

## 3) Créer son répertoire local <a id="local"></a>

Après création de son compte Git, nous demandons à l'administrateur un accès au compte `FBU`.

Ensuite on clique sur le bouton `fork` pour créer une copie du `gadgetron-ihu` sur son espace personnel Github.

Enfin en local sur son pc on effectue la commande suivante (de préférence dans `/home/name/Dev/`)

{% highlight ruby %}
$ git clone	https://github.com/valeryozenne/gadgetron-ihu.git
{% endhighlight %}

pour vérifier le lien entre le code et le dépot on peut taper la commande `remote`
{% highlight ruby %}
$ git remote -v
origin	https://github.com/valeryozenne/gadgetron-ihu.git (fetch)
origin	https://github.com/valeryozenne/gadgetron-ihu.git (push)
{% endhighlight %}

par convention nous allons renommer le lien en lettre majuscule avec la commande `remote rename`

{% highlight ruby %}
$ git remote rename origin VALERY
$ git remote -v
VALERY	https://github.com/valeryozenne/gadgetron-ihu.git (fetch)
VALERY	https://github.com/valeryozenne/gadgetron-ihu.git (push)
{% endhighlight %}

nous allons aussi ajouter les liens vers les repertoires distants `NIH` et `FBU` avec les commandes suivantes

{% highlight ruby %}
$ git remote add https://github.com/FBU-IHU-LIRYC/gadgetron-ihu.git FBU
$ git remote add https://github.com/gadgetron/gadgetron.git NIH
{% endhighlight %}

la commande remote nous donne alors :

{% highlight ruby %}
$ git remote -v
FBU	https://github.com/FBU-IHU-LIRYC/gadgetron-ihu.git (fetch)
FBU	https://github.com/FBU-IHU-LIRYC/gadgetron-ihu.git (push)
NIH	https://github.com/gadgetron/gadgetron.git (fetch)
NIH	https://github.com/gadgetron/gadgetron.git (push)
VALERY	https://github.com/valeryozenne/gadgetron-ihu.git (fetch)
VALERY	https://github.com/valeryozenne/gadgetron-ihu.git (push)
{% endhighlight %}

ensuite nous pouvons utiliser la commande `checkout` pour regarder les différents versions.
{% highlight ruby %}
$ git checkout FBU/integration
{% endhighlight %}

## 4) Developpement routine (MEGA IMPORTANT) <a id="routine"></a>

Le développe routine s'articule autour de ces différentes étapes.

Hypothèses: FBU doit être à jour en local. Si ce n'est pas le cas et même si c'est le cas on commence par faire un `fetch`.
La fonction `fetch` vérifie la présence de changements entre la version locale et la version distante.

{% highlight ruby %}
$ git fetch FBU
{% endhighlight %}

Ensuite on se déplace sur la branche intégration de FBU (qui est la branche par défaut)

{% highlight ruby %}
$ git checkout FBU/integration
HEAD est maintenant sur bc81dc4... Merge pull request #23 from valeryozenne/mise_a_jour_NIH_janvier
{% endhighlight %}

Il nous dit que le pointeur (cf fonctionnement de git) est maintenant sur FBU/integration et affiche le texte correspondant à la dernière modification. A prtir de ce moment là, nous avons la possibilité de travailler.

Une fois le travail réalisé, nous utilisons les fonctions git add et git commit pour valider le travail

Ensuite nous allons créer une nouvelle branche correspondant à ces changements avec la commande `checkout -b` et nous l'envoyons sur notre répertoire distant personnel (`VALERY`) avec la commande `push`

{% highlight ruby %}
$ git checkout -b nom_de_la_branche
$ git push VALERY nom_de_la_branche
{% endhighlight %}

Nos modifications ont été envoyés sur notre répertoire distant personnel. Elles attendent une validation avant d'être envoyée sur le FBU/intégration pour que tous les acteurs du projet puissent bénéficier des avancements et corrections. Pour cela nous allons sur GitHub et nous créons une `pull request`. Une fois la pull request validé, il reste plusieurs étapes à faire.

En local avant de continuer à travailler sur un nouveau code, il faut mettre à jour FBU/integration pour toujours repartir de la dernière version. Puis nous rebasculons sur l'entête de integration en local.

{% highlight ruby %}
$ git fetch FBU
$ git checkout FBU/integration
{% endhighlight %}

Les derniers changements ayant été pris en compte, nous sommes prêts de nouveau à travailler et à créer une nouvelle branche.

## 5) Synchroniser le code avec les changements du NIH <a id="nih"></a>

Il s'agit à intervale régulier de récupérer les changements réalisés par l'équipe du Gadgetron et de rajouter le code dans `FBU`. Si nous n'effectuons pas ce travail, nous risquons de diverger sévèrement du travail réalisé par l'équipe du Gadgetron.

Attention, une mise à jour de `FBU/integration` avec les derniers codes de `NIH/master` peut parfois perturber le fonctionnement du code. Par exemple, l'ajout de nouvelles fonctionnalités n'est pas testé sur nos plateformes et peut bugger à la compilation, perturbant ainsi notre travail et un possible examen irm. Pour cela nous avons créer deux branches sur `FBU`: la branche `master` et la branche `integration`.

* La branche `FBU/master` est la branche stable, déployée sur les IRM et le serveur de calcul. `FBU/master` doit toujours compiler, fonctionner et gérer tous les cas et multiples aspects du temps réel (fichierDeConfig.xml à jour). Elle est forcément un peu en retard sur `FBU/integration` mais elle est là pour assurer.
* La branche `FBU/integration` est la branche de travail, elle est aussi déployée sur différentes stations et le serveur de calcul mais elle peut merdouiller de temps en temps.

Hypothèse préalable. Tous nos travaux sont commités et `NIH/master` et `FBU/intégration` sont à jour en local. Mais dans tous les cas nous commençons par le vérifier.

{% highlight ruby %}
$ git fetch NIH
$ git fetch FBU
{% endhighlight %}

Pour basculer sur la branche intégration de FBU (qui est la branche par défaut)

{% highlight ruby %}
$ git checkout FBU/integration
{% endhighlight %}

Maintenant nous allons rejouer le travail du Gadgetron (`NIH`) avec la commande `pull --rebase`
{% highlight ruby %}
$ git pull --rebase NIH master
{% endhighlight %}

Il faut s'attendre à des conflits... généralement, nos travaux ont été placés dans des répertoires différents de ceux utilisés par l'équipe du Gadgetron pour limiter les conflits. Ainsi les conflits apparaissent dans les cmakelist.txt et rarement dans les fonctions. Si le nombre de changements est trop important nous risquons de devoir gèrer des conflits dans le code du Gadgetron et c'est loin d'être idéal. Il faut alors vérifier sur le `GitHub` en ligne quelles changements sont à prendre.

Une fois les conflits édités , nosu commitons le code et continous le rebase avec `git rebase --continue`
 puis nous créons une nouvelle branche que nous allons appeler mise_jour_NIH_janvier par exemple.
{% highlight ruby %}
$ git add
$ git commit -m " gestion des conflits suites à la mise à jour de janvier"
$ git rebase --continue
{% endhighlight %}

Nous créons une nouvelles branches qui est chargée d'apporter ces changements à FBU par la processus décrit dans le paragraphe précédent.

{% highlight ruby %}
$ git checkout -b mise_jour_NIH_janvier
{% endhighlight %}

A ce niveau, nous nous devons de compile le Gadgetron et de vérifier le bon fonctionnement des chaines de reconstructions avec les scripts offline.
Puis nous pouvons pousser les changements sur notre répertoire distant personnel sur la branch intégration et ensuite demander une pull request

{% highlight ruby %}
$ git push VALERY nom_de_la_branche
{% endhighlight %}

Une fois la pull request accepté nous finissons le travail en mettant à jour nos codes en local et rebasculons sur l'entête de `FBU/intégration` pour être pret à travailler à nouveaux.

{% highlight ruby %}
$ git fetch FBU
$ git checkout FBU/integration
{% endhighlight %}


## 6) Gérer les conflits <a id="conflits"></a>

Soit la situation suivante:

{% highlight ruby %}
----|---> A ---> x ---> pull request
    |---> B  
{% endhighlight %}

La branche A a avancée et a été mergée dans FBU integration.
La branche B avance mais plus lentement. Lors du pull request de la branche B, il n'est plus possible de merger si B a modifié qcq chose appartenant à A.
Pour cela, il faut rejouer l'historique de la branche A sur B avant de la merger.

{% highlight ruby %}

git stash
git pull --rebase FBU integration

gedit gadgets/noncartesian_liryc/CMakeLists.txt &
git add gadgets/noncartesian_liryc/CMakeLists.txt

git rebase --continue
git push VALERY sms_and_dense

{% endhighlight %}

{% highlight ruby %}
  git push -f VALERY sms_and_dense
{% endhighlight %}
