# 3. Installation de l'application

Cette section présente la procédure d'installation de **SEM Transcription** sur un Raspberry Pi 5.

L'installation est organisée en huit étapes successives. Chaque étape présente les instructions nécessaires à l'installation des différents composants et à la vérification de leur fonctionnement.

## 3.1. Préparation du Raspberry Pi 5

### Objectif

Installer un système d'exploitation compatible et préparer le Raspberry Pi pour les étapes suivantes.

### Procédure

**1. Installer le système d'exploitation**

À partir d'un ordinateur :

1. Ouvrir Raspberry Pi Imager.
2. Sélectionner Raspberry Pi 5.
3. Choisir Raspberry Pi OS (64-bit) avec environnement graphique.
4. Sélectionner la carte microSD.
5. Configurer les paramètres de connexion et le compte utilisateur.
6. Installer le système et démarrer le Raspberry Pi.

La configuration de référence utilise Debian GNU/Linux 13 (Trixie).

**2. Mettre à jour le système**

Dans le terminal :

```bash
sudo apt update
sudo apt full-upgrade -y
sudo reboot
```

### Vérification

Après le redémarrage :

```bash
cat /etc/os-release
uname -m
free -h
df -h /
```

Vérifier que l'architecture est `aarch64` et que l'espace disponible est suffisant.

## 3.2. Installation des logiciels nécessaires

Avant d'installer les différents composants de SEM Transcription, il est nécessaire d'installer les outils de compilation, les bibliothèques Python et les logiciels de traitement audio.

### Installation

Ouvrir un terminal et exécuter les commandes suivantes :

```bash
sudo apt update

sudo apt install -y git cmake build-essential \
  python3 python3-pip python3-venv python3-tk \
  python3-docx python3-reportlab \
  ffmpeg libsndfile1 libssl-dev \
  curl alsa-utils fonts-dejavu-core
```

### Vérification

Vérifier que les principaux logiciels sont correctement installés :

```bash
git --version
cmake --version
g++ --version
python3 --version
ffmpeg -version
```

Vérifier également la disponibilité des bibliothèques Python nécessaires à l'interface graphique et à l'exportation des documents :

```bash
python3 -c "import tkinter, docx, reportlab; print('OK')"
```

Si le terminal affiche `OK`, les trois bibliothèques Python sont disponibles.

Les dépendances spécifiques à la diarisation seront installées séparément dans un environnement virtuel Python, conformément à la section 3.7.

## 3.3. Installation de Whisper.cpp

Whisper.cpp est le moteur de transcription utilisé par SEM Transcription. Il permet d'exécuter localement les modèles Whisper sur le Raspberry Pi, sans recourir à un service de transcription externe.

### Création du dossier de travail

```bash
mkdir -p ~/projet_transcription
cd ~/projet_transcription
```

### Téléchargement de Whisper.cpp

```bash
git clone https://github.com/ggml-org/whisper.cpp.git
cd whisper.cpp
```

Afin de garantir la compatibilité avec SEM Transcription, utiliser la version de référence suivante :

```bash
git checkout 284029c07012d2000941a08d38a31ffb71f4734e
```

### Compilation

```bash
cmake -B build
cmake --build build -j4
```

La compilation peut prendre plusieurs minutes.

### Vérification

Vérifier que l'exécutable nécessaire à la transcription a bien été généré :

```bash
ls -lh build/bin/whisper-cli
```

Vérifier ensuite son fonctionnement :

```bash
./build/bin/whisper-cli --help
```

Si les options de commande de Whisper.cpp s'affichent dans le terminal, l'exécutable est opérationnel.

L'étape suivante consiste à télécharger le modèle de transcription utilisé par SEM Transcription.

## 3.4. Téléchargement du modèle Whisper

SEM Transcription utilise le modèle Whisper Medium quantifié en Q5_0 (`ggml-medium-q5_0.bin`).

Ce modèle multilingue offre un compromis entre la qualité de transcription, la vitesse de traitement et les ressources matérielles disponibles sur le Raspberry Pi 5.

### Téléchargement du modèle

Depuis le terminal, accéder au dossier des modèles de Whisper.cpp :

```bash
cd ~/projet_transcription/whisper.cpp/models
```

Télécharger le modèle :

```bash
./download-ggml-model.sh medium-q5_0
```

Une connexion Internet est nécessaire pour effectuer ce téléchargement.

### Vérification

Vérifier que le modèle a bien été téléchargé :

```bash
ls -lh ggml-medium-q5_0.bin
```

Le fichier doit avoir une taille d'environ 515 Mio (540 Mo).

### Emplacement du modèle

Le modèle doit être conservé à l'emplacement suivant :

```text
~/projet_transcription/whisper.cpp/models/ggml-medium-q5_0.bin
```

SEM Transcription utilise automatiquement ce fichier lors de la transcription.

Une fois le modèle téléchargé, les transcriptions peuvent être réalisées localement, sans connexion Internet.

