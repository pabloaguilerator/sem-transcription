# 4. Configuration de l'application

Cette section présente les paramètres nécessaires au fonctionnement de SEM Transcription après son installation.

Elle explique comment vérifier la configuration des moteurs d'intelligence artificielle, des périphériques audio et des dossiers utilisés par l'application.

## 4.1. Configuration de Whisper.cpp

SEM Transcription utilise Whisper.cpp pour effectuer la transcription locale des fichiers audio.

### Paramètres de référence

| Paramètre | Configuration |
|---|---|
| Moteur de transcription | Whisper.cpp |
| Modèle | Whisper Medium |
| Quantification | Q5_0 |
| Langue de transcription | Français |
| Exécution | Locale, sur CPU |

### Emplacement de l'exécutable

```text
~/projet_transcription/whisper.cpp/build/bin/whisper-cli
```

### Emplacement du modèle

```text
~/projet_transcription/whisper.cpp/models/ggml-medium-q5_0.bin
```

Ces chemins sont définis dans le fichier `app.py` de SEM Transcription.

Si les instructions d'installation du [chapitre 3](03-installation.md) ont été respectées, aucune modification manuelle de la configuration n'est nécessaire.

L'application utilise automatiquement l'exécutable et le modèle lors du lancement d'une transcription.

## 4.2. Configuration du serveur Qwen3

SEM Transcription communique avec le modèle Qwen3 par l'intermédiaire d'un serveur local exécuté avec llama.cpp.

### Paramètres de référence

| Paramètre | Configuration |
|---|---|
| Moteur d'inférence | llama.cpp |
| Modèle | Qwen3-4B |
| Quantification | Q4_K_M |
| Adresse locale | 127.0.0.1 |
| Port | 8080 |
| Fenêtre de contexte | 4096 tokens |
| Nombre de threads | 4 |
| Mode de raisonnement | Désactivé |
| Exécution | Locale, sur CPU |

### Communication avec l'application

SEM Transcription envoie les requêtes d'analyse au serveur local via l'adresse suivante :

```text
http://127.0.0.1:8080/v1/chat/completions
```

Cette configuration est intégrée au code source de l'application. Aucune modification manuelle n'est nécessaire si les instructions d'installation ont été respectées.

### Fonctionnement du serveur

Le serveur doit être démarré conformément aux instructions de la section 3.6 du [chapitre 3 — Installation](03-installation.md) et rester actif pendant l'utilisation des fonctionnalités suivantes :

- Analyse des transcriptions.
- Génération de résumés.
- Questions-réponses sur le contenu des réunions.

Le serveur n'est pas nécessaire pour la transcription audio ni pour la diarisation.

Les requêtes d'analyse sont traitées localement sur le Raspberry Pi, sans transmission du contenu des réunions à un service d'intelligence artificielle externe.

## 4.3. Configuration des microphones RØDE

SEM Transcription peut être utilisé avec le système de microphones sans fil RØDE Wireless GO II pour enregistrer les échanges lors de réunions.

### Connexion des microphones

1. Connecter le récepteur RØDE Wireless GO II au Raspberry Pi à l'aide d'un câble USB.
2. Allumer le récepteur et les émetteurs.
3. Vérifier que les émetteurs sont correctement associés au récepteur.

### Détection du périphérique audio

Ouvrir un terminal et exécuter :

```bash
arecord -l
```

Le récepteur RØDE doit apparaître dans la liste des périphériques de capture audio.

Le numéro de la carte audio peut varier selon les périphériques connectés au Raspberry Pi.

### Paramètres audio de référence

| Paramètre | Configuration |
|---|---|
| Périphérique | RØDE Wireless GO II |
| Connexion | USB |
| Fréquence d'échantillonnage | 48 000 Hz |
| Format audio | S24_3LE (24 bits) |
| Nombre de canaux | 2 (stéréo) |

**Important :** utiliser le format S24_3LE. Lors des essais réalisés avec cette configuration matérielle, l'utilisation du format S16_LE a entraîné la présence de bruit blanc dans les enregistrements.

### Enregistrement audio

Adapter les numéros de carte et de périphérique aux informations obtenues avec la commande `arecord -l`.

Par exemple, pour la carte 2 et le périphérique 0 :

```bash
arecord -D hw:2,0 -f S24_3LE -r 48000 -c 2 ~/enregistrement.wav
```

