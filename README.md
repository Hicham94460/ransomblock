# ransomblock



Outil de preuve de concept qui automatise la création de fichiers PE, utilisé pour exploiter le pré-chiffrement des ransomwares.

Language : C
SHA256 : 647494bda466e645768d6f7d1cd051097aee319f88018d1a80547d8d538c98db

PoC vidéo (ancienne v2) :
https://www.youtube.com/watch?v=_Ho0bpeJWqI

Les fichiers PE générés par RansomLord sont enregistrés sur le disque dans les répertoires x32 ou x64 à partir desquels le programme est exécuté.

L’objectif est d’exploiter les vulnérabilités inhérentes à certaines souches de ransomwares en déployant des exploits pour défendre le réseau !

Les DLL peuvent également fournir une couverture supplémentaire contre les malwares génériques et voleurs d'informations.
RansomLord et ses DLL exportées ne sont PAS malveillantes, voir l'indicateur -s pour les informations de sécurité.

[Histoire de Malvuln : https://github.com/malvuln/RansomLord]
En mai 2022, j'ai divulgué publiquement une nouvelle stratégie pour vaincre avec succès les ransomwares. Utilisation d'une technique d'attaquant bien connue (détournement de DLL) pour mettre fin au pré-chiffrement des logiciels malveillants. Le premier malware exploité avec succès était celui du groupe Lockbit MVID-2022-0572. Suivis par Conti, REvil, BlackBasta et CryptoLocker, prouvant que beaucoup sont vulnérables. RansomLord v1 intercepte et met fin aux logiciels malveillants testés par 33 groupes de menaces différents. Clop, Play, Royal, BlackCat (alphv), Yanluowang, DarkSide, Nokoyawa etc...

[Mise à jour v3.1]
RansomLord intercepte et met désormais fin aux ransomwares testés par 49 groupes de menaces différents.
Ajout de StopCrypt, RisePro, RuRansom, MoneyMessage, CryptoFortress et Onyx à la liste des victimes.
La fonctionnalité de journal des événements Windows -e flag tentera de consigner le hachage SHA256 du ransomware.
Ajout de l'indicateur -r pour générer une règle Sigma permettant de détecter l'activité de RansomLord à l'aide du journal des événements Windows.

[Génération d'exploits]
L'indicateur -g répertorie les ransomwares à exploiter en fonction du groupe de ransomwares sélectionné. Il générera une DLL 32 ou 64 bits nommée de manière appropriée en fonction de la famille sélectionnée.

[Stratégie]
La logique du fichier d'exploitation DLL créé est simple, nous vérifions si le répertoire actuel est C:\Windows\System32. Sinon, nous récupérons notre propre ID de processus (PID) et mettons fin à nous-mêmes ainsi qu'au pré-cryptage des logiciels malveillants, car nous contrôlons désormais le flux d'exécution du code.

[Journal des événements IOC]
L'indicateur -e configure une source d'événements Windows personnalisée dans le registre Windows. Les événements sont écrits dans « Journaux Windows\Application » en tant qu'ID d'événement « RansomLord ». 1 Le nom du logiciel malveillant et le chemin complet du processus sont également inclus dans les informations générales. La fonctionnalité de journal des événements Windows -e flag enregistrera désormais le hachage SHA256 du ransomware.

[Carte DLL]
L'indicateur -m affiche les groupes de ransomwares, les DLL requises et l'architecture x32 ou 64 bits.

[Salle des Trophées]
L'indicateur -t répertorie les anciens avis de ransomware de 2022 avec l'identifiant de vulnérabilité du logiciel malveillant.

[Avertissement]
Les familles de ransomwares et/ou les échantillons répertoriés ne garantissent PAS un résultat positif. De nombreux facteurs peuvent ruiner le succès : différentes variantes, versions du système d'exploitation, emplacement des logiciels malveillants, etc. Par conséquent, procédez avec prudence car le kilométrage peut varier, bonne chance.

[Environnement de test]
Les tests ont été effectués sur une machine virtuelle Windows 10 et un client léger avec système d'exploitation intégré Win-7.

[À propos]
Le drapeau -a informations générales, contact et clause de non-responsabilité. En utilisant ce programme et/ou ses fichiers DLL, vous acceptez tous les risques et l'intégralité de la clause de non-responsabilité. Par John Page (alias Malvuln) Copyright (c) 2023


Références :
https://web.archive.org/web/20220601204439/https://www.bleepingcomputer.com/news/security/conti-revil-lockbit-ransomware-bugs-exploited-to-block-encryption/

https : //web.archive.org/web/20220504180432/https://www.securityweek.com/vulnerabilities-allow-hijacking-most-ransomware-prevent-file-encryption/