## 3.5. Installation de llama.cpp

Llama.cpp est le moteur d'inférence utilisé par SEM Transcription pour exécuter localement un modèle de langage. Il permet notamment de générer des résumés, d'analyser les transcriptions et de répondre à des questions sur leur contenu.

### Téléchargement de llama.cpp

Depuis le terminal, exécuter les commandes suivantes :

```bash
cd ~/projet_transcription
git clone https://github.com/ggml-org/llama.cpp.git
cd llama.cpp
```

Utiliser la version de référence suivante :

```bash
git checkout af6b5243d6871923dfecbe06814bd566d9dc383b
```

### Compilation

Configurer la compilation avec la prise en charge des connexions HTTPS :

```bash
cmake -S . -B build \
  -DCMAKE_BUILD_TYPE=Release \
  -DLLAMA_OPENSSL=ON
```

Lancer la compilation :

```bash
cmake --build build --config Release -j4
```

Cette opération peut prendre plusieurs minutes.

### Vérification

Vérifier que l'exécutable du serveur a bien été généré :

```bash
ls -lh build/bin/llama-server
```

Vérifier ensuite que les options de commande du serveur sont accessibles :

```bash
./build/bin/llama-server --help
```

L'exécutable doit être conservé à l'emplacement suivant :

```text
~/projet_transcription/llama.cpp/build/bin/llama-server
```

L'étape suivante consiste à télécharger et à démarrer le modèle de langage Qwen3 utilisé par SEM Transcription.

## 3.6. Installation et démarrage du modèle Qwen3

SEM Transcription utilise le modèle de langage Qwen3-4B, quantifié au format Q4_K_M, pour analyser les transcriptions, générer des résumés et répondre à des questions sur leur contenu.

### Téléchargement et démarrage du modèle

Accéder au dossier de llama.cpp :

```bash
cd ~/projet_transcription/llama.cpp
```

Exécuter la commande suivante :

```bash
./build/bin/llama-server \
  -hf Qwen/Qwen3-4B-GGUF:Q4_K_M \
  --host 127.0.0.1 \
  --port 8080 \
  -c 4096 \
  -t 4 \
  --reasoning off \
  --no-ui
```

Lors de la première exécution, le modèle est automatiquement téléchargé depuis Hugging Face. Sa taille est d'environ 2,5 Go.

Une connexion Internet est nécessaire pour ce premier téléchargement.

### Paramètres de configuration

| Paramètre | Description |
|---|---|
| `-hf` | Modèle téléchargé depuis Hugging Face. |
| `--host 127.0.0.1` | Accès limité à la machine locale. |
| `--port 8080` | Port utilisé par le serveur. |
| `-c 4096` | Taille de la fenêtre de contexte. |
| `-t 4` | Nombre de threads utilisés. |
| `--reasoning off` | Désactivation du mode de raisonnement. |
| `--no-ui` | Désactivation de l'interface web de llama.cpp. |

### Vérification du fonctionnement

Une fois le modèle chargé, ouvrir un deuxième terminal sans fermer le premier.

Exécuter la commande suivante :

```bash
curl -sS --max-time 180 \
  http://127.0.0.1:8080/v1/chat/completions \
  -H "Content-Type: application/json" \
  -d '{
    "messages": [
      {
        "role": "user",
        "content": "Réponds uniquement : Bonjour SEM"
      }
    ],
    "max_tokens": 30,
    "temperature": 0
  }'
```

La réponse obtenue doit contenir le message `Bonjour SEM`.

### Utilisation avec SEM Transcription

Le serveur Qwen3 doit rester actif pendant l'utilisation des fonctionnalités d'analyse par intelligence artificielle.

Il n'est pas nécessaire de démarrer ce serveur pour effectuer une transcription audio ou une diarisation.

Pour arrêter le serveur, revenir dans le terminal dans lequel il a été lancé et utiliser la combinaison de touches `Ctrl + C`.

## 3.7. Installation de Pyannote et du modèle de diarisation

Pyannote.audio est une bibliothèque permettant d'identifier et de distinguer les différents interlocuteurs présents dans un enregistrement audio.

Cette fonctionnalité, appelée diarisation, permet à SEM Transcription d'associer les segments de transcription aux différents interlocuteurs.

### Création de l'environnement virtuel Python

Afin d'isoler les dépendances nécessaires à la diarisation, créer un environnement virtuel dédié :

```bash
cd ~/projet_transcription
python3 -m venv venv_pyannote
source venv_pyannote/bin/activate
```

Une fois l'environnement activé, le terminal affiche le préfixe `(venv_pyannote)`.

### Installation des bibliothèques Python

Exécuter les commandes suivantes :

```bash
python -m pip install --upgrade pip
```

```bash
python -m pip install \
  torch==2.9.1 \
  torchaudio==2.9.1 \
  pyannote.audio==4.0.7 \
  soundfile==0.14.0 \
  torchcodec==0.17.0 \
  huggingface_hub==2.3.0
```

