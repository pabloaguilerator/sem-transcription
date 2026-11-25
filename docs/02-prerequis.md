# 2. Prérequis matériels et logiciels

Cette section présente le matériel et les logiciels nécessaires pour installer et utiliser l'application SEM Transcription.

La configuration décrite correspond à l'environnement de développement utilisé au SEM.

## 2.1. Matériel nécessaire

### Raspberry Pi 5

L'application est développée et exécutée sur un Raspberry Pi 5.

Configuration de référence :

| Composant | Caractéristiques |
|---|---|
| Ordinateur | Raspberry Pi 5 Model B |
| Mémoire vive (RAM) | 8 Go |
| Stockage | Carte microSD de 32 Go |
| Architecture | ARM64 (64 bits) |
| Alimentation | Alimentation adaptée au Raspberry Pi 5 |
| Périphériques | Écran, clavier et souris pour l'utilisation graphique |

Un espace de stockage supplémentaire peut être utile pour conserver les enregistrements audio, les transcriptions et les modèles d'intelligence artificielle.

### Matériel audio

Pour enregistrer des réunions, le dispositif peut être utilisé avec les microphones sans fil RØDE Wireless GO II.

Le système comprend :

- Deux émetteurs sans fil (TX).
- Un récepteur (RX).
- Un câble USB permettant de connecter le récepteur au Raspberry Pi.

Le récepteur transmet le signal audio au Raspberry Pi pour son enregistrement.

**Remarque :** l'utilisation des microphones RØDE n'est pas obligatoire. Il est également possible d'enregistrer une réunion avec un autre appareil, comme un smartphone, un ordinateur ou un enregistreur numérique, puis d'importer le fichier audio dans SEM Transcription pour le transcrire. Il suffit de disposer d'un fichier audio dans un format compatible avec l'application.

## 2.2. Système d'exploitation

L'environnement de référence utilise :

| Élément | Version |
|---|---|
| Système d'exploitation | Raspberry Pi OS 64 bits (basé sur Debian GNU/Linux 13 Trixie) |
| Architecture | ARM64 |
| Noyau Linux | 6.12.47+rpt-rpi-2712 |
| Interface | Environnement graphique |

Les versions indiquées correspondent à la configuration de référence. Une version plus récente du noyau Linux peut également être utilisée.

Le système doit permettre l'utilisation d'un terminal et l'installation de logiciels.

Une connexion Internet est nécessaire lors de l'installation initiale pour télécharger les programmes, les dépendances et les modèles d'intelligence artificielle.

Une fois les composants et les modèles installés, les traitements de transcription, de diarisation et d'analyse sont conçus pour être effectués localement, sans connexion Internet.

## 2.3. Logiciels nécessaires

L'application repose sur plusieurs outils.

| Logiciel | Version de référence | Utilisation |
|---|---|---|
| Python | 3.13.5 | Exécution de l'application |
| Git | 2.47.3 | Téléchargement des projets |
| CMake | 3.31.6 | Compilation des moteurs IA |
| GCC / G++ | 14.2.0 | Compilation des programmes |
| FFmpeg | 7.1.5 | Conversion et traitement audio |
| Whisper.cpp | Commit `284029c` | Transcription audio |
| llama.cpp | Commit `af6b524` | Analyse avec l'IA |
| ALSA Utils | Paquet système | Capture audio |
| libsndfile | Paquet système | Lecture des fichiers audio |
| OpenSSL | Paquet système | Connexions sécurisées |
| DejaVu Fonts | Paquet système | Exportation PDF |

Les versions indiquées correspondent à la configuration de référence. Les références Git complètes de Whisper.cpp et de llama.cpp sont précisées dans les sections 3.3 et 3.5 du [chapitre 3 — Installation](03-installation.md) afin de faciliter la reproduction de l'installation.

## 2.4. Bibliothèques Python

L'interface graphique et les fonctionnalités de l'application utilisent plusieurs bibliothèques Python.

