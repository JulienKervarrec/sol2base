# Chapitre 5 -- Resoudre ENS et Basenames, et le guide de bout en bout pour ponter un nouveau token

`src/lib/addressResolver.ts` permet a l utilisateur de saisir une adresse
Base sous forme lisible plutot qu un hexadecimal brut : un nom se terminant
par `.eth` (mais pas `.base.eth`) est resolu comme un nom ENS classique via
une API publique (`api.ensdata.net`), tandis qu un nom se terminant par
`.base` ou `.base.eth` est normalise puis resolu comme un Basename via
plusieurs resolveurs tentes successivement. Le code precise explicitement
qu il s agit d une implementation a but demonstratif, s appuyant sur des
API publiques tierces plutot que sur une integration directe avec les
contrats de resolution ENS/Basenames -- un choix raisonnable pour un
prototype de hackathon mais qui ne conviendrait pas necessairement a une
application de production critique.

Le guide `docs/spl-to-base-erc20.md` documente le parcours complet pour
faire exister un nouveau token pontable de zero, en cinq etapes : creer un
token SPL sur Solana devnet avec `deploySpl` (mint complet vers le wallet
connecte) ; deriver son identifiant distant bytes32 avec `remoteToken` ;
deployer manuellement, via Basescan et la fonction `deploy(remoteToken,
name, symbol, decimals)` d une fabrique de contrats cross-chain, le jumeau
ERC-20 correspondant sur Base ; executer la commande `bridge` en pointant
vers ce nouveau contrat ERC-20 via le flag `--remote` ; et enfin verifier le
resultat des deux cotes (solde ERC-20 sur Basescan, solde SPL debite sur
l explorateur Solana). Ce guide illustre bien que "ponter un token" ne se
limite pas a l acte de transfert lui-meme : il suppose au prealable un
appariement explicite entre le token source et son representant sur la
chaine de destination, appariement que ce depot documente mais ne peut pas
automatiser lui-meme puisqu il depend d une action de deploiement manuelle
sur Base.
