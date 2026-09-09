# Chapitre 6 -- Limites et perimetre de ce parcours

Ce parcours couvre la structure de configuration multi-environnement du
pont (devnet/mainnet, avec le garde-fou `NEXT_PUBLIC_ENABLE_MAINNET`), la
construction manuelle des instructions Solana pour le programme de pont
(discriminateurs, derivation de PDA via un sel aleatoire, mecanisme de
repli sans paiement de relais), le mecanisme d appel de contrat Base
attache a une transaction de pont, l interface en ligne de commande
terminal et son mini-langage de commandes, la resolution d adresses
ENS/Basenames, et le guide de bout en bout pour deployer et ponter un
nouveau token SPL.

Sont volontairement laisses hors champ : l implementation des programmes
Solana et des contrats Base du pont eux-memes (ce depot en est uniquement
un client, les programmes/contrats sont deployes et maintenus separement
par l equipe Base, reference externe `github.com/base/bridge`) ; le detail
de l integration du faucet CDP (Coinbase Developer Platform,
`src/lib/cdpFaucet.ts`) ; les composants d interface purement visuels
(effets d animation "Matrix", theme graphique du terminal) qui ne portent
aucune logique de pont ; et la gestion fine des erreurs RPC/reseau au-dela
de leur traitement de base. Le README precise egalement que la version
hebergee publiquement (terminallyonchain.xyz) tourne exclusivement sur
Solana Devnet, l option mainnet n etant accessible que dans une instance
auto-hebergee avec la variable d environnement appropriee.

L objectif de ce parcours est de comprendre precisement comment cette
application construit et soumet des transactions de pont Solana vers Base,
avec ou sans appel de contrat attache, pas de documenter l implementation
interne des programmes de pont sous-jacents, qui appartiennent a un depot
distinct maintenu par Base.
