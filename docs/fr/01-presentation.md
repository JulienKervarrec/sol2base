# Chapitre 1 -- Presentation de sol2base

Ce depot (publie sous le nom "Terminally Onchain" par son auteur, dans le
cadre d un hackathon Base) est une application Next.js qui expose un pont
(bridge) entre Solana et Base : elle permet a un utilisateur connecte avec un
wallet Solana (Phantom, Solflare) de transferer du SOL natif ou des tokens
SPL vers une adresse Base, et meme d attacher un appel de contrat arbitraire
sur Base a executer au moment de la reception. Le README resume bien
l intention : "Call any contract on Base from your Solana wallet".

Contrairement a beaucoup de demos de cette bibliotheque qui exposent une
interface a formulaires classique, celle-ci choisit une esthetique "terminal
hacker" : fond noir, texte vert, polices monospace, effets de pluie facon
Matrix -- et surtout, une interface en ligne de commande textuelle
(`deploySpl`, `remoteToken`, `bridge`, `faucet`, voir chapitre 4) plutot que
des boutons. Le pont s appuie sur des programmes Solana et des contrats Base
deja deployes par l equipe Base elle-meme (et non par ce depot) : un
programme "bridge" et un programme "relayer" sur Solana, un contrat "bridge"
et un "bridge validator" sur Base, disponibles a la fois sur les
environnements de test (Solana Devnet <-> Base Sepolia) et de production
(Solana Mainnet <-> Base Mainnet).

Le fichier `docs/spl-to-base-erc20.md` du depot fournit un guide pratique
complementaire : comment creer un nouveau token SPL sur Solana, deployer son
"jumeau" ERC-20 sur Base via une fabrique de contrats dediee, puis brancher
les deux avant de pouvoir les faire transiter par le pont.
