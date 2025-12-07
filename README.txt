# MiraKat

MiraKat est une application de messagerie sécurisée pensée comme un projet open source expérimental.

L'idée : une messagerie qui combine :

- Chiffrement moderne, idéalement post-quantique à terme (lattice / PQC).
- Échange de clés uniquement en local (QR code, Bluetooth, NFC, etc.).
- Transport sur Internet uniquement de données chiffrées, jamais de clés.
- Envoi conditionnel : l’émetteur n’envoie un message que si le destinataire répond présent (PING/PONG).
- Messages en attente stockés uniquement en local chez l’émetteur.
- Accusés de réception (ACK) quand le message est reçu complet.
- Clé par conversation pour isoler les discussions entre elles.

MiraKat est pensé comme une base de travail : un projet qui puisse servir de point de départ à d’autres outils, recherches ou forks.

---

## 🎯 Objectifs

- Créer un protocole simple et documenté pour des messages chiffrés texte / vocal / petites images.
- Expérimenter avec des briques post-quantiques (via [liboqs](https://openquantumsafe.org/)).
- Fournir un exemple de messagerie P2P / semi-P2P sans serveur de stockage central.
- Servir de terrain d’expérimentation pour d’autres développeurs : nouvelles UIs, nouveaux protocoles, nouvelles couches réseau, etc.

> MiraKat est un projet d’étude / expérimental.  
> Ce n’est pas (encore) une solution de sécurité prête pour la production.

---

Idée générale du protocole

1. Pairing local

Deux appareils A et B :

1. Génèrent chacun une paire de clés (à terme : post-quantique).
2. Échangent la clé publique en local (QR code, Bluetooth, etc.).
3. Dérivent un secret partagé de base.
4. Pour chaque conversation, dérivent une clé de conversation :

```txt
conversation_key = KDF(shared_base_secret_AB, conversation_id)


2. Types de messages

Tous les messages sont sérialisés (JSON ou binaire) et, à terme, chiffrés avec la clé de conversation.

PING
L’émetteur demande au destinataire s’il est disponible maintenant.

{
  "type": "PING",
  "from": "contact_id_A",
  "conversation_id": "UUID_CONV",
  "timestamp": 1234567890
}


PONG
Le destinataire répond qu’il est dispo.

{
  "type": "PONG",
  "from": "contact_id_B",
  "conversation_id": "UUID_CONV",
  "timestamp": 1234567891
}


MSG
Un message utilisateur (texte, vocal, image compressée).

{
  "type": "MSG",
  "conversation_id": "UUID_CONV",
  "message_id": "UUID_MSG",
  "content_type": "text | voice | image",
  "chunk_index": 0,
  "chunk_total": 1,
  "payload": "<données chiffrées>",
  "timestamp": 1234567892
}


ACK
Accusé de réception d’un message complet.

{
  "type": "ACK",
  "conversation_id": "UUID_CONV",
  "message_id": "UUID_MSG",
  "status": "RECEIVED",
  "timestamp": 1234567893
}


Optionnel :

CHECK (pull côté destinataire, pour demander s’il y a des messages en attente).


3. Logique côté émetteur

    L’utilisateur compose un message.

        Le message est :

        sérialisé,

        chiffré avec conversation_key,

        enregistré en local avec un statut EN_ATTENTE_D_ENVOI.

    L’appli envoie un PING.

    Si PONG reçu :

        envoi de MSG,

        attente d’un ACK,

        si ACK → statut REÇU.

    Si pas de PONG :

        le message reste stocké localement,

        tentative automatique plus tard (ex. dans 1h/2h),

        ou relance manuelle par l’utilisateur.


4. Logique côté destinataire

    À la réception d’un PING :

        si l’appli est active → envoi de PONG.

    À la réception d’un MSG :

        reconstitution des chunks,

        déchiffrement,

        stockage local + notification,

        envoi d’un ACK.

Architecture envisagée (MVP)

    Pour un premier prototype (référence, pas imposé) :

        Langage : Python, Go, Rust ou autre, au choix des contributeurs.

    Modules :

        crypto/ : génération de clés, KDF, chiffrement / déchiffrement.

        protocol/ : définitions des messages (PING, PONG, MSG, ACK, CHECK).

        storage/ : stockage local (SQLite, fichiers, etc.).

        transport/ : sockets TCP ou librairie P2P (ex. libp2p).

        client/ : logique de l’appli (émetteur/récepteur, UI simple).