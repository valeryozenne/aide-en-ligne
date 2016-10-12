---
layout: post
title:  "Astuces VTK ITK"
date:   2016-10-10 09:48:44 +0100
categories: jekyll update
lang: en
---
Cet article se situe dans le dossier `_posts`. Allez l'éditer, et générez votre site à nouveau pour voir les modifications. Vous pouvez générer le site de différentes façons, mais le plus efficace est de lancer la commande `jekyll serve`, qui crée un serveur web et génère automatiquement votre site à chaque fois qu'un fichier est modifié.

Pour ajouter un autre article, créez un nouveau fichier dans le dossier `_posts` dont le nom contient la date de la façon suivante : `AAAA-MM-JJ-nom-de-l-article.ext` et placez-y l'entête.  includes the necessary front matter. Regardez le code source de cet article pour avoir une bonne idée de la façon dont cela fonctionne.

Jekyll permet aussi d'intégrer des extraits de code :

{% highlight ruby %}

sudo chown -R valery ~/.matlab/
sudo chgpr -R valery ~/.matlab/
sudo chgrp -R valery ~/.matlab/

#ubuntu 14.04

sudo apt-get install vtk6
sudo apt-get install libvtk6-dev
sudo apt-get install cmake insighttoolkit3-examples libfftw3-dev libinsighttoolkit3-dev libinsighttoolkit3.6
sudo apt-get install libinsighttoolkit4-dev libinsighttoolkit4.5  sudo apt-get install libpng12-dev libtiff4-dev
sudo apt-get install uuid-dev
sudo apt-get install libvtk5-dev python-vtk
1393  sudo apt-get install libvtk5-dev
1394  sudo apt-get install python-vtk6

#ubuntu 16.04

# Install ITK Python wrapping
sudo apt-get install insighttoolkit4-python

# Install examples from the ITK Software Guide
sudo apt-get install insighttoolkit4-examples

# Install ITK C++ headers
sudo apt-get install libinsighttoolkit4-dev

# Install ITK library debugging symbols
sudo apt-get install libinsighttoolkit4-dbg



{% endhighlight %}

Jetez un coup d'oeil à la [documentation de Jekyll][jekyll-docs] pour en savoir plus sur ce qu'il vous est possible de faire avec Jekyll. Tous les bugs et demandes de fonctionnalités doivent être envoyés sous forme de requête sur [GitHub][jekyll-gh]. Si vous avez des questions, allez les poser sur le [fil d'aide de Jekyll][jekyll-talk].

[jekyll-docs]: http://jekyllrb.com/docs/home
[jekyll-gh]:   https://github.com/jekyll/jekyll
[jekyll-talk]: https://talk.jekyllrb.com/

http://stackoverflow.com/questions/23684789/cmake-build-multiple-executables-in-one-project-with-static-library
