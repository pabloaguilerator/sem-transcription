# 6. Dépannage

Cette section présente les principaux problèmes susceptibles de survenir lors de l'utilisation de SEM Transcription sur Raspberry Pi 5.

Pour chaque situation, les vérifications proposées permettent d'identifier l'origine du problème avant d'effectuer des modifications.

## 6.1. L'application ne démarre pas

### Problème

La fenêtre de SEM Transcription ne s'ouvre pas ou l'application se ferme immédiatement.

### Causes possibles

- Le fichier principal `app.py` est introuvable.
- Une bibliothèque Python nécessaire est absente.
- Une erreur empêche le lancement de l'interface graphique.

### Procédure

Ouvrir un terminal depuis l'environnement graphique du Raspberry Pi et lancer l'application manuellement :

```bash
cd ~/projet_transcription/sem_app
python3 app.py
```

Observer les éventuels messages d'erreur affichés dans le terminal.

Vérifier également la présence du fichier principal :

```bash
ls -l ~/projet_transcription/sem_app/app.py
```

Vérifier les principales bibliothèques Python :

```bash
python3 -c "import tkinter, docx, reportlab; print('OK')"
```

### Solution

- Si le fichier est absent, vérifier que le dossier de l'application a été correctement extrait conformément à la section 3.8 du [chapitre 3 — Installation de l'application](03-installation.md).
- Si Python signale une bibliothèque manquante, identifier son nom et consulter les dépendances indiquées dans la section 3.2 avant de procéder à son installation.
- Vérifier que l'application est lancée depuis un environnement graphique.

## 6.2. La transcription ne fonctionne pas

### Problème

Le traitement ne démarre pas, s'interrompt ou ne produit aucun texte.

### Causes possibles

- L'exécutable Whisper.cpp est introuvable.
- Le modèle de transcription est absent.
- Le fichier audio ne peut pas être lu ou converti.
- Une erreur survient pendant le traitement.

### Procédure

Vérifier la présence de l'exécutable Whisper.cpp :

```bash
ls -l ~/projet_transcription/whisper.cpp/build/bin/whisper-cli
```

Vérifier le modèle :

```bash
ls -lh ~/projet_transcription/whisper.cpp/models/ggml-medium-q5_0.bin
```

Vérifier que FFmpeg est installé :

```bash
ffmpeg -version
```

Pour examiner un fichier audio :

```bash
ffprobe "chemin/vers/enregistrement.wav"
```

Remplacer le chemin indiqué par celui du fichier concerné.

### Solution

Si un composant est absent, consulter les sections 3.3 et 3.4 du [chapitre 3 — Installation de l'application](03-installation.md).

Si le fichier audio est illisible, vérifier son intégrité et son format avant de relancer la transcription.

## 6.3. La transcription est de mauvaise qualité

### Problème

Le texte contient de nombreuses erreurs, des mots manquants ou des passages incompréhensibles.

### Causes possibles

- Le son est de mauvaise qualité.
- Les interlocuteurs parlent simultanément.
- Le volume d'enregistrement est trop faible.
- Le fichier contient du bruit de fond.
- Certains noms propres ou termes spécialisés sont mal reconnus.

### Solutions recommandées

- Écouter l'enregistrement d'origine.
- Vérifier que les voix sont clairement audibles.
- Utiliser un enregistrement de meilleure qualité si possible.
- Corriger manuellement les erreurs dans l'interface.
- Vérifier les passages importants avant de produire une analyse avec l'IA.

**Remarque :** même avec un modèle de transcription performant, une relecture humaine reste nécessaire pour les documents importants.

## 6.4. Le serveur Qwen3 ne répond pas

### Problème

Les fonctionnalités d'analyse ou de questions-réponses ne produisent aucun résultat.

### Causes possibles

- Le serveur llama.cpp n'a pas été démarré.
- Le modèle Qwen3 est encore en cours de chargement.
- Le serveur s'est arrêté.
- Le port 8080 est indisponible.

### Procédure

Vérifier l'état du serveur :

```bash
curl -s http://127.0.0.1:8080/health
```

Si aucune réponse n'apparaît, vérifier si le processus fonctionne :

```bash
pgrep -af llama-server
```

### Solution

Si le serveur n'est pas actif, le démarrer conformément à la procédure décrite dans la section 3.6 du [chapitre 3 — Installation de l'application](03-installation.md).

Attendre la fin du chargement du modèle et laisser le terminal du serveur ouvert.

Dans un deuxième terminal, répéter la vérification :

```bash
curl -s http://127.0.0.1:8080/health
```

