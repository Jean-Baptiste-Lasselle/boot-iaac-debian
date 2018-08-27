# boot-iaac-debian
boot de la ligne de production, de la cible de déploiement, du poste devops

Un fichier de reccourci Eclipse Photon qui marche : 

```
[Desktop Entry]
Version=1.0
Name=Eclipse
Comment=Java IDE
Type=Application
Categories=Development;IDE;
Exec=/home/jean-baptiste/carrefour/usine/poste-devops/ide/eclipses/photon/1/eclipse -data "$HOME/carrefour/usine/poste-devops/ide/espaces-travail/1"
Terminal=false
StartupNotify=true
Icon=/home/jean-baptiste/carrefour/usine/poste-devops/ide/eclipses/photon/1/eclipse.icon
Name[en_US]=Eclipse
```



# Procédure de ré-initialisation de l’infra devops.

Créer les 6 repositories Git suivants : 

[REPO_HISTORIQUE_OPS_SRV_DEMO] le repo utilisé par les devops pour historiser toute opération sur le serveur de démo, i.e. 100.72.4.183. N.B.: il y aun bastion SSH au-dessus duquel on peut faire un jump, en 100.72.4.58. Le jump : 
ssh -i $HOME/.ssh/id_rsa -J ubuntu@100.72.4.58 ubuntu@100.72.4.183  
https://jean_baptiste_lasselle@bitbucket.agilefabric.fr.carrefour.com/scm/dev/cible-srv-demo-183.git
[REPO_HISTORIQUE_OPS_ENV_LOCAL] Le repo utilisé par les devops pour historiser toute opération sur leur propre machine de travail, considéré comme cible de déploiement (pourrait, devrait être inclut dans marguerite). https://jean_baptiste_lasselle@bitbucket.agilefabric.fr.carrefour.com/scm/dev/cible-locale-poste-jbl.git
[REPO_VERSIONING_STRAPI] : un fork interne de https://github.com/strapi/strapi/ https://jean_baptiste_lasselle@bitbucket.agilefabric.fr.carrefour.com/scm/dev/bootstrapi.git
[REPO_VERSIONING_DEV] le repo utilisé par les développeurs pour versionner leur production https://jean_baptiste_lasselle@bitbucket.agilefabric.fr.carrefour.com/scm/dev/developpements-bootstrapi.git
[REPO_VERSIONING_PROVISION_BOOTSTRAPI] (le repo utilisé par les devops pour versionner la recette de provision de bootstrapi) https://jean_baptiste_lasselle@bitbucket.agilefabric.fr.carrefour.com/scm/dev/provision-bootstrapi.git
[REPO_VERSIONNING_BOOT_LIGNE_PROD_BOOTSTRAPI] le repo utilisé par les devops : 
pour ré-initialiser l’ensemble des 6 repositories Git pour le IAAC de la ligne de production de bootstrapi
pour initialiser un poste devops debian stretch, en vue d’agir en mode IAAC sur l’univers « ligne de production bootstrapi ».
https://jean_baptiste_lasselle@bitbucket.agilefabric.fr.carrefour.com/scm/dev/cycle-devops.git


Je dois toujours commencer par les opérations faites dans le repo cycle-devops.

Note : pour les git push, avec tout l’historique, il faut l’option --set-upstream origin/master

# README.md
Pour initialiser toute l’infra de la ligne de production, sauf votre poste devops, exécutez :
./boot/infra-devops/operations.sh

Devra donc effectuer la mavenistation automatique des projets. Il suffit pour cela de placer un pom.xml bien formé, avec des sed pour changer l’artefactid, le groupid, artifactVersionNumber. Typiquement un rôle ansible, sachant qu’il est attendu que ce projets soient tous vides.
Je recommande donc d’ajouter les pom.xml dans les repo git, pusi ensuite de faire l’initialisation des repos. 
 

Pour initialiser votre environnement devops intégré, sur votre psote de travail : 
./boot/poste-devops/operations.sh
devra donc effectuer le 


# On commence par [REPO_VERSIONNING_BOOT_LIGNE_PROD_BOOTSTRAPI] – 
# ./boot/infra-devops/operations.sh
# ./boot/poste-devops/operations.sh => pour initialiser le poste devops avec eclipse, ce qui nécessite de maveniser les projets avec lesquels on travaille en tant que devops.


 - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - 
# ./boot/infra-devops/operations.sh

export MAISON_OPS=$HOME/ligne-prod-bootstrapi

export MAISON_BOOT_LIGNE_PROD=$HOME/boot-ligne-prod
URI_REPO_GIT_BOOT_LIGNE_PROD=https://jean_baptiste_lasselle@bitbucket.agilefabric.fr.carrefour.com/scm/dev/cycle-devops.git
export URI_REPO_GIT_BOOT_LIGNE_PROD

export MAISON_HISTORY_SRV_DEMO_183=$HOME/historique-ops-srv-demo.183
export  URI_REPO_GIT_HISTORY_SRV_DEMO_183=https://jean_baptiste_lasselle@bitbucket.agilefabric.fr.carrefour.com/scm/dev/cible-srv-demo-183.git