Cette installation peut prendre plusieurs minutes et nécessite une connexion Internet.

### Vérification des dépendances

```bash
python -m pip check
```

Le terminal doit afficher :

```text
No broken requirements found.
```

### Autorisation d'accès au modèle Hugging Face

SEM Transcription utilise le modèle `pyannote/speaker-diarization-community-1`.

Pour télécharger ce modèle :

1. Créer un compte ou se connecter sur [Hugging Face](https://huggingface.co/).
2. Accéder à la [page du modèle](https://huggingface.co/pyannote/speaker-diarization-community-1).
3. Accepter les conditions d'accès au modèle.
4. Créer un jeton d'accès disposant des droits de lecture nécessaires depuis la page [Access Tokens](https://huggingface.co/settings/tokens).

Depuis le terminal, s'authentifier avec la commande suivante :

```bash
hf auth login
```

Saisir le jeton d'accès lorsque le terminal le demande.

**Attention :** le jeton d'accès est personnel et confidentiel. Il ne doit pas être intégré au code source de l'application ni partagé dans la documentation.

### Téléchargement du modèle de diarisation

Exécuter la commande suivante :

```bash
python - <<'PY'
from pyannote.audio import Pipeline

pipeline = Pipeline.from_pretrained(
    "pyannote/speaker-diarization-community-1"
)

print("Modèle chargé avec succès.")
PY
```

Le modèle est automatiquement téléchargé et conservé dans le cache local de Hugging Face.

### Vérification du fonctionnement hors ligne

Vérifier que le modèle peut être chargé depuis le cache local, sans accéder à Internet :

```bash
HF_HUB_OFFLINE=1 python - <<'PY'
import torch
from pyannote.audio import Pipeline

pipeline = Pipeline.from_pretrained(
    "pyannote/speaker-diarization-community-1"
)

pipeline.to(torch.device("cpu"))

print("Modèle local chargé avec succès.")
PY
```

Le terminal doit afficher :

```text
Modèle local chargé avec succès.
```

### Emplacement du modèle

Le modèle est conservé dans le dossier suivant :

```text
~/.cache/huggingface/hub/models--pyannote--speaker-diarization-community-1/
```

Il n'est pas nécessaire de déplacer les fichiers téléchargés.

SEM Transcription utilise automatiquement l'environnement virtuel et le modèle local lors de la diarisation.

### Désactivation de l'environnement virtuel

Une fois l'installation terminée, exécuter :

```bash
deactivate
```

Il n'est pas nécessaire d'activer manuellement cet environnement virtuel lors du démarrage de SEM Transcription.

## 3.8. Installation et démarrage de SEM Transcription

Une fois les différents moteurs d'intelligence artificielle et leurs modèles installés, procéder à l'installation de l'application SEM Transcription.

### Téléchargement de l'application

Télécharger l'archive `sem_transcription.zip` depuis le dépôt GitHub du projet SEM Transcription.

**Lien de téléchargement :** à venir dans la section Releases du dépôt.

Enregistrer l'archive dans le dossier `Downloads` du Raspberry Pi.

### Extraction de l'application

Ouvrir un terminal et exécuter les commandes suivantes :

```bash
cd ~/projet_transcription
python3 -m zipfile -e ~/Downloads/sem_transcription.zip .
```

Cette opération crée le dossier `sem_app`, contenant les fichiers Python nécessaires au fonctionnement de l'application.

### Vérification des fichiers

```bash
ls ~/projet_transcription/sem_app
```

Le dossier doit contenir les 13 fichiers suivants :

```text
sem_app/
├── analisis_completo.py
├── analyse_ia.py
├── app.py
├── chat_ia.py
├── diarisation_auto.py
├── export_documents.py
├── fragments_audio.py
├── fusion_locuteurs.py
├── gestion_reunions.py
├── lecteur_audio.py
├── nombres_locutores.py
├── resume_long.py
└── selection_fragments.py
```

### Démarrage de l'application

Depuis le terminal, exécuter :

```bash
cd ~/projet_transcription/sem_app
python3 app.py
```

L'interface graphique de SEM Transcription doit s'ouvrir.

Il n'est pas nécessaire d'activer manuellement l'environnement virtuel de Pyannote.

### Activation des fonctionnalités d'analyse par intelligence artificielle

Pour utiliser les fonctionnalités d'analyse, de synthèse et de questions-réponses, démarrer également le serveur Qwen3 dans un terminal distinct, conformément aux instructions de la section 3.6.

Le serveur doit rester actif pendant l'utilisation de ces fonctionnalités.

La transcription et la diarisation peuvent être utilisées indépendamment du serveur Qwen3.

---

[← Chapitre 2 — Prérequis matériels et logiciels](02-prerequis.md) | [Chapitre 4 — Configuration de l'application →](04-configuration.md)