Résultat attendu :

```json
{"status":"ok"}
```

Si le serveur ne démarre pas, consulter les messages d'erreur affichés dans son terminal.

**Remarque :** une réponse indiquant « Loading model » signifie que le chargement n'est pas encore terminé. Il faut patienter avant de lancer une analyse.

Le serveur Qwen3 n'est pas nécessaire pour utiliser les fonctionnalités de transcription et de diarisation.

## 6.5. L'analyse par IA est très lente

### Problème

La génération d'un résumé ou d'une réponse prend beaucoup de temps.

### Causes possibles

- Le modèle est exécuté sur le processeur du Raspberry Pi.
- Le texte à analyser est volumineux.
- Plusieurs traitements sont exécutés simultanément.
- La mémoire disponible est insuffisante.

### Procédure

Consulter l'utilisation de la mémoire :

```bash
free -h
```

Puis consulter l'utilisation des ressources en temps réel :

```bash
top
```

Appuyer sur la touche `q` pour quitter cet affichage.

Vérifier également l'espace de stockage disponible :

```bash
df -h /
```

### Solutions recommandées

- Attendre la fin du traitement en cours.
- Éviter de lancer plusieurs analyses simultanément.
- Fermer les applications inutilisées.
- Vérifier que le Raspberry Pi dispose de suffisamment de mémoire et d'espace de stockage.
- Vérifier que le Raspberry Pi dispose d'un système de refroidissement adapté.
- Privilégier des enregistrements audio d'une durée maximale d'environ 20 minutes.
- Diviser les enregistrements plus longs en plusieurs fichiers audio.

**Important :** la lenteur ne signifie pas nécessairement que l'application est bloquée. Les traitements réalisés localement peuvent nécessiter plusieurs minutes, voire davantage pour les enregistrements longs.

Les limites matérielles et les performances de l'application sont présentées dans la section 1.5 du [chapitre 1 — Présentation du projet](01-presentation.md).

## 6.6. Les microphones RØDE ne sont pas détectés

### Problème

Le Raspberry Pi ne reconnaît pas le récepteur RØDE Wireless GO II.

### Causes possibles

- Le récepteur n'est pas correctement connecté.
- Le câble USB présente un problème.
- Le périphérique n'est pas reconnu par le système.
- Le numéro de carte audio a changé.

### Procédure

Connecter le récepteur puis exécuter :

```bash
lsusb
```

Afficher ensuite les périphériques de capture :

```bash
arecord -l
```

### Solutions recommandées

- Vérifier le câble USB.
- Reconnecter le récepteur.
- Essayer un autre port USB.
- Identifier le numéro de carte attribué au périphérique.
- Consulter la configuration décrite dans la section 4.3 du [chapitre 4 — Configuration de l'application](04-configuration.md).

**Remarque :** le numéro de carte audio peut changer d'une session à l'autre.

## 6.7. L'enregistrement contient du bruit blanc

### Problème

Un enregistrement réalisé avec les microphones RØDE contient un bruit important ou devient difficilement exploitable.

### Causes possibles

Les essais réalisés avec le récepteur RØDE Wireless GO II ont montré qu'une sélection incorrecte du format audio pouvait provoquer du bruit blanc.

### Configuration de référence

| Paramètre | Valeur |
|---|---|
| Format | S24_3LE |
| Résolution | 24 bits |
| Fréquence | 48 000 Hz |
| Canaux | 2 (stéréo) |

### Solution

Vérifier que le périphérique de capture et le logiciel d'enregistrement utilisent les paramètres indiqués ci-dessus.

Pour réaliser un court enregistrement de test, identifier le numéro de carte audio :

```bash
arecord -l
```

Puis lancer l'enregistrement. Par exemple, pour la carte 2 et le périphérique 0 :

```bash
arecord -D hw:2,0 -f S24_3LE -r 48000 -c 2 -d 10 ~/test_audio.wav
```

Adapter les numéros de carte et de périphérique à la configuration du Raspberry Pi.

Écouter ensuite l'enregistrement :

```bash
ffplay -autoexit ~/test_audio.wav
```

**Important :** privilégier le format S24_3LE avec la configuration matérielle de référence. Effectuer un court enregistrement de test avant de commencer une réunion.

## 6.8. La diarisation ne fonctionne pas

### Problème

L'identification des interlocuteurs échoue ou produit une erreur.

### Causes possibles

- L'environnement virtuel dédié à la diarisation est absent ou incomplet.
- Une dépendance est absente ou incompatible.
- La lecture du fichier audio échoue.
- Le modèle de diarisation n'est pas disponible dans le cache local.

### Procédure

Activer l'environnement virtuel pour effectuer les vérifications :

```bash
source ~/projet_transcription/venv_pyannote/bin/activate
```

Vérifier les dépendances installées :

```bash
python -m pip check
```

Vérifier l'importation des principales bibliothèques :

```bash
python -c "import torch, soundfile, pyannote.audio; print('OK')"
```

Vérifier que le modèle de diarisation peut être chargé localement :

```bash
HF_HUB_OFFLINE=1 python - <<'PY'
import torch
from pyannote.audio import Pipeline

pipeline = Pipeline.from_pretrained(
    "pyannote/speaker-diarization-community-1"
)

pipeline.to(torch.device("cpu"))

print("Modèle de diarisation chargé")
PY
```

Quitter ensuite l'environnement virtuel :

```bash
deactivate
```

### Solutions recommandées

- Si une dépendance est absente, consulter la procédure d'installation décrite dans la section 3.7 du [chapitre 3 — Installation de l'application](03-installation.md).
- Si le modèle ne peut pas être chargé, vérifier qu'il a été correctement téléchargé depuis Hugging Face.
- Si la lecture audio échoue, vérifier l'intégrité du fichier et la disponibilité de SoundFile.
- Si un avertissement lié à TorchCodec apparaît, vérifier si le traitement se poursuit. L'application utilise SoundFile pour lire les fichiers audio destinés à la diarisation.
- Si les interlocuteurs sont mal identifiés, vérifier la qualité de l'enregistrement et les passages dans lesquels plusieurs personnes parlent simultanément.

