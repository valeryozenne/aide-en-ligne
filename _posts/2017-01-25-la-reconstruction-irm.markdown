---
layout: post
title:  "La reconstruction IRM"
date:   2017-01-25 16:48:29 +0200
categories: jekyll update
lang: fr
---
> Ceci est une page en construction.

## 1) Sommaire

Bienvenue sur ce tutorial consacré à la reconstruction des images IRM. Nous allons utiliser des images provenant de la banque de données Osirix disponibles à cette adresse: [lien](http://www.osirix-viewer.com/resources/dicom-image-library/)

* [Exemple 1: Transformée de Fourier inverse](#exemple1)

* [Exemple 2: Filtrage des hausses et basses fréquences](#exemple2)

* [Exemple 3: ](#exemple3)

* [Script complet](#script)

### Exemple 1: Transformée de Fourier inverse <a id="exemple1"></a>


Lecture d'un fichier Dicom avec la fonction `dicomread` puis conversion en format `double`. Diminution de la taille de l'image et affichage.

{% highlight ruby %}
clear all
close all

filename='/home/valery/Telechargements/BRAINIX/BRAINIX/IRMcerebrale,neuro-crâne/sT2-TSE-T- 01/IM-0001-0010.dcm';
img_tempo=double(dicomread(filename));
img=imresize(img_tempo,0.5);

ismrm_imshow(img, [], [], {'Image'});

{% endhighlight %}

![image1](../../../../../images/reconstruction/image1.png){:height="400px" }

Calculons la transformée de Fourier de l'image pour obtenir l'image dans l'espace réciproque (dit kspace pour la suite). Nous utilisons la fonction `fft_2D` qui effectue la transformée suivant x puis suivant y et affichons cête à cote l'image et le kspace.


{% highlight ruby %}
function [ out ] = fft_2D( in )

K = fftshift(fft(fftshift(in,1),[],1),1); 

out = fftshift(fft(fftshift(K,2),[],2),2);

end
{% endhighlight %}


{% highlight ruby %}
kspace=fft_2D(img);
ismrm_imshow( [img*100 ,abs(kspace)], [0 1e5], [] , {'Image vs Kspace'} );

{% endhighlight %}






![image2](../../../../../images/reconstruction/image2.png)


### Exemple 2: Filtrage des hausses et basses fréquences <a id="exemple2"></a>

Creation du mask On enleve le centre du mask

{% highlight ruby %}

mask=ones(size(img));
center=round(size(img,1))/2;
offset=32;
mask(center-offset:center+offset,center-offset:center+offset)=0;

{% endhighlight %}

On applique le masque sur le kspace. On effectue la reconstruction et nous comparons les resultats Affichage des images

{% highlight ruby %}
kspace_crop_ext=kspace .* mask;
kspace_crop_in=kspace-kspace_crop_ext;

img_crop_ext=ifft_2D(kspace_crop_ext);
img_crop_in=ifft_2D(kspace_crop_in);

ismrm_imshow(cat(3,abs(kspace),abs(kspace_crop_ext),abs(kspace_crop_in),abs(img) * 100, abs(img_crop_ext) * 100, abs(img_crop_in) * 100),...
    [0 1e5],[2 3], {'Kspace Complet', 'Kspace sans le centre', 'Kspace sans exterieur', 'Image', 'Haute Fréquence|', 'Basse Fréquence'});

{% endhighlight %}

![image3](../../../../../images/reconstruction/image3.png)

### Exemple 3: <a id="exemple3"></a>

### Script complet: <a id="script"></a>

Le script complet est disponible ici: [lien]()
