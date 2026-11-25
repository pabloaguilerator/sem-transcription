# 5. Guide d'utilisation de SEM Transcription

Cette section explique comment utiliser SEM Transcription depuis son interface graphique pour transcrire des enregistrements audio, corriger les textes obtenus et exploiter leur contenu à l'aide d'une intelligence artificielle locale.

L'application est conçue pour être utilisée sans connaissances particulières en programmation.

## 5.1. Démarrage de l'application

### Objectif

Ouvrir SEM Transcription sur le Raspberry Pi 5.

### Procédure

1. Allumer le Raspberry Pi et attendre le chargement du bureau.
2. Vérifier que les composants nécessaires sont disponibles.
3. Ouvrir SEM Transcription.

Pour un démarrage depuis le terminal :

```bash
cd ~/projet_transcription/sem_app
python3 app.py
```

L'interface graphique de SEM Transcription doit apparaître.

**Remarque :** les fonctions d'analyse par IA nécessitent que le serveur Qwen3 soit démarré séparément, conformément à la section 4.2 du [chapitre 4 — Configuration de l'application](04-configuration.md).

## 5.2. Importation d'un enregistrement audio

### Objectif

Sélectionner un fichier audio à transcrire.

### Procédure

1. Ouvrir SEM Transcription.
2. Utiliser la fonction de sélection d'un fichier audio.
3. Parcourir les dossiers du Raspberry Pi.
4. Sélectionner l'enregistrement souhaité.
5. Vérifier que le fichier a été correctement chargé.

### Vérification

Le fichier sélectionné doit être reconnu par l'application et prêt pour la transcription.

L'utilisation des microphones RØDE n'est pas obligatoire. Il est également possible d'enregistrer une réunion avec un autre appareil, comme un smartphone, un ordinateur ou un enregistreur numérique, puis d'importer le fichier audio dans SEM Transcription pour le transcrire. Il suffit de disposer d'un fichier audio dans un format compatible avec l'application.

**Conseil :** privilégier un enregistrement dans lequel les voix sont clairement audibles, avec le moins possible de bruit de fond.

## 5.3. Transcription et diarisation automatiques

### Objectif

Transformer un enregistrement audio en texte et identifier les différents interlocuteurs.

### Procédure

1. Vérifier qu'un fichier audio a été sélectionné.
2. Cliquer sur le bouton « Transcrire ».
3. Attendre la fin de la transcription et de la diarisation.
4. Consulter la transcription générée.

La transcription est effectuée localement sur le Raspberry Pi à l'aide du modèle Whisper Medium Q5. La diarisation est réalisée automatiquement à l'aide de Pyannote.audio.

La durée du traitement dépend notamment de la longueur de l'enregistrement et des ressources disponibles.

### Vérification

Le texte transcrit doit apparaître dans l'interface. Lorsque la diarisation a abouti, les segments de transcription sont associés aux différents interlocuteurs.

En cas d'échec de la diarisation, la transcription reste disponible sans identification automatique des interlocuteurs.

Il est recommandé de vérifier les noms propres, les termes techniques, les nombres et les passages dans lesquels plusieurs personnes parlent simultanément.

## 5.4. Consultation et correction de la transcription

### Objectif

Relire et corriger le texte généré automatiquement.

### Procédure

1. Consulter la transcription affichée.
2. Repérer les éventuelles erreurs.
3. Modifier les passages concernés directement dans l'interface.
4. Relire le texte corrigé.
5. Utiliser la fonction d'enregistrement ou d'exportation disponible.

### Identification des interlocuteurs

Les interlocuteurs identifiés automatiquement sont initialement désignés par des identifiants génériques, tels que `SPEAKER_00` et `SPEAKER_01`.

L'utilisateur peut les renommer directement dans l'interface afin de faciliter la lecture et l'exploitation de la transcription.

Il est recommandé de vérifier que les prises de parole sont correctement attribuées.

### Vérification

Les corrections doivent apparaître dans le texte final.

**Important :** la qualité des analyses réalisées ensuite dépend du contenu de la transcription. Il est donc préférable de corriger les erreurs importantes avant de générer un résumé ou de poser des questions à l'IA.

## 5.5. Enregistrement et exportation

### Objectif

Conserver les résultats d'une transcription pour une utilisation ultérieure.

### Procédure

Après avoir relu et corrigé le texte :

