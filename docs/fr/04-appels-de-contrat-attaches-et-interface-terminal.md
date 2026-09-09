# Chapitre 4 -- Attacher un appel de contrat Base a la transaction de pont, et l interface en ligne de commande

Une des capacites les plus notables du depot est la possibilite de joindre
un appel de contrat Base arbitraire a une transaction de pont, pour qu il
soit execute automatiquement des reception des fonds sur Base : la fonction
`serializeOptionalCall` encode un objet `BaseContractCall` (type d appel --
`call`, `delegatecall`, `create` ou `create2` --, adresse cible, valeur en
ETH, et donnees calldata) dans le format binaire attendu par l instruction
de pont, ou l absence d appel est simplement representee par un octet `0`.
Ce mecanisme transforme le pont en un vecteur d execution a distance : au
lieu de se contenter de deplacer de la valeur, le message pontage peut
demander au relayeur d executer n importe quelle fonction de contrat sur
Base pour le compte du destinataire.

Cote interface, `src/lib/terminalParser.ts` implemente un mini-langage de
commandes textuelles inspire d un shell : `deploySpl <name> <symbol>
<decimals> <supply>` pour creer un nouveau token SPL de test, `remoteToken
<mint>` pour deriver l identifiant "distant" (bytes32) associe a un mint
SPL donne (une operation deterministe et independante du cluster, comme le
precise `docs/spl-to-base-erc20.md`), `bridge <amount> <asset> <destination>
[--call-contract ...] [--call-selector ...] [--call-args ...]` pour executer
un transfert avec ou sans appel attache, et `faucet <asset>` pour demander
des fonds de test. Chaque commande est tokenisee puis validee (types de
donnees, presence des champs requis pour un appel de type `call`) avant
d etre transformee en un objet `ParsedCommand` structure que le reste de
l application peut executer sans reparser de texte.

Ce choix d une interface terminal plutot qu un formulaire classique n est
pas seulement esthetique : il permet d exposer directement, de facon
composable, l ensemble des options avancees du protocole de pont (flags
`--call-*`, `--with-bc`, `--bc-fee`) sans multiplier les champs de formulaire
correspondants, au prix d une courbe d apprentissage plus elevee pour
l utilisateur final.