**Remarque :** l'activation manuelle de l'environnement virtuel est uniquement nécessaire pour effectuer ces vérifications. SEM Transcription utilise automatiquement l'interpréteur Python dédié à la diarisation.

En cas d'échec de la diarisation, la transcription reste disponible sans identification automatique des interlocuteurs.

## 6.9. Impossible d'exporter un document

### Problème

L'enregistrement ou l'exportation d'une transcription échoue.

### Causes possibles

- Le dossier de destination n'est pas accessible.
- L'utilisateur ne dispose pas des permissions nécessaires.
- L'espace de stockage est insuffisant.
- Une bibliothèque nécessaire à l'exportation est absente.
- Une erreur survient lors de la génération du fichier.

### Procédure

Vérifier l'espace disponible :

```bash
df -h /
```

Vérifier la présence des dossiers de destination et leurs permissions :

```bash
ls -ld ~/Downloads
ls -ld ~/projet_transcription/reunions/
```

Ces dossiers sont créés automatiquement par l'application lorsque cela est nécessaire. Leur absence avant le premier enregistrement ou la première exportation n'indique donc pas nécessairement une erreur.

Vérifier les bibliothèques nécessaires à l'exportation Word et PDF :

```bash
python3 -c "import docx, reportlab; print('OK')"
```

Pour les documents PDF, vérifier également la présence des polices DejaVu :

```bash
ls -l /usr/share/fonts/truetype/dejavu/DejaVuSans.ttf
ls -l /usr/share/fonts/truetype/dejavu/DejaVuSans-Bold.ttf
```

### Solutions recommandées

- Vérifier que les dossiers de destination sont accessibles en écriture.
- Libérer de l'espace de stockage si nécessaire.
- Si une bibliothèque ou une police est absente, consulter la section 3.2 du [chapitre 3 — Installation de l'application](03-installation.md).
- Si le problème persiste, lancer l'application depuis le terminal afin d'identifier le message d'erreur.

Les réunions enregistrées sont conservées dans le dossier `~/projet_transcription/reunions/`, tandis que les documents exportés sont enregistrés dans `~/Downloads/`.

## 6.10. Informations à transmettre en cas de problème

Lorsqu'un problème ne peut pas être résolu à l'aide des vérifications précédentes, il est utile de conserver les informations suivantes :

- La fonctionnalité concernée.
- Les étapes réalisées avant l'apparition du problème.
- Le message d'erreur exact.
- Le format et la durée du fichier audio concerné, si nécessaire.
- Les vérifications déjà effectuées.

Ces informations facilitent le diagnostic et la résolution des problèmes techniques.

**Attention :** ne pas transmettre d'enregistrements ou de transcriptions contenant des données confidentielles sans autorisation appropriée. Ne pas communiquer les jetons d'accès Hugging Face ni les autres identifiants personnels.

---

[← Chapitre 5 — Guide d'utilisation de SEM Transcription](05-utilisation.md) | [Retour à l'accueil du projet](../README.md)