1. Utiliser la fonction d'enregistrement ou d'exportation.
2. Choisir l'option disponible selon le document souhaité.
3. Vérifier le fichier obtenu.

L'application permet de conserver les réunions et d'exporter des documents dans les formats suivants, selon la fonctionnalité utilisée :

- **TXT :** texte brut.
- **DOCX :** document Word.
- **PDF :** document destiné à la consultation ou au partage.

Les réunions enregistrées sont conservées dans le dossier `~/projet_transcription/reunions/`.

Les documents exportés sont disponibles dans le dossier `~/Downloads/`.

### Vérification

Ouvrir le document exporté et vérifier que le contenu correspond à la transcription corrigée ou au résultat de l'analyse, selon le document exporté.

## 5.6. Analyse de la transcription avec l'IA

### Objectif

Utiliser le modèle Qwen3 pour exploiter le contenu d'une transcription.

### Fonctionnement

SEM Transcription communique avec un modèle de langage installé localement sur le Raspberry Pi.

Ce modèle peut être utilisé pour produire des synthèses et répondre à des questions portant sur le contenu des réunions.

### Procédure

1. Vérifier que la transcription est disponible et corrigée.
2. Vérifier que le serveur Qwen3 est actif.
3. Accéder à la fonctionnalité d'analyse dans l'application.
4. Sélectionner l'opération souhaitée.
5. Attendre la génération du résultat.
6. Relire et vérifier le contenu produit.

### Vérification

Comparer le résultat de l'analyse avec la transcription d'origine.

**Important :** les réponses produites par une intelligence artificielle peuvent contenir des erreurs ou des interprétations incorrectes. Les informations importantes doivent toujours être vérifiées dans la transcription.

## 5.7. Questions sur le contenu d'une réunion

### Objectif

Retrouver des informations dans une transcription en posant des questions en langage naturel.

### Exemples d'utilisation

L'utilisateur peut notamment poser des questions comme :

- « Quels sont les principaux sujets abordés pendant la réunion ? »
- « Quelles décisions ont été prises ? »
- « Quelles tâches ont été attribuées aux participants ? »
- « Quels points doivent être discutés lors de la prochaine réunion ? »

### Procédure

1. Ouvrir la fonction de questions-réponses.
2. Saisir une question concernant la réunion.
3. Envoyer la demande.
4. Consulter la réponse générée.
5. Vérifier les informations dans la transcription.

Le modèle doit s'appuyer sur le contenu disponible et ne pas être considéré comme une source indépendante d'informations sur la réunion.

## 5.8. Bonnes pratiques d'utilisation

Pour obtenir des résultats fiables, il est recommandé de :

1. Utiliser des enregistrements audio de bonne qualité.
2. Éviter autant que possible les conversations simultanées.
3. Privilégier des enregistrements audio d'une durée maximale d'environ 20 minutes.
4. Diviser les enregistrements plus longs en plusieurs fichiers audio.
5. Vérifier les transcriptions avant leur analyse.
6. Contrôler les résumés et les réponses générés par l'IA.
7. Conserver les documents importants dans un emplacement clairement identifié.
8. Respecter les règles de confidentialité et de gestion des données applicables au contexte d'utilisation.

La recommandation relative à la durée des enregistrements tient compte des ressources matérielles limitées du Raspberry Pi 5. Les performances peuvent varier selon la durée des enregistrements et la complexité des traitements demandés.

Les limites matérielles et les performances de l'application sont présentées dans la section 1.5 du [chapitre 1 — Présentation du projet](01-presentation.md).

Le traitement local permet de limiter le recours à des services externes pour la transcription et l'analyse. Il ne dispense toutefois pas de respecter les règles de protection des données.

## 5.9. Vérification finale

Avant de terminer une session de travail, vérifier les points suivants :

- Le fichier audio a été correctement importé.
- La transcription a été générée.
- L'attribution des prises de parole aux interlocuteurs a été vérifiée lorsque la diarisation a été effectuée.
- Les erreurs importantes ont été corrigées.
- Le document final a été enregistré ou exporté.
- Les analyses générées ont été relues et vérifiées.
- Les fichiers nécessaires ont été conservés dans un emplacement approprié.

La section suivante, **6. Dépannage**, présentera les problèmes techniques les plus fréquents et les solutions permettant de les résoudre.

---

[← Chapitre 4 — Configuration de l'application](04-configuration.md) | [Chapitre 6 — Dépannage →](06-maintenance.md)