export MAISON_HISTORY_ENV_LOCAL=$HOME/historique-ops-env-local
export  URI_REPO_GIT_HISTORY_ENV_LOCAL=https://jean_baptiste_lasselle@bitbucket.agilefabric.fr.carrefour.com/scm/dev/cible-locale-poste-jbl.git

export MAISON_VERSIONING_BOOTSTRAPI=$HOME/versioning-bootstrapi
export URI_REPO_GIT_VERSIONING_BOOTSTRAPI=https://jean_baptiste_lasselle@bitbucket.agilefabric.fr.carrefour.com/scm/dev/bootstrapi.git

export MAISON_VERSIONING_DEV=$HOME/versionning-dev
export  URI_REPO_GIT_VERSIONING_DEV=https://jean_baptiste_lasselle@bitbucket.agilefabric.fr.carrefour.com/scm/dev/developpements-bootstrapi.git

export MAISON_VERSIONING_PROVISION_BOOTSTRAPI=$HOME/versionning-dev
export  URI_REPO_GIT_VERSIONING_DEV=https://jean_baptiste_lasselle@bitbucket.agilefabric.fr.carrefour.com/scm/dev/provision-bootstrapi.git




git clone  $REPO_VERSIONNING_BOOT_LIGNE_PROD_BOOTSTRAPI

# -----------------------------------------------
# INITIALISATION [REPO_HISTORIQUE_OPS_SRV_DEMO] : 
# -----------------------------------------------
#     le principe est que l’on doit
#     l’initialiser avec une première opération idempotente, laissant la cible 
#     de déploiement dans un état stable, reconnut par l’équipe technique, et
#     l’équipe métier (Product owners et business analysts)

export URI_REPO_GIT_A_INITIALISER=https://$USER@bitbucket.agilefabric.fr.carrefour.com/scm/dev/cible-srv-demo-183.git
export MAISON_OPS=$HOME/carrefour/usine/recettes/cibles-deploiement/srv-demo.183
export REPERTOIRE_CODE_A_VERSIONNER=$MAISON_OPS/recup-version-ini/docker/
export REPO_LOCAL_TEMP=$MAISON_OPS/iaac/repo_local_temp

git clone ‘’$URI_REPO_GIT_A_INITIALISER’’ $REPO_LOCAL_TEMP

rm -rf $REPERTOIRE_CODE_A_VERSIONNER/.git
cp -Rf $REPERTOIRE_CODE_A_VERSIONNER/*  $REPO_LOCAL_TEMP
echo ‘’ VERIFICATION contenu [$REPO_LOCAL_TEMP] : ’’
ls -all $REPO_LOCAL_TEMP

# Lorsque les tests sont validés jusque là, on peut passer aux tests git push.








 - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - 
# ./boot/poste-devops/operations.sh
export MAISON_POSTE_DEVOPS=$HOME/carrefour/usine/poste-devops
export ECLIPSE_RELEASE_CODENAME=photon
export LINUX_HW_SUPPORTED_ARCHITECTURE=x86_64
export NOM_ARCHIVE_ECLIPSE=eclipse-java-$ECLIPSE_RELEASE_CODENAME-R-linux-gtk-x86_64.tar.gz
export URI_DOWNLOAD_ECLIPSE=http://www.eclipse.org/downloads/download.php?file=/technology/epp/downloads/release/$ECLIPSE_RELEASE_CODENAME/R/eclipse-java-$ECLIPSE_RELEASE_CODENAME-R-linux-gtk-x86_64.tar.gz
export URI_DOWNLOAD_ECLIPSE=http://www.eclipse.org/downloads/download.php?file=/technology/epp/downloads/release/photon/R/eclipse-java-photon-R-linux-gtk-x86_64.tar.gz&mirror_id=17

cd $MAISON_POSTE_DEVOPS

# ATTENTION !!! ==>> dans l’option ci-dessous, c’est la lettre O majuscule
# wget $URI_DOWNLOAD_ECLIPSE -O $NOM_ARCHIVE_ECLIPSE
# que ce soit avec wget ou curl, le téléchargement corrompt l’archive, j’ai donc été
# obligé de la télécharger à la main, pour la versionner dans ce repo.

# On va dézipper la distribution d’eclipse dans  $MAISON_POSTE_DEVOPS/ide/photon/1
mkdir -p $MAISON_POSTE_DEVOPS/ide/eclipses/photon/1

gunzip -c $MAISON_POSTE_DEVOPS/eclipse-java-photon-R-linux-gtk-x86_64.tar.gz | tar xopf -
cp -Rf $MAISON_POSTE_DEVOPS/eclipse/* $MAISON_POSTE_DEVOPS/ide/eclipses/photon/1/ 

# TODO : On créée le raccourci Bureau avec l’option eclipse -data ‘’$MAISON_POSTE_DEVOPS/ide/espaces-travail/1’’ 


# On va cloner le code source de chaque projet de travail, dans $MAISON_POSTE_DEVOPS/ide/espaces-travail/1
# Ainsi, dans $MAISON_POSTE_DEVOPS/ide/espaces-travail/1, on trouvera un workspace eclipse dans lequel on
# va importer (Import Existing Maven Project) tous les projets souhaités : donc il faut d’abord maveniser les
# projets, avant de provisionner l’IDE
