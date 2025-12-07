Merci de votre intérêt pour MiraKat !
Ce projet est encore jeune et expérimental, et votre participation peut vraiment faire la différence.
Toutes les contributions sont les bienvenues : code, idées, documentation, design, tests, corrections…

1. À propos du projet
MiraKat est une messagerie expérimentale visant à explorer :
  - le chiffrement moderne (potentiellement post-quantique),
  - l’échange de clés uniquement en local (QR code, Bluetooth, NFC…),
  - le transport de messages chiffrés via PING → PONG → MSG → ACK,
  - le stockage local uniquement (pas de serveur de messages),
  - les clés dérivées par conversation (isolation),
  - une approche simple, claire et éducative.
Ce n’est pas encore un produit final : la priorité est d’apprendre, de prototyper, et d’avancer ensemble.

2. Comment contribuer
Il existe plusieurs façons de contribuer :
  2.1. Proposer une idée ou une amélioration
      Ouvre une Issue GitHub
      (bouton “Issues” → “New Issue”)
      Explique ton idée clairement et pourquoi elle est utile.
  2.2. Corriger un bug
      Cherche dans les Issues si le bug existe déjà.
      Sinon, crée une Issue avec :
        ce qui se passe,
        ce à quoi tu t’attendais,
        comment reproduire le problème.
  2.3. Proposer du code (Pull Request)
      Fork le dépôt (bouton “Fork”).
      Clone ton fork :
        git clone https://github.com/TON_NOM/MiraKat
      Crée une branche pour ta modification :
        git checkout -b feature/ma-feature
      Écris ton code, ajoute des commentaires si nécessaire.
      Pousse ta branche :
        git push origin feature/ma-feature
      Ouvre une Pull Request (PR) sur GitHub.
  Tu n’as pas besoin d’ajouter une fonctionnalité complète pour contribuer — même une petite PR est utile

3. Prototypes et expérimentation
    MiraKat est en phase de conception : tu peux proposer des prototypes dans n’importe quel langage.
    Les directions les plus utiles pour commencer :
   3.1. Prototypes transport / protocole
      Créer une version simple du protocole :
        PING
        PONG
        MSG
        ACK
      Le tout en local (localhost) pour commencer.
    3.2. Stockage local
      Une table ou un fichier local pour stocker :
        messages en attente,
        messages envoyés,
        messages reçus.
    3.3. Crypto (classique → post-quantique)
      Commencer avec une crypto simple (AES, Curve25519…).
      Préparer l’intégration future de liboqs (Kyber, NTRU…).
    3.4. Clés par conversation
      Proposer un mécanisme simple :
        conversation_id (UUID)
        derive → conversation_key via HKDF

4. Structure du dépôt (proposée)
   Cette structure est indicative : les contributeurs peuvent en proposer une meilleure.
       /protocol/      → structures de messages (PING, MSG, ACK…)
       /crypto/        → gestion des clés, chiffrement, KDF
       /storage/       → fichiers, SQLite…
       /transport/     → sockets TCP, libp2p, WebRTC…
       /client/        → interface CLI ou GUI
       /docs/          → documentation technique
       /tests/          → tests unitaires

5. Style de code
   Simple et lisible
    MiraKat est un projet pédagogique : préférons la clarté à l’optimisation.
   Commentaires utiles
    Un commentaire clair vaut 10 lignes de code obscures.
   Pas de dépendances inutiles
    Éviter les frameworks lourds si 5 lignes de code suffisent.
   Garder les commits propres
    Un commit = un changement logique.
6. Bugs, sécurité et vie privée
  MiraKat expérimente des concepts liés à la sécurité et au chiffrement.
  Si vous trouvez une vulnérabilité potentielle :
  Merci de ne pas l’exposer publiquement dans une Issue,
  Préférez envoyer une alerte privée au mainteneur (email dans le repo).

7. Merci !
  Merci de votre intérêt pour MiraKat 🙌
  Chaque contribution — même minuscule — fait avancer le projet.
  Si vous avez la moindre question, ouvrez simplement une Issue.
  C’est un projet communautaire avant tout
