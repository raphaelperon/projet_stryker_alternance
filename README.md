# Segmentation et modélisation 3D de l'articulation de l'épaule

Ce projet propose une solution automatisée pour la segmentation et la reconstruction 3D de la scapula et de l'humérus à partir d'images scanner (CT-scan). L'objectif est de générer des modèles anatomiques précis au format STL, utilisables pour la planification chirurgicale préopératoire.

**Présentation technique**

Le pipeline repose sur une architecture Deep Learning de type U-Net 3D. Le processus est divisé en trois phases principales : l'entraînement spécifique par structure osseuse, l'inférence par fenêtre glissante et la reconstruction de surface.

**Données et méthodologie**

**Source des données** : L'entraînement et la validation ont été réalisés à partir du dataset TotalSegmentator, offrant des segmentations anatomiques de référence robustes.

**Segmentation** : Réseau U-Net 3D avec blocs de double convolution, normalisation par lots (BatchNorm) et connexions résiduelles (Skip Connections).

**Prétraitement** : Normalisation des unités Hounsfield (HU) sur une plage de -200 à 1800 pour isoler les densités osseuses et extraction de patches 3D de 64x64x64 voxels.

**Post-traitement** : Filtrage des composantes connexes pour éliminer le bruit, remplissage des cavités internes et lissage gaussien des masques binaires.

**Résultats et Performances**

L'évaluation des modèles sur les données de test montre une excellente capacité de généralisation. Les scores de Dice obtenus pour la segmentation sont les suivants :

**Scapula : 0.909**

**Humérus : 0.964**

Ces scores valident la précision anatomique des masques produits, permettant une reconstruction 3D fidèle via l'algorithme des Marching Cubes.

**Structure du dépôt**

Le projet est organisé en trois notebooks principaux :

train_scapula.ipynb : Préparation des données et entraînement du modèle dédié à la scapula.

train_humerus.ipynb : Processus d'entraînement spécifique pour l'humérus.

epaule3d.ipynb : Pipeline de production. Chargement des poids des modèles, inférence sur patient test et export des fichiers STL finaux.

**Installation et dépendances**

Les notebooks nécessitent un environnement Python 3 avec les bibliothèques suivantes :

pip install torch torchvision torchaudio

pip install nibabel

pip install scikit-image

pip install scipy

pip install numpy

pip install matplotlib

**Utilisation**

Exécuter les notebooks d'entraînement pour générer les fichiers de poids .pth.

Renseigner les chemins des modèles dans le dictionnaire de configuration du notebook d'inférence.

Lancer le notebook de modélisation pour générer les fichiers scapula_patient.stl et humerus_patient.stl.

Les maillages générés respectent les dimensions anatomiques originales, permettant une intégration directe dans des logiciels de planification chirurgicale ou de visualisation 3D.
