# 1. Présentation du projet

## 1.1. Contexte et objectifs

Le projet **SEM Transcription** est une application développée au sein du Service écoles-médias (SEM) du Département de l'instruction publique de Genève.

Son objectif est de faciliter la **transcription et l'analyse de réunions** à l'aide d'outils d'intelligence artificielle exécutés localement sur un Raspberry Pi 5.

L'application permet de transformer un enregistrement audio en texte, de corriger la transcription obtenue et d'exploiter son contenu pour produire des résumés ou rechercher des informations.

Le projet privilégie une approche locale : les traitements sont conçus pour être réalisés directement sur le Raspberry Pi, sans nécessiter l'envoi des enregistrements à un service de transcription ou d'analyse en ligne.

## 1.2. Fonctionnalités principales

L'application regroupe plusieurs fonctionnalités :

- **Transcription audio :** conversion d'un fichier audio en texte à l'aide de Whisper.
- **Diarisation :** identification automatique des différents interlocuteurs, association des segments de transcription à chacun d'entre eux et possibilité de les renommer dans l'interface.
- **Correction de la transcription :** consultation et modification du texte directement dans l'interface.
- **Gestion des réunions :** enregistrement et consultation des transcriptions associées aux réunions.
- **Analyse avec l'IA :** génération de résumés structurés à partir des transcriptions.
- **Questions-réponses :** possibilité d'interroger le contenu d'une réunion et de retrouver les extraits utilisés pour produire une réponse.
- **Exportation :** téléchargement des transcriptions et production de documents aux formats TXT, Word et PDF.

Certaines fonctionnalités avancées nécessitent le démarrage préalable du serveur d'intelligence artificielle.

## 1.3. Architecture technique

L'application repose sur quatre composants principaux.

### 1.3.1. Raspberry Pi 5

Le Raspberry Pi 5 constitue l'unité centrale du dispositif. Il héberge l'application, les modèles d'intelligence artificielle et les fichiers nécessaires à leur fonctionnement.

### 1.3.2. Whisper.cpp — Transcription audio

Whisper.cpp est utilisé pour convertir les enregistrements audio en texte.

**Modèle utilisé :** Whisper Medium Q5 (`ggml-medium-q5_0.bin`).

Ce modèle permet notamment de traiter des enregistrements en français.

### 1.3.3. llama.cpp et Qwen3 — Analyse des transcriptions

L'analyse des transcriptions repose sur llama.cpp, qui permet d'exécuter localement un modèle de langage.

**Modèle utilisé :** Qwen3-4B, quantification Q4_K_M.

Ce composant intervient dans la génération des résumés et dans les fonctionnalités de questions-réponses.

La communication entre l'application et le modèle Qwen3 s'effectue à travers une API locale, accessible sur le port 8080. Le serveur llama.cpp doit être démarré et le modèle entièrement chargé avant d'utiliser les fonctionnalités d'analyse avec l'IA.

### 1.3.4. Pyannote.audio — Diarisation

Pyannote.audio est utilisé pour identifier et distinguer les différents interlocuteurs présents dans un enregistrement audio.

**Modèle utilisé :** `pyannote/speaker-diarization-community-1`.

Ce composant permet d'associer les segments de transcription aux différents interlocuteurs et de faciliter ainsi la lecture et l'analyse des échanges.

La diarisation est réalisée localement sur le Raspberry Pi, indépendamment du serveur Qwen3.

## 1.4. Fonctionnement général

Le fonctionnement de l'application suit les étapes suivantes :

1. **Importer un enregistrement audio** depuis l'interface de l'application.
2. **Lancer la transcription** pour convertir automatiquement l'audio en texte.
3. **Identifier les interlocuteurs** grâce à la diarisation automatique.
4. **Relire et corriger** la transcription obtenue et, si nécessaire, renommer les interlocuteurs.
5. **Enregistrer la réunion** afin de conserver les informations associées.
6. **Analyser le contenu** à l'aide de l'intelligence artificielle pour générer un résumé ou poser des questions.
7. **Exporter les résultats** dans le format souhaité.

La transcription, la diarisation et l'analyse avec l'IA constituent trois traitements distincts. Il est donc possible d'utiliser la transcription et la diarisation sans démarrer le modèle de langage Qwen3.

## 1.5. Limites matérielles et performances

L'application SEM Transcription est conçue pour fonctionner localement sur un Raspberry Pi 5. Bien que ce dispositif permette d'exécuter plusieurs modèles d'intelligence artificielle, ses ressources matérielles restent limitées par rapport à celles d'un ordinateur plus puissant.

Dans la configuration actuelle, il est recommandé de privilégier des enregistrements audio d'une durée maximale d'environ 20 minutes afin de conserver des conditions de traitement satisfaisantes.

Au-delà de cette durée, les temps de traitement peuvent augmenter considérablement et certaines fonctionnalités d'analyse avec l'IA peuvent atteindre leurs limites techniques.

Pour les réunions plus longues, il est recommandé de diviser les enregistrements en plusieurs fichiers audio d'une durée maximale d'environ 20 minutes.

Les performances dépendent notamment :

- De la durée et de la qualité de l'enregistrement audio.
- Du nombre d'interlocuteurs.
- Des ressources matérielles disponibles.
- De la complexité des analyses demandées.
- De la fenêtre de contexte du modèle de langage utilisé.

Le projet vise ainsi à proposer une solution locale, accessible et autonome, tout en tenant compte des contraintes matérielles du Raspberry Pi 5.

## 1.6. Organisation de cette documentation

Cette documentation explique comment installer, configurer et utiliser l'application SEM Transcription sur un Raspberry Pi 5.

Elle est organisée en plusieurs parties :

1. **Présentation du projet** — objectifs, fonctionnalités et architecture.
2. **Prérequis** — matériel et logiciels nécessaires.
3. **Installation** — préparation du Raspberry Pi et installation des composants.
4. **Configuration** — modèles d'IA et périphériques audio.
5. **Guide d'utilisation** — fonctionnement de l'application étape par étape.
6. **Dépannage** — problèmes fréquents et solutions.

L'objectif est de permettre à chaque utilisateur de reproduire l'installation et d'utiliser l'application de manière autonome.
