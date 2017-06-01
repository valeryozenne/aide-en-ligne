---
layout: post
title:  "La reconstruction SENSE"
date:   2017-01-26 16:48:29 +0200
categories: jekyll update
lang: fr
---
> Ceci est une page en construction.

# 1) Sommaire

Bienvenue sur ce tutorial consacré à la reconstruction des images IRM. Nous allons utiliser des images provenant de la banque de données Osirix disponibles à cette adresse: [lien](http://www.osirix-viewer.com/resources/dicom-image-library/)

* [La reconstruction SENSE en mode rapide:](#moderapide)

* [La reconstruction SENSE en détail:](#detail)

* [Script complet](#script)

### La reconstruction SENSE en mode rapide <a id="moderapide"></a>

Lecture d'un fichier Dicom avec la fonction `dicomread` puis conversion en format `double`. Diminution de la taille de l'image et affichage.


{% highlight ruby %}

close all;
clear all;

filename='/home/valery/Téléchargements/BRAINIX/BRAINIX/IRM cérébrale, neuro-crâne/sT2-TSE-T - 301/IM-0001-0010.dcm'
im1_tempo=double(dicomread(filename));
img_originale=imresize(im1_tempo,0.5);

ismrm_imshow(img_originale);

{% endhighlight %}

![image_depart](../../../../../images/sense/image_depart.png){:height="400px" }


Chargement des cartes de sensibilités et affichage.

{% highlight ruby %}

addpath('/home/valery/Reseau/Valery/MatlabUnix/ismrm_sunrise_matlab-master/');
load smaps_phantom.mat

ismrm_imshow(abs(smaps),[min(abs(smaps(:))) max(abs(smaps(:)))],[2 4]);
ismrm_imshow(angle(smaps),[min(angle(smaps(:))) max(angle(smaps(:)))],[2 4]);

%  Echantillonnage avec 8 antennes et carte de sensibilites
[data_1, sp_1] = ismrm_sample_data(img_originale, smaps, 1);
nCoils=size(data_1,3);

{% endhighlight %}

{% highlight ruby %}

I_raw_1=ifft_2D(data_1);

ismrm_imshow(abs(I_raw_1),[0 max(abs(I_raw_1(:)))],[2 4]);
ismrm_save_image( outputFolder, 'raw_image_abs' );

ismrm_imshow(angle(I_raw_1),[-pi pi],[2 4]);
ismrm_save_image( outputFolder, 'raw_image_angle' );

{% endhighlight %}




{% highlight ruby %}

readout=size(img_originale,1);
N=size(img_originale,2);

%  Ici nous avons le kspace

K_raw_for_sense=zeros(size(data_1,1),size(data_1,2),size(data_1,3));
samp_mat=zeros(size(data_1,1),size(data_1,2));

for p=1:2:size(data_1,2)

    K_raw_for_sense(:,p,:)=data_1(:,p,:);
    samp_mat(:,p)=1;
end

I_raw_for_sense=ifft_2D(K_raw_for_sense);

figure()
for c=1:1:nCoils
    subplot(2,4,c);imagesc(abs(I_raw_for_sense(:,:,c))); colormap(gray);
end

acc_factor=2

[unmix_sense, gmap]   = ismrm_calculate_sense_unmixing(acc_factor, smaps);

inp=K_raw_for_sense;
img_alias = sqrt(4) * ismrm_transform_kspace_to_image(K_raw_for_sense,[1,2]);
img_reco_sense = sum(img_alias .* unmix_sense,3);

{% endhighlight %}


### La reconstruction SENSE en détaillé <a id="detail"></a>


### Script complet: <a id="script"></a>

Le script complet est disponible ici: [lien]()