| Bibliothèque | Version de référence | Fonction |
|---|---|---|
| Tkinter | 3.13.5 (paquet système) | Interface graphique |
| python-docx | 1.1.2 | Exportation Word |
| ReportLab | 4.3.1 | Exportation PDF |
| SoundFile | 0.14.0 | Lecture et traitement audio |
| PyTorch | 2.9.1 | Calculs nécessaires à Pyannote |
| TorchAudio | 2.9.1 | Traitement audio |
| TorchCodec | 0.17.0 | Décodage multimédia |
| pyannote.audio | 4.0.7 | Diarisation des interlocuteurs |
| huggingface_hub | 2.3.0 | Téléchargement et gestion des modèles |

Les bibliothèques sont réparties dans deux environnements Python distincts :

- **Python du système :** Tkinter, python-docx et ReportLab.
- **Environnement virtuel `venv_pyannote` :** SoundFile, PyTorch, TorchAudio, TorchCodec, pyannote.audio et huggingface_hub.

L'installation et la configuration de l'environnement virtuel dédié à la diarisation sont décrites dans la section 3.7 du [chapitre 3 — Installation](03-installation.md).

## 2.5. Modèles d'intelligence artificielle

Trois modèles principaux sont utilisés.

### Whisper Medium Q5

- **Fonction :** transcription des enregistrements audio.
- **Fichier :** `ggml-medium-q5_0.bin`.
- **Taille :** environ 539 Mo.
- **Moteur :** Whisper.cpp.

Ce modèle est utilisé par défaut dans l'application.

### Qwen3-4B Q4_K_M

- **Fonction :** analyse des transcriptions, génération de résumés et questions-réponses.
- **Modèle :** `Qwen/Qwen3-4B-GGUF:Q4_K_M`.
- **Taille :** environ 2,5 Go.
- **Moteur :** llama.cpp.

Le modèle Qwen3 est chargé par un serveur local qui doit être démarré avant l'utilisation des fonctionnalités d'analyse.

### Pyannote — Speaker Diarization Community-1

- **Fonction :** identification et distinction des interlocuteurs dans un enregistrement audio.
- **Modèle :** `pyannote/speaker-diarization-community-1`.
- **Bibliothèque :** Pyannote.audio.
- **Exécution :** locale, sur CPU.

Le téléchargement initial du modèle nécessite un compte Hugging Face, l'acceptation des conditions d'utilisation du modèle et un jeton d'accès disposant des droits de lecture.

Une fois téléchargé, le modèle est conservé dans le cache local et peut être utilisé sans connexion Internet.

La procédure de téléchargement est détaillée dans la section 3.7 du [chapitre 3 — Installation](03-installation.md).

## 2.6. Espace de stockage

L'installation nécessite de l'espace pour :

- Le système d'exploitation.
- Les logiciels et bibliothèques.
- L'environnement virtuel dédié à la diarisation.
- Les modèles Whisper, Qwen3 et Pyannote.
- Les fichiers audio et les transcriptions.
- Les réunions enregistrées et les résultats des analyses.
- Les éventuels documents exportés.

La configuration de référence utilise une carte microSD de 32 Go. Pour une nouvelle installation, une capacité de **64 Go ou plus est recommandée** afin de disposer de davantage d'espace pour les enregistrements et les modèles.

## 2.7. Avant de commencer

Avant de passer à l'installation, vérifier que :

- Le Raspberry Pi 5 démarre correctement.
- Le système d'exploitation 64 bits est installé.
- Une connexion Internet est disponible.
- Le terminal est accessible.
- L'utilisateur dispose des autorisations nécessaires pour installer des logiciels.
- L'espace de stockage est suffisant.
- Un compte Hugging Face est disponible ou peut être créé pour télécharger le modèle de diarisation.

La section suivante présente l'installation des composants étape par étape.

---

[← Chapitre 1 — Présentation du projet](01-presentation.md) | [Chapitre 3 — Installation →](03-installation.md)
