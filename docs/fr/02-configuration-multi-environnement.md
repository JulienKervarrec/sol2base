# Chapitre 2 -- Une configuration qui bascule integralement entre deux environnements

Le fichier `src/lib/constants.ts` centralise toute la configuration reseau
sous la forme de deux "presets" complets, `devnet` et `mainnet`
(`NETWORK_PRESETS`), chacun regroupant a la fois le cote Solana
(`SolanaClusterConfig` : URL RPC, adresses des programmes bridge et relayer,
receveur de frais de gas, mint du token USDC pontable) et le cote Base
(`BaseNetworkConfig` : `chainId`, URL RPC, adresse du contrat de pont, de son
validateur, et pour le mainnet uniquement, une fabrique de contrats
cross-chain et un orchestrateur de relayeurs). Selectionner un environnement
revient donc a echanger cet objet de configuration complet plutot que des
champs individuels epars dans le code.

Un detail de securite/produit merite d etre releve : la variable d
environnement `NEXT_PUBLIC_ENABLE_MAINNET` controle si l option mainnet
apparait meme dans la liste des environnements disponibles
(`AVAILABLE_ENVIRONMENTS`) -- si elle n est pas exactement egale a la chaine
`"true"`, l application reste cantonnee au devnet, quelle que soit la
configuration mainnet presente dans le code. C est un garde-fou deliberement
place au niveau de la liste d environnements disponibles plutot qu au niveau
de chaque action individuelle, ce qui reduit la surface ou un bug pourrait
laisser passer une transaction mainnet non voulue.

Les actifs pontables eux-memes (`BridgeAssetConfig`) sont egalement definis
par environnement : le devnet expose SOL natif et un token "Bridge USDC" de
test, tandis que le mainnet, dans la configuration par defaut du depot,
n expose que SOL natif -- une illustration du fait que la liste des actifs
supportes n est pas une propriete figee du protocole de pont lui-meme, mais
une decision de configuration de l application cliente.
