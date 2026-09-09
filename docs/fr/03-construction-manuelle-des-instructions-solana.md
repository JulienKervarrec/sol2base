# Chapitre 3 -- Construire les instructions du programme de pont a la main

La classe `RealBridgeImplementation` (`src/lib/realBridgeImplementation.ts`)
construit les transactions Solana destinees aux programmes de pont sans
passer par un client genere automatiquement (comme le ferait un IDL Anchor
charge dynamiquement) : chaque instruction est serialisee manuellement,
octet par octet, dans un `Buffer`. Chaque type d instruction commence par un
discriminateur fixe de 8 octets (par exemple `[190, 190, 32, 158, 75, 153,
32, 86]` pour `bridge_sol`, ou `[41, 191, 218, 201, 250, 164, 156, 55]` pour
`pay_for_relay`) -- la signature caracteristique du systeme de
discriminateurs Anchor, ici reconstruite a la main plutot que generee.

Une transaction de pont typique combine deux instructions : `pay_for_relay`,
qui credite le receveur de frais de gas sur Solana pour compenser le cout du
relayeur qui executera l appel correspondant sur Base, et `bridge_sol` (ou
`bridge_spl` pour les tokens) qui transfere effectivement les fonds vers un
compte "vault" du programme de pont. Les deux instructions partagent le meme
"salt" -- 32 octets aleatoires generes via `crypto.getRandomValues`
(`createSaltBundle`) -- qui sert a deriver deux adresses de compte (PDA,
Program Derived Address) distinctes mais liees : `outgoing_message` (cote
programme de pont) et `mtr` / message-to-relay (cote programme relayeur),
calculees par les fonctions `deriveOutgoingMessagePda` et
`deriveMessageToRelayPda` de `src/lib/pdas.ts`. Un mecanisme de repli
explicite existe : si la construction de `pay_for_relay` echoue (par exemple
si le compte de configuration du relayeur n existe pas encore sur cet
environnement), le code retombe sur une transaction ne contenant que
l instruction de pont, sans le paiement du relais -- un choix qui privilegie
la tentative de transfert plutot que l echec complet, au prix probable d un
relais qui ne s executera pas automatiquement.

Le transfert de tokens SPL (`buildSplBridgeTransaction`) suit la meme
logique que le SOL natif, avec une derivation d adresse de vault
supplementaire qui incorpore a la fois le mint du token et l adresse
"distante" (le contrat ERC-20 correspondant sur Base) dans son sel de
derivation (`token_vault`, mint, `remoteTokenBytes`), garantissant un coffre
distinct par paire (token Solana, contrat Base).
