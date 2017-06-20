---
layout: post
title:  "Du format Bruker au format ISMRMRD"
date:   2017-06-11 16:48:29 +0200
categories: jekyll update
lang: fr
---
> Ceci est une page en construction.

## 1) Sommaire

* [Les séquences qui fonctionnent](#fonctionnent)

* [Mode d'emploi: Utiliser le convertisseur](#emploi)

* [Mode d'emploi: Reconstruire avec Gadgetron](#gt)

* [Installation : Les prérequis](#prerequis)

* [Installation : BruKitchen](#brukitchen)

* [Installation : ISMRMRD ](#ismrmrd)

* [Installation : cute ](#cute)

* [Les séquences à ajouter](#gerer)

&nbsp;

### 1.1) 	Les séquences qui fonctionnent <a id="fonctionnent"></a>

Le passage en format `ISMRMRD` permet d'utiliser les algorithmes de reconstruction présent dans le `Gadgetron`. La conversion s'effectuera correctement si l'une de ces séquences suivantes est utilisée:

* 2D
  * Normal, 1 Slice  (128x128)
  * GRAPPA Y, 1 Slice  (76x128)
  * Normal, 3 slices  (128x128x3)
  * GRAPPA Y, 3 Slices (76x128x3)
  * GRAPPA Y, 1 Slice, 2 répetitions  
  * GRAPPA Y,  3 Slices, 2 répetitions
  * E72 2D complet (matrix size 128*128)  
  * E73 2D complet (matrix size 114*114)
  * E74 2D complet (matrix size 99*100)
  * E75 2D partial fourier (matrix size 128*128)
  * E76 2D partial fourier (matrix size 114*114)
  * E77 2D partial fourier (matrix size 99*100)
  * 2D grappa accelaration 2 (matrix size 128*128) E78
  * 2D complet (matrix size 128*128) (5 slices, 4 repetitions, 3 averages ) E80 (interlaced)
  * 2D complet (matrix size 128*128) (5 slices, 4 repetitions, 3 averages ) E82 (sequential)

* 3D
  * Normal (128x128x128)
  * GRAPPA Y (128x76x128)


&nbsp;

### 1.2) Mode d'emploi:	Utiliser le convertisseur <a id="emploi"></a>

Ouvrir le script `generic_create_dataset_from_bruker.m` situé dans le dossier `bruker-to-ismrmrd-matlab/`

{% highlight ruby %}
matlab cute/bruker-to-ismrmrd-matlab/generic_create_dataset_from_bruker.m &
{% endhighlight %}

Modifier le lien vers le répertoire d'acquisition `Bruker` et le nom du fichier qui contiendrait la fid au format `ISMRMRD`.

{% highlight ruby %}

acquisition_path='/home/user/DICOM/protocol_name/4/'
output_filename = '/home/user/DICOM/testdata.h5';
{% endhighlight %}

Lancer le script.  

&nbsp;

### 1.3) Mode d'emploi: Reconstruire avec Gadgetron <a id="emploi"></a>

Ouvrir deux consoles sur le calculateur et lancer le `Gadgetron` dans une des consoles, le message suivant s'affiche

{% highlight ruby %}
valeryo@IHUSUX001:/home/Partage/Dev/BruKitchen$ gadgetron
2017-06-12 14:04:37 INFO [main.cpp:200] Starting ReST interface on port 9080
2017-06-12 14:04:37 INFO [main.cpp:212] Starting cloudBus: localhost:8002
2017-06-12 14:04:37 INFO [main.cpp:260] Configuring services, Running on port 9002
{% endhighlight %}

Dans la seconde console, les lignes de commandes suivantes permettent d'effectuer la reconstruction, il est nécessaire de modifier le fichier de configuration suivant le type de séquence.

{% highlight ruby %}

/usr/local/bin/gadgetron_ismrmrd_client -c default_short_phase.xml -f /home/user/DICOM/testdata.h5 -o fichier_de_sortie.h5

/usr/local/bin/gadgetron_ismrmrd_client -c Generic_Cartesian_Grappa.xml -f /home/user/DICOM/testdata.h5 -o fichier_de_sortie.h5

/usr/local/bin/gadgetron_ismrmrd_client -c Generic_Cartesian_Grappa_Phase_Bruker.xml -f /home/user/DICOM/testdata.h5 -o fichier_de_sortie.h5

{% endhighlight %}

Attention le `coil compression` est activé par défaut, il est peut-être judicieux de le désactiver.
Il est possible d'ouvrir les fichiers de sortie en format `.h5` sous `Matlab` en utilisant le script `lecture_image_3D.m`

&nbsp;

### 1.4) 	Les séquences à gérer <a id="gerer"></a>

* 2D complet (matrix size 99*100) (5 slices, 4 repetitions, 3 averages ) E81
le probleme semble le readout , il est impair.
* mp2rage
* epi
* ute
* radial

&nbsp;

### 2.1) Installation : Les prérequis <a id="prerequis"></a>

Le convertisseur `bruker-to-ismrmrd-matlab` est disponible sur le répertoire [FBU](https://github.com/FBU-IHU-LIRYC).

Si vous avez accès au calculateur de l'`IHU`, `Brukitchen` et le convertisseur `Bruker-to-ismrmrd-matlab` sont déjà installés dans le répertoire `Partage/`

&nbsp;

### 2.2)	Installation : BruKitchen <a id="brukitchen"></a>

`Bruker-to-ismrmrd-matlab` utilise `BruKitchen`, disponible à cette [adresse](https://github.com/tesch1/BruKitchen.git).

Un fork existe aussi [ici](https://github.com/valeryozenne/BruKitchen.git). Un README est diponible mais l'installation est très rapide.

&nbsp;

{% highlight ruby %}

git clone https://github.com/tesch1/BruKitchen.git

{% endhighlight %}

Ouvrir matlab et lancer le script `BruKitchen/matlab/brukitchen_setup.m`

Pour finir il est possible d'ajouter le path au fichier `pathdef.m` de Matlab, ou d'utiliser à chaque fois la commande `addpath()` de Matlab

{% highlight ruby %}

sudo nedit /usr/local/MATLAB/R2015b/toolbox/local/pathdef.m

# et ajouter la ligne suivante après %%% BEGIN ENTRIES %%%

 '/home/Partage/Dev/BruKitchen/matlab:', ...

{% endhighlight %}

&nbsp;


### 2.3) 	Installation : le format ISMRMRD <a id="ismrmrd"></a>

Un tutorial en anglais est disponible [ici](https://github.com/ismrmrd/ismrmrd).

&nbsp;

### 2.4) 	Installation de Bruker-to-ismrmrd-matlab

Télécharger le répertoire suivant:

{% highlight ruby %}

git clone https://github.com/FBU-IHU-LIRYC/cute

{% endhighlight %}

C'est un ensemble de fichier `Matlab`, il n'y a donc rien à faire.
