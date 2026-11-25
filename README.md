# SEM Transcription

Application locale de transcription, de diarisation et d'analyse de réunions sur Raspberry Pi 5.

## 1. Présentation du projet

SEM Transcription est une application développée dans le cadre d'un projet du Service écoles-médias (SEM) du canton de Genève.

Elle vise à faciliter la transcription et l'analyse d'enregistrements audio à l'aide de modèles d'intelligence artificielle exécutés localement sur un Raspberry Pi 5.

L'application dispose d'une interface graphique permettant d'utiliser ces fonctionnalités sans avoir à manipuler directement les outils en ligne de commande.

## 2. Fonctionnalités principales

- Importation de fichiers audio.
- Transcription automatique en français.
- Identification et distinction des interlocuteurs (diarisation).
- Renommage des interlocuteurs.
- Écoute de fragments audio.
- Génération de synthèses et de comptes rendus de réunions.
- Assistant IA permettant d'interroger le contenu des transcriptions.
- Sauvegarde et consultation des réunions.
- Exportation des résultats aux formats TXT, PDF et Word.

## 3. Architecture technique

L'application repose sur plusieurs composants complémentaires :

| Composant | Technologie | Fonction |
|---|---|---|
| Interface graphique | Python / Tkinter | Interaction avec l'utilisateur |
| Transcription | Whisper.cpp — Medium Q5_0 | Conversion de l'audio en texte |
| Diarisation | Pyannote.audio — Community-1 | Identification des interlocuteurs |
| Analyse IA | Llama.cpp — Qwen3-4B Q4_K_M | Synthèse et analyse des transcriptions |
| Traitement audio | FFmpeg | Conversion et lecture des fichiers audio |
| Exportation | ReportLab / Python-docx | Génération de documents PDF et Word |

Les modèles d'intelligence artificielle sont exécutés localement sur le processeur du Raspberry Pi.

## 4. Prérequis matériels et logiciels

### Matériel

- Raspberry Pi 5 avec au moins 8 Go de RAM.
- Carte microSD de 32 Go minimum (64 Go ou plus recommandés).
- Alimentation adaptée au Raspberry Pi 5.
- Système de refroidissement adapté.
- Écran, clavier et souris.
- Connexion Internet pour l'installation initiale.

Un système de microphones sans fil, tel que le RØDE Wireless GO II, peut être utilisé pour enregistrer les réunions.

Il n'est toutefois pas nécessaire pour transcrire des fichiers audio enregistrés avec un autre dispositif, notamment un smartphone.

### Logiciels

- Raspberry Pi OS 64 bits (Debian 13).
- Python 3.
- Whisper.cpp.
- Llama.cpp.
- Pyannote.audio.
- FFmpeg.

Les versions de référence et les procédures d'installation sont détaillées dans la documentation technique.

## 5. Installation et documentation

La documentation du projet est organisée en six chapitres :

1. Présentation générale du projet.
2. Prérequis matériels et logiciels.
3. Installation de l'environnement technique.
4. Configuration de l'application.
5. Guide d'utilisation de SEM Transcription.
6. Maintenance et résolution des problèmes.

Ces chapitres seront disponibles dans le dossier `docs` du dépôt.

## 6. Confidentialité et traitement local

SEM Transcription repose sur une architecture locale permettant de traiter les enregistrements audio sans les transmettre à un service d'intelligence artificielle distant.

Une connexion Internet est nécessaire lors de l'installation initiale, notamment pour télécharger les dépendances et les modèles.

Les enregistrements, transcriptions et documents générés sont conservés localement sur le dispositif.

L'application ne dispose pas de son propre système de chiffrement des fichiers. Il appartient donc à l'utilisateur de protéger l'accès au dispositif et aux données enregistrées.

## 7. Limites et précautions d'utilisation

Les résultats produits par les modèles d'intelligence artificielle doivent être vérifiés par l'utilisateur.

La qualité des transcriptions et de la diarisation dépend notamment de la qualité de l'enregistrement audio, du bruit ambiant et des conditions de prise de parole.

Les performances et les temps de traitement dépendent des ressources matérielles disponibles et de la durée des enregistrements.

Les capacités d'analyse des réunions longues sont également limitées par les ressources matérielles et la fenêtre de contexte du modèle utilisé.

## 8. Téléchargement

Le code source de l'application est disponible dans le dossier `sem_app`.

Une archive ZIP contenant les fichiers nécessaires à l'installation de l'application sera disponible dans la section **Releases** du dépôt.

## 9. État du projet

Version 1.0.0 — En cours de validation finale.

## 10. Remerciements

Ce projet s'appuie sur plusieurs technologies et projets open source :

- [Whisper.cpp](https://github.com/ggml-org/whisper.cpp)
- [Llama.cpp](https://github.com/ggml-org/llama.cpp)
- [Pyannote.audio](https://github.com/pyannote/pyannote-audio)
- [Qwen](https://github.com/QwenLM)
- [FFmpeg](https://ffmpeg.org/)