Pour arrêter l'enregistrement, utiliser la combinaison de touches `Ctrl + C`.

Le fichier audio obtenu peut ensuite être importé dans SEM Transcription.

### Utilisation d'autres dispositifs d'enregistrement

Les microphones RØDE ne sont pas indispensables au fonctionnement de SEM Transcription.

Il est également possible d'enregistrer une réunion à l'aide d'un smartphone, d'un dictaphone ou d'un autre dispositif d'enregistrement, puis de transférer le fichier audio sur le Raspberry Pi afin de le transcrire.

## 4.4. Configuration de la diarisation

SEM Transcription utilise Pyannote.audio pour identifier les différents interlocuteurs présents dans un enregistrement et leur associer les segments de transcription correspondants.

### Paramètres de référence

| Paramètre | Configuration |
|---|---|
| Bibliothèque | Pyannote.audio 4.0.7 |
| Modèle | speaker-diarization-community-1 |
| PyTorch | 2.9.1 |
| Lecture des fichiers audio | SoundFile 0.14.0 |
| Exécution | Locale, sur CPU |

### Environnement virtuel

L'environnement Python dédié à la diarisation se trouve dans le dossier suivant :

```text
~/projet_transcription/venv_pyannote/
```

SEM Transcription utilise automatiquement l'interpréteur Python de cet environnement :

```text
~/projet_transcription/venv_pyannote/bin/python
```

### Fonctionnement de la diarisation

La diarisation repose sur deux modules intégrés à l'application :

- `diarisation_auto.py` : identification des interlocuteurs et de leurs intervalles de prise de parole.
- `fusion_locuteurs.py` : association des segments de transcription aux interlocuteurs identifiés.

Les interlocuteurs sont initialement identifiés par des identifiants génériques, tels que `SPEAKER_00` et `SPEAKER_01`. L'utilisateur peut ensuite les renommer dans l'interface de SEM Transcription.

### Fonctionnement hors ligne

Une fois le modèle téléchargé conformément aux instructions de la section 3.7 du [chapitre 3 — Installation](03-installation.md), la diarisation utilise les fichiers conservés dans le cache local de Hugging Face.

Aucune activation manuelle de l'environnement virtuel ni aucun démarrage du serveur Qwen3 ne sont nécessaires.

En cas d'échec de la diarisation, la transcription reste disponible, sans identification automatique des interlocuteurs.

## 4.5. Organisation des fichiers et des dossiers

L'ensemble des composants de SEM Transcription est organisé dans un dossier de travail principal situé dans le répertoire personnel de l'utilisateur.

### Structure des dossiers

```text
~/projet_transcription/
├── whisper.cpp/
├── llama.cpp/
├── venv_pyannote/
├── sem_app/
├── reunions/
└── analyses_partielles/
```

### Description des dossiers

| Dossier | Contenu |
|---|---|
| `whisper.cpp/` | Moteur de transcription et modèle Whisper. |
| `llama.cpp/` | Moteur d'inférence et serveur local Qwen3. |
| `venv_pyannote/` | Environnement Python dédié à la diarisation. |
| `sem_app/` | Fichiers Python de l'application SEM Transcription. |
| `reunions/` | Réunions enregistrées, transcriptions et analyses associées. |
| `analyses_partielles/` | Résultats intermédiaires générés lors de certaines analyses. |

Les dossiers `reunions` et `analyses_partielles` sont créés automatiquement par l'application lorsque cela est nécessaire.

### Stockage des modèles de langage et de diarisation

Les modèles Qwen3 et Pyannote téléchargés depuis Hugging Face sont conservés dans le cache local suivant :

```text
~/.cache/huggingface/hub/
```

Il n'est pas nécessaire de déplacer ces fichiers.

### Exportation des documents

Les documents exportés depuis SEM Transcription sont enregistrés dans le dossier suivant :

```text
~/Downloads/
```

Ce dossier est créé automatiquement par l'application s'il n'existe pas.

### Conservation de la structure

Il est recommandé de conserver cette organisation afin de garantir le bon fonctionnement de l'application et la communication entre ses différents composants.

Les chemins d'accès sont définis à partir du répertoire personnel de l'utilisateur. Il n'est donc pas nécessaire d'utiliser un nom d'utilisateur spécifique lors de l'installation.

---

[← Chapitre 3 — Installation de l'application](03-installation.md) | [Chapitre 5 — Guide d'utilisation →](05-utilisation.md)
