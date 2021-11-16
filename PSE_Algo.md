# Algorithmique et méthodes de programmation

#### Introduction

Cet exposé traite de la partie algo du programme PSE.

Les trois premières parties présentent les bases de l'algorithmique avec les structures de contrôle, les tris et les structures
de données. La quatrième partie traite très rapidement du sujet hautement important des algorithmes de chiffrements. La
cinquième présente succintement la POO. La sixième survole les mécanismes de la compilation et traite de la différence entre
langage compilé et langage interprété. Enfin la dernière partie traite des APIs (utilisés dans les échanges de données
inter-applicatifs) des spécifications SOAP, REST et de leurs évolutions récentes : GraphQL de Facebook et gRPC de Google.

#### Table des matières

* [Structures de contrôle](#structures-de-contrôle)
    + [Structures de contrôle séquentielles](#structures-de-contrôle-séquentielles)
    + [Structures de contrôle itératives](#structures-de-contrôle-itératives)
    + [Extensions de la notion de boucles](#extensions-de-la-notion-de-boucles)
    + [Sous-programmes](#sous-programmes)
    + [Exceptions](#exceptions)
    + [Programmation multitâche](#programmation-multitâche)
    + [Programmation événementielle](#programmation-événementielle)
* [Algorithmes de tri](#algorithmes-de-tri)
    + [Critère de classification](#critère-de-classification)
    + [Exemples d'algorithmes de tri](#exemples-dalgorithmes-de-tri)
* [Structures de données](#structures-de-données)
    + [Pile](#pile)
    + [File](#file)
    + [Liste](#liste)
    + [Arbre enraciné](#arbre-enraciné)
    + [Arbre binaire de recherche](#arbre-binaire-de-recherche)
    + [Arbre B](#arbre-b)
    + [Arbre rouge-noir](#arbre-rouge-noir)
    + [Tas](#tas)
    + [Table de hachage](#table-de-hachage)
* [Algorithmes de chiffrements](#algorithmes-de-chiffrements)
    + [Chiffrement symétrique AES](#chiffrement-symétrique-aes)
    + [Chiffrement asymétrique RSA](#chiffrement-asymétrique-rsa)
* [Programmation orientée objet](#programmation-orientée-objet)
* [Compilation](#compilation)
* [API](#api)
    + [SOAP](#soap)
    + [REST](#rest)
    + [GraphQL](#graphql)
    + [gRPC](#grpc)

## Structures de contrôle

En programmation informatique, une structure de contrôle est une instruction particulière à un langage de programmation
impératif pouvant dévier le flot de contrôle du programme la contenant lorsqu'elle est exécutée. Si, au plus bas niveau,
l'éventail se limite généralement aux branchements et aux appels de sous-programme, les langages structurés offrent des
constructions plus élaborées comme les alternatives (if, if-else, switch...), les boucles (while, do-while, for...) ou encore
les appels de fonction. Outre les structures usuelles, la large palette des structures de contrôle s'étend des constructions de
gestion d'exceptions (try-catch...) fréquemment trouvés dans les langages de haut niveau aux particularismes de certains
langages comme les instructions différées (defer) de Go.

### Structures de contrôle séquentielles

Un programme informatique impératif est une suite d'instructions. Un registre interne de processeur, le compteur ordinal (PC),
est chargé de mémoriser l'adresse de la prochaine instruction à exécuter.

La plupart des instructions d'un programme sont exécutées séquentiellement : après le traitement de l'instruction courante le
compteur ordinal est incrémenté, et la prochaine instruction est chargée.

La séquence est donc la structure de contrôle implicite. Elle donne l'ordre d'exécution des instructions, souvent séparées par
un point-virgule ou par des retours chariots.

Un programme s'arrête généralement après l'exécution de la dernière instruction. La plupart des langages de programmation
proposent également une ou plusieurs instructions pour stopper l'exécution du programme à une position arbitraire.

Selon l'environnement d'exécution sous-jacent (système d'exploitation ou microprocesseur), cet arrêt peut être définitif ou
correspondre à une suspension de l'exécution du programme en attendant un évènement externe : c'est par exemple le
fonctionnement habituel de la plupart des instructions d'entrée sorties qui bloquent le flot d'exécution (mécanisme
d'interruption avec stockage en mémoire tampon) jusqu'à ce que le périphérique concerné ait terminé de traiter les données.

### Structures de contrôle itératives

Ces instructions permettent de réaliser une machine à états finis, cela signifie que leur seul effet de bord est de modifier un
registre qui correspond à l'état courant du programme.

Dans un processeur, cet état correspond à la valeur du compteur ordinal.

Commandes à étiquettes :

* Sauts inconditionnels
* Sauts conditionnels
* Sous programmes, commandes de sorties de boucles

Commandes de blocs :

* Blocs d'instructions
* Alternatives (if, then, else, switch)
* Boucles (do, while, each)

### Extensions de la notion de boucles

Un compteur permet de réaliser une boucle associée à une variable entière ou un pointeur qui sera incrémenté à chaque itération.
Il est souvent utilisé pour exploiter les données d'une collection indexée (boucle for).

Un itérateur (ou curseur ou énumérateur) est un objet qui permet de réaliser une boucle parcourant tous les éléments contenus
dans une structure de données.

### Sous-programmes

Un sous-programme permet la réutilisation d'une partie du code et ainsi le développement des algorithmes récursifs.

Beaucoup de langages modernes ne supportent pas directement la notion de sous-programme au profit de constructions de haut
niveau qui peuvent être appelées, d'un langage à l'autre *procédure, fonction, méthode*, ou *routine*. Ces constructions
ajoutent la notion de passage de paramètres et surtout le cloisonnement des espaces de noms pour éviter que le sous-programme
ait un effet de bord sur la routine appelante.

Il existe diverses extensions à la notion de procédure comme les coroutines (routine avec suspension), signaux, et slots
(signaux implémentés pour les objets), fonctions de rappel (callback, traitement post-fonction), méthodes virtuelles
(programmation par contrat, méthode implémentée dans les classes héritées) Elles permettent de modifier dynamiquement, c'est à
dire à l'exécution, la structure du flot d'exécution du programme.

### Exceptions

Tout programme en exécution peut être sujet à des erreurs pour lesquelles des stratégies de détection et de réparation sont
possibles. Ces erreurs ne sont pas des bugs mais des conditions exceptionnelles dans le déroulement normal d'une partie d'un
programme.

### Programmation multitâche

Dans un système multitâche, plusieurs flots d'exécutions, appelés processus légers, s'exécutent simultanément.

Il est alors nécessaire d'assurer la synchronisation de ces flots d'exécution. Dans la plupart des langages, cela est réalisé
via des bibliothèques externes ; certains d'entre eux intègrent néanmoins des structures de contrôle permettant d'agir sur des
tâches concourantes.

### Programmation événementielle

La programmation évènementielle est une autre façon de contrôler le flot d'exécutions d'un programme. Il s'agit de créer des
gestionnaires qui viendront s'abonner à une boucle mère, chargée d'aiguiller les évènements qui affectent le logiciel.

## Algorithmes de tri

Un algorithme de tri est, en informatique ou en mathématiques, un algorithme qui permet d'organiser une collection d'objets
selon une relation d'ordre déterminée. Les objets à trier sont des éléments d'un ensemble muni d'un ordre total. Il est par
exemple fréquent de trier des entier selon la relation d'ordre usuelle "est inférieur ou égal à". Les algorithmes de tris sont
utilisés dans de très nombreuses situations. Ils sont en particulier utiles à de nombreux algorithmes plus complexes dont
certains algorithmes de recherche, comme la recherche dichotomique. Ils peuvent également servir pour mettre des données sous
forme canonique ou les rendre plus lisibles pour l'utilisateur.

La collection à trier est souvent donnée sous forme de tableau, afin de permettre l'accès direct aux différents éléments de la
collection, ou sous forme de liste, ce qui peut se révéler être plus adapté à certains algorithmes et l'usage de la
programmation fonctionnelle.

Bon nombre d'algorithmes de tri procèdent par comparaison successives, et peuvent donc être définis indépendamment de l'ensemble
auquel appartiennent les éléments de la relation d'ordre associée. Un même algorithme peut par exemple être utilisé pour trier
les réels selon la relation d'ordre usuelle "est inférieur ou égal à" et des chaînes de caractères selon l'ordre
lexicographique. Ces algorithmes se prêtent naturellement à une implémentation polymorphe (différents types).

Les algorithmes de tri sont souvent étudiés dans les cours d'algorithmique pour introduire des notions comme la complexité
algorithmique ou la terminaison.

### Critère de classification

La classification des algorithmes de tri est très importante, car elle permet de choisir l'algorithme le plus adapté au problème
traité, tout en tenant compte des contraintes imposées par celui-ci. Les principales caractéristiques qui permettent de
différencier les algorithmes de tri, outre leur principe de fonctionnement, sont la complexité temporelle, la complexité
spatiale et le caractère stable.

La complexité temporelle (en moyenne ou dans le pire des cas) mesure le nombre d'opération élémentaires effectuées pour trier
une collection d'éléments. C'est un critère majeur pour comparer les algorithmes de tri, puisque c'est une estimation directe du
temps d'exécution de l'algorithme. Dans le cas des algorithmes de tri par comparaison, la complexité en temps est le plus
souvent assimilable au nombre de comparaisons effectuées, la comparaison et l'échange éventuel de deux valeurs s'effectuant en
temps constant.

La complexité spatiale (en moyenne ou dans le pire des cas) représente, quant à elle, la quantité de mémoire dont va avoir
besoin l'algorithme pour s'exécuter. Celle-ci peut dépendre, comme le temps d'exécution, de la taille de l'entrée. Il est
fréquent que les complexités spatiales en moyenne et dans le pire des cas soient identiques. C'est souvent le cas lorsqu'une
complexité est donnée sans indication supplémentaire.

Un tri est dit *en place* s'il n'utilise qu'un nombre très limité de variables et qu'il modifie directement la structure qu'il
est en train de trier. Ceci nécessite l'utilisation d'une structure de donnée adaptée (un tableau par exemple). Ce caractère
peut être très important si on ne dispose pas de beaucoup de mémoire.

Toutefois, on ne déplace pas, en général, les données elles-mêmes, mais on modifie seulement des références (ou pointeurs) vers
ces dernières.

Un tri est dit *stable* s'il préserve l'ordonnancement initial des éléments que l'ordre considère comme égaux (tri à bulles, tri
par insertion et le tri par fusion).

Un tri *interne* s'effectue entièrement en mémoire centrale tandis qu'un tris *externe* utilise des fichiers sur une mémoire de
masse pour trier des volumes trop importants pour pouvoir tenir en mémoire centrale.

Certains algorithmes permettent d'exploiter les capacités multitâches de la machine. Notons également, que certains algorithmes,
notamment ceux qui fonctionnent par insertion, peuvent être lancés sans connaître l'intégralité des données à trier; on peut
alors trier et produire les données à trier en parallèle.

### Exemples d'algorithmes de tri

#### Algorithmes rapides *T(n)=O(n.log n)*

* **Tri fusion** (*merge sort*) : Pour une entrée donnée, l'algorithme la divise en deux parties de tailles similaires, trie
chacune d'elles en utilisant le même algorithme, puis fusionne les deux parties triées. Il se prête aussi bien à des
implémentations sur listes que sur tableaux.
* **Tri rapide** (*quick sort*) : Une valeur est choisie comme pivot et les éléments plus petits que le pivot sont dissociés,
par échanges successifs, des éléments plus grands que le pivot ; chacun de ces deux sous-ensembles est ensuite trié de la même
manière. On peut rendre la complexité quasiment indépendante des données en utilisant un pivot aléatoire ou en appliquant au
tableau une permutation aléatoire avant de le trier.
* **Tri par tas** (*heap sort*) : Il s'agit d'une amélioration du tri par sélection. L'idée est la même (insérer les élément un
à un dans une structure déjà triée) mais l'algorithme utilise une structure de tas, souvent implémentée au moyen d'un tableau.
* **Tri introspectif** (*Introspective sort*) : Il s'agit d'un hybride du tri rapide et du tri par tas. Par rapport au tri
rapide, il présente l'avantage d'avoir une complexité *O(n.log n)* dans le pire cas.
* **Tri arborescent** (*tree sort*): L'idée est d'insérer les éléments un à un dans l'arbre binaire de recherche, puis de lire
l'arbre selon un parcours en profondeur. Un arbre binaire de recherche(ABR) est un arbre binaire dans lequel chaque noeud
possède une clé, telle que chaque noeud du sous-arbre *gauche* ait une clé inférieure ou égale à celle du noeud considéré, et
que chaque noeud du sous-arbre *droit* possède une clé supérieure ou égale à celle-ci.
* **Tri doux** (*smoothsort*) : La première étape consiste à transformer le tableau en arbre binaire. Le premier élément est
déjà trivialement bien ordonné, puis on ajoute un à un les éléments suivants. On réordonne chaque fois un peu les éléments si
nécessaire pour qu'ils correspondent aux critères :
    + Chaque noeud ne peut être supérieur à son noeud parent.
    + Le premier noeud enfant ne peut être supérieur au deuxième noeud enfant.

La deuxième étape consiste à retransformer l'arbre binaire en tableau trié. Chaque élément en partant de la droite est laissé
tel quel car il s'agit de la racine de l'arbre qui est déjà le plus grand élément, et l'arbre restant est réordonné si
nécessaire. On fait ceci jusqu'à arriver à un tableau trié.

#### Algorithmes moyennement rapides

* **Tri de Shell** (*shell sort*) : Ce tri repose sur le tri par insertion des sous-suites de l'entrée obtenues en prenant les
éléments espacés d'un pas constant, pour une suite de pas prédéfinie. La complexité varie selon le choix de cette suite.
* **Tri à peigne** (*comb sort*) : Il s'agit d'une variante plus efficace du tri à bulles, ne comparant pas uniquement des
éléments consécutifs. On peut dire qu'il est au tri à bulles ce que le tri de Shell est au tri par insertion.
* **Tri par insertion** (*insertion sort*): Ce tri souvent utilisé naturellement pour trier des cartes à jouer. Les valeurs sont
insérées les unes après les autres dans une liste triée (initialement vide). C'est souvent le plus rapide et le plus utilisé
pour trier les entrées de petite taille. Il est également efficace pour des entrées déjà presque triées.
* **Tri à bulles** (*bubble sort*) : L'algorithme consiste à parcourir l'entrée du début à la fin et pour chaque couple
d'éléments consécutifs, à les intervertir s'ils sont mal ordonnés. Cette opération est répétée jusqu'à ce que la structure soit
triée (aucune intervention lors du dernier passage). Cet algorithme est peu efficace et rarement utilisé en pratique ; son
intérêt est principalement pédagogique.
* **Tri cocktail** (*cocktail sort*) : Il s'agit d'une variante du tri à bulles dans laquelle l'entrée est alternativement
parcourue dans les deux sens. S'il permet de traiter de manière plus efficace quelques cas problématiques pour le tri à bulles,
il reste essentiellement similaire à ce dernier et l'intérêt est encore une fois principalement pédagogique.
* **Tri pair-impair** (*odd-even sort*) : Il s'agit d'une variante du tri à bulles, qui procède en comparant successivement tous
les éléments d'index pairs avec les éléments d'index impairs qui les suivent, puis inversement. On va ainsi commencer en
comparant le premier élément au second, le troisième au quatrième, etc., puis l'on comparera le second élément au troisième, le
quatrième au cinquième. L'opération est répétée jusqu'à ce que la structure soit triée.

#### Algorithmes lents

* **Tri par selection** (*selection sort*) : Sur un tableau de *n* éléments on recherche l'élément le plus petit du tableau et
on l'échange avec l'élément d'indice 0. Puis on recherche le deuxième plus petit et on l'échange avec l'élément d'indice 1.
L'opération est répétée jusqu'à ce que la structure soit triée.

## Structures de données

Une structure de données est une manière d'organiser les données pour les traiter plus facilement. C'est une mise en oeuvre
concrète d'un type abstrait.

### Pile

Une pile est une structure de données fondée sur le principe "dernier arrivé, premier sorti" (LIFO), ce qui veut dire qu'en
général, le dernier élément ajouté à la pile, sera le premier à en sortir.

La plupart des microprocesseurs gèrent nativement une pile. Elle correspond alors à une zone de la mémoire, et le processeur
retient l'adresse du dernier élément.

Voici les primitives communément utilisées pour manipuler les piles. Il n'existe pas de normalisation pour les primitives de
manipulation de pile. Leurs noms sont donc indiqués de manière informelle. Seules les trois premières sont réellement
indispensables, les autres pouvant s'en déduire :

* Empiler (*Push*) : ajoute un élément sur la pile.
* Dépiler (*Pull*) : enlève un élément de la pile et le renvoie.
* "La pile est-elle vide ?" (*IsNull*) : renvoie vrai si la pile est vide, faux sinon.
* "Nombre d'éléments de la pile" (*Length*): renvoie le nombre d'élément de la pile.
* "Quel est l'élément de tête ?" (*Peek* ou *Top*) : renvoie l'élément de tête sans le dépiler.
* "Vider la liste" (*Clear*) : dépiler tous les éléments.

### File

Une file est une structure de donnée basée sur le principe de "premier entré, premier sorti" (FIFO), les premiers éléments
ajoutés à la file seront les premiers à en être retirés.

Les files servent à organiser le traitement séquentiel des blocs de données d'origines diverses.

La théorie des files d'attente, élaborée pour le dimensionnement des réseaux téléphoniques, relie le nombre d'usagers, le nombre
de canaux disponibles, le temps d'occupation moyen du canal, et le temps d'attente à prévoir (Loi de Poisson).

Cette structure est utilisée :

* en général, pour mémoriser temporairement des transactions qui doivent attendre pour être traitées ;
* les serveurs d'impression, qui traitent ainsi les requêtes dans l'ordre dans lequel elles arrivent, et les insèrent dans une
file d'attente (spool) ; certains moteurs multitâches, dans les systèmes d'exploitation, qui doivent accorder du temps machine à
chaque tâche, sans en privilégier aucune ;
* un algorithme de parcours en largeur utilise une file pour mémoriser les noeuds visités ;
* pour créer toutes sortes de mémoires tampons (*buffers*) ;
* En gestion des stocks les algorithmes doivent respecter la gestion physique des stocks pour assurer la cohérence
physique/valorisation.

Voici les primitives communément utilisées pour manipuler les files. Il n'existe pas de normalisation pour les primitives de
manipulation de file. Leurs noms sont donc indiqués de manière informelle :

* Enfiler (*Enqueue*) : ajouter un élément dans la file.
* Defiler (*Dequeue*) : renvoie le prochain élément de la file, et le retire de la file.
* "La file est-elle vide ?" (*IsNull*) : renvoie "vrai" si la file est vide, "faux" sinon.
* "Nombre d'élément dans la file" (*Length*) : renvoie le nombre d'élément dans la file.

### Liste

Une liste est une structure de données permettant de regrouper des données de manière à pouvoir y accéder librement
(contrairement aux files et aux piles).

Voici les primitives communément utilisées pour manipuler des listes ; il n'existe pas de normalisation pour les primitives de
manipulation de listes, leurs noms respectifs sont donc indiqués de manière informelle.

Primitives de base :

* Insérer (*Add*) : ajoute un élément dans la liste ;
* Retirer (*Remove*) : retire un élément de la liste ;
* "La liste est-elle vide ?" (*IsNull*) : renvoie "vrai" si la liste est vide, "faux" sinon ;
* "Nombre d'éléments dans la liste" (*Length*) : renvoie le nombre d'éléments dans la liste.

Primitives auxiliaires fréquemment rencontrées :

* Premier (*First*) : retourne le premier élément dans la liste ;
* Dernier (*Last*) : retourne le dernier élément dans la liste ;
* Prochain (*Next*) : retourne le prochain élément dans la liste ;
* Précédent (*Previous*) : retourne l'élément qui précède dans la liste ;
* Cherche (*find*) : cherche si un élément précis est contenu dans la liste et retourne sa position.

Une liste est un conteneur d'éléments, où chaque élément contient la donnée, ainsi que d'autres informations permettant la
récupération des données au sein de la liste. La nature (les types) de ces informations caractérise un type différent de liste.

On peut distinguer, de manière générale, deux types de liste :

* les tableaux ;
* les listes chaînées.

Dans un tableau, l'accès à un élément se fait à l'aide d'un index qui représente l'emplacement de l'élément dans la structure.

Les données présentes dans un tableau sont contiguës en mémoire. Cela induit une taille de tableau fixe. Cependant certains
langages de haut niveau fournissent des tableaux qui modifient leur taille en fonction de leur utilisation : on parle alors de
tableau à taille dynamique. Mais leur implémentation utilise le principe des listes chaînées. Les tableaux peuvent également
avoir plusieurs dimensions, représentées par une séquence d'indices.

Contrairement à un tableau, la taille d'une liste chaînée n'a pas de limite autre que celle de la mémoire disponible. Cette
limitation est franchie par le fait que chaque élément peut pointer, suivant le type de liste chaînée, vers un ou plusieurs
éléments de la liste en utilisant une définition récursive. Ainsi, pour augmenter la taille d'une liste chaînée, il suffit de
créer un nouvel élément et de faire pointer certains éléments, déjà présents au sein de la liste, vers le nouvel élément.

Il existe deux grand types de liste chainée :

* les listes simplement chaînées : chaque élément dispose d'un pointeur sur l'élément suivant (ou successeur) de la liste. Le
parcours se fait dans un seul sens ;
* les listes doublement chaînées : chaque élément dispose de deux pointeurs, respectivement sur l'élément suivant (ou
successeur) et sur l'élément précédent (ou prédécesseur). Le parcours peut alors se faire dans deux sens, mutuellement opposés :
de successeur en successeur, ou de prédécesseur en prédécesseur.

A cela on peut ajouter une propriété : le cycle. Cette fois ci, la liste chaînée forme une boucle. Dès qu'on atteint la "fin" de
la liste et qu'on désire continuer, on se retrouve sur le "premier" élément de la liste. Dans ce cas, la notion de début ou de
fin de chaîne n'a plus de raison d'être.

### Arbre enraciné

En théorie de graphes, un arbre enraciné ou une arborescence est un graphe acyclique orienté possédant une unique racine, et tel
que tous les noeuds sauf la racine ont un unique parent.

Dans un arbre, on distingue deux catégories d'éléments :

* les *feuilles* (ou noeuds externes), éléments ne possédant pas de fils dans l'arbre ;
* les *noeuds* interne, éléments possédant des fils (sous-branches).

La *racine* de l'arbre est l'unique noeud ne possédant pas de parent. Les noeuds (les pères avec leurs fils) sont reliés entre
eux par une *arête*. Selon le contexte, un noeud peut désigner un noeud interne ou externe (feuille) de l'arbre.

La *profondeur* d'un noeud est la distance, i.e. le nombre d'arêtes, de la racine au noeud. La *hauteur* d'une arbre est la plus
grande profondeur d'une feuille de l'arbre. La *taille* d'un arbre est son nombre de noeuds (en comptant les feuilles ou non),
la longueur de cheminement est la somme des profondeurs de chacune des feuilles.

Les arbres peuvent être étiquetés. Dans ce cas, chaque noeud possède une *étiquette*, qui est en quelque sorte le "contenu" du
noeud. L'étiquette peut être très simple : un nombre entier, par exemple. Elle peut également être aussi complexe que l'on veut
: un objet, une instance d'une structure de donnée, un pointeur, etc. Il est presque toujours obligatoire de pouvoir comparer
les étiquettes selon une relation d'ordre total, afin d'implanter les algorithmes sur les arbres.

Les fichiers et dossiers dans un système de fichiers sont généralement organisés sous forme arborescente.

Les arbres sont en fait rarement utilisés en tant que tels, mais de nombreux types d'arbres avec une structure plus restrictive
existent et sont couramment utilisés en algorithmique, notamment pour gérer des bases de données, ou pour l'indexation de
fichiers. Ils permettent alors des recherches rapides et efficaces. Par exemple :
* Les arbres binaires dont chaque noeud a au plus deux fils : ils sont en fait utilisés sous forme d'arbres binaires de
recherche, de tas, ou encore d'arbres rouge-noir. Le dernier exemple est un cas particulier d'arbre équilibré, c'est à dire dont
les sous-branches ont presque la même hauteur.
* Les arbres n-aires qui sont une généralisation des arbres binaires : chaque noeud a au plus *n* fils. Les arbres 2-3-4 et les
arbres B en sont des exemples d'utilisation et sont eux aussi des arbres équilibrés.

Pour construire un arbre à partir de cases ne contenant que des informations, on peut procéder de l'une des trois façons
suivantes :

1. Créer une structure de données composée de :
    * l'étiquette (la valeur contenue dans le noeud),
    * un lien vers *chaque* noeud fils,
    * un arbre particulier, l'arbre vide, qui permet de caractériser les feuilles. Une feuille a pour fils des arbres vides
    uniquement.
2. Créer une structure de données composée de :
    * l'étiquette (la valeur contenue dans le noeud),
    * un lien vers le "premier" noeud fils (noeud fils gauche le cas échéant),
    * un autre lien vers le noeud frère (le "premier" noeud frère sur la droite le cas échéant).
3. Créer une structure de données composée de :
    * l'étiquette (la valeur contenue dans le noeud),
    * un lien vers le noeud père.

On note qu'il existe d'autres types de représentation propres à des cas particuliers d'arbres. Par exemple, le tas est
représenté par un tableau d'étiquettes.

Les parcours d'arbre sont des processus de visites des sommets d'un arbre, par exemple pour trouver une valeur.

Le **parcours en largeur** correspond à un parcours par niveau de noeuds de l'arbre. Un niveau est un ensemble de noeuds
internes ou de feuilles situés à la même profondeur - on parle aussi de noeud ou de feuille de même hauteur dans l'arbre
considéré. L'ordre de parcours d'un niveau donné est habituellement conféré, de manière récursive, par l'ordre de parcours des
noeuds parents - noeuds de niveau immédiatement supérieur.

Le **parcours en profondeur** est un parcours récursif sur un arbre. Dans le cas général, deux ordres sont possibles :

* Parcours en profondeur préfixe : dans ce mode de parcours, le noeud courant est traité avant ses descendants.
* Parcours en profondeur suffixe : dans ce mode de parcours, le noeud courant est traité après ses descendants.

Pour les arbres binaires, on peut également faire un *parcours infixe*, c'est à dire traiter le noeud courant entre les noeuds
gauche et droit.

*Parcours préfixe* :

    visiterPréfixe(Arbre A) :
        visiter (A)
        Si nonVide (gauche(A))
            visiterPréfixe(gauche(A))
        Si nonVide (droite(A))
            visiterPréfixe(droite(A))

*Parcours suffixe* :

    visiterSuffixe(Arbre A) :
        Si nonVide(gauche(A))
            visiterSuffixe(gauche(A))
        Si nonVide(droite(A))
            visiterSuffixe(droite(A))
        visiter(A)

*Parcours infixe* :

    visiterInfixe(Arbre A) :
        Si nonVide(gauche(A))
            visiterInfixe(gauche(A))
        visiter(A)
        Si nonVide(droite(A))
            visiterInfixe(droite(A))

### Arbre binaire de recherche

Un arbre binaire de recherche ou ABR (en anglais, binary search tree ou BST) est une structure de données représentant un
ensemble ou un tableau associatif dont les clefs appartiennent à un ensemble totalement ordonné. Un arbre binaire de recherche
permet des opérations rapides pour rechercher une clé, insérer ou supprimer une clé.

Un arbre binaire de recherche est un arbre binaire dans lequel chaque noeud possède une clé, telle que chaque noeud du
sous-arbre *gauche* ait une clé inférieure ou égale à celle du noeud considéré, et que chaque noeud du sous-arbre *droit*
possède une clé supérieure ou égale à celle-ci - selon la mise en oeuvre de l'ABR, on pourra interdire ou non des clés de valeur
égale. Les noeuds que l'on ajoute deviennent des feuilles de l'arbre.

La recherche dans un arbre binaire d'un noeud ayant une clé particulière est un procédé récursif. On commence par examiner la
racine. Si la clé est la clé recherchée, l'algorithme se termine et renvoie la racine. Si elle est strictement inférieure, alors
elle est dans le sous-arbre gauche, sur lequel on effectue alors récursivement la recherche. De même si la clé recherchée est
strictement supérieure à la clé de la racine, la recherche continue dans le sous-arbre droit. Si on atteint une feuille dont la
clé n'est pas celle recherchée, on sait alors que la clé recherchée n'appartient à aucun noeud, elle ne figure donc pas dans
l'arbre de recherche. On peut comparer l'exploration d'un arbre binaire de recherche avec la recherche par dichotomie qui
procède à peu près de la même manière sauf qu'elle accède directement à chaque élément d'un tableau au lieu de suivre les liens.
La différence entre les deux algorithmes est que, dans la recherche dichotomique, on suppose avoir un critère de découpage de
l'espace en deux parties que l'on n'a pas dans la recherche dans un arbre.

Cette opération requiert un temps en *O(log n)* dans le cas moyen, mais *O(n)* dans le cas critique où l'arbre est complètement
déséquilibré et ressemble à une liste chaînée. Ce problème est écarté si l'arbre est équilibré par rotation au fur et à mesure
des insertions pouvant créer des listes trop longues.

L'insertion d'un noeud commence par une recherche : on cherche la clé du noeud à insérer ; lorsqu'on arrive à une feuille, on
ajoute le noeud comme fils de la feuille en comparant sa clé à celle de la feuille : si elle est inférieure, le nouveau noeud
sera à gauche ; sinon il sera à droite.

Sa complexité est la même que pour la recherche.

Il est aussi possible d'écrire une procédure d'ajout d'élément à la racine d'un arbre binaire. Cette opération requiert la même
complexité mais est meilleure en terme d'accès aux éléments.

Pour la suppression, on commence par rechercher la clé du noeud à supprimer dans l'arbre. Plusieurs cas sont à considérer, une
fois que le noeud à supprimer a été trouvé à partir de sa clé :

* *Suppression d'une feuille* : Il suffit de l'enlever de l'arbre puisqu'elle n'a pas de fils.
* *Suppression de noeud avec un enfant* : Il faut l'enlever de l'arbre en le remplaçant par son fils.
* *Suppression d'un noeud avec deux enfants* : Supposons que le noeud à supprimer soit appelé N. On échange le noeud N avec son
successeur le plus proche (le noeud le plus à gauche du sous-arbre droit) ou son plus proche prédécesseur (le noeud le plus à
droite du sous-arbre gauche). Cela permet de garder à la fin de l'opération une structure d'arbre binaire de recherche. Puis on
applique à nouveau la procédure de suppression à N, qui est maintenant une feuille ou un noeud avec un seul fils.

Ce choix d'implémentation peut contribuer à déséquilibrer l'arbre. En effet, puisque ce sont toujours des feuilles du sous-arbre
gauche qui sont supprimées, une utilisation fréquente de cette fonction amènera à un arbre plus lourd à droite qu'à gauche. On
peut remédier à cela en alternant successivement la suppression du minimum du fils droit avec celle maximum du fils gauche,
plutôt que toujours choisir ce dernier. Il est par exemple possible d'utiliser un facteur aléatoire : le programme aura une
chance sur deux de choisir le fils droit et une chance sur deux de choisir le fils gauche.

Dans tous les cas cette opération requiert de parcourir l'arbre de la racine jusqu'à une feuille : le temps d'exécution est donc
proportionnel à la profondeur de l'arbre qui vaut n dans le pire des cas, d'où une complexité maximale *O(n)*.

On peut récupérer les clés d'un arbre binaire de recherche dans l'ordre croissant en réalisant un parcours infixe. Il faut
concaténer dans cet ordre la liste triée obtenue récursivement par parcours du fils gauche à la racine puis à celle obtenue
récursivement par parcours du fils droit. Il est possible de le faire dans l'ordre inverse en commençant par le sous-arbre
droit. Le parcours de l'arbre se fait en temps linéaire, puisqu'il doit passer par chaque noeud une seule fois.

On peut dès lors créer un algorithme de tri simple mais peu efficace, en insérant toutes les clés que l'on veut trier dans un
nouvel arbre binaire de recherche puis en parcourant de manière ordonnée cet arbre comme ci-dessus.

Les arbres binaires de recherche peuvent servir d'implémentation au type abstrait de file de priorité. En effet, les opérations
d'insertion d'une clé et de test au vide se font avec des complexités avantageuses (respectivement en *O(log n)* et en *O(1)*).
Pour l'opération de suppression de la plus grande clé, il suffit de parcourir l'arbre depuis sa racine en choisissant le fils
droit de chaque noeud, et de supprimer la feuille terminale. Cela demande un nombre d'opérations égal à la hauteur de l'arbre,
donc une complexité logarithmique. L'avantage notoire de cette représentation d'une file de priorité est qu'avec un processus
similaire, on dispose d'une opération de suppression de la plus petite clé en temps logarithmique également.

L'insertion et la suppression s'exécutent en *O(h)* où *h* est la hauteur de l'arbre. Cela s'avère particulièrement coûteux
quand l'arbre est très déséquilibré (un arbre peigne par exemple, dont la hauteur est linéaire en le nombre de clés), et on
gagne donc en efficacité à équilibrer les arbres au cours de leur utilisation. Il existe des techniques pour obtenir des arbres
équilibrés, c'est à dire une hauteur logarithmique en nombre d'éléments :

* les arbres rouge-noir
* les B-arbres


### Arbre B

Un arbre B est une structure de données en arbre équilibré. Les arbres B sont principalement mis en oeuvre dans les mécanismes
de gestion de bases de données et de systèmes de fichiers. Ils stockent les données sous une forme triée et permettent une
exécution des opérations d'insertion et de suppression en temps toujours logarithmique.

Le principe est de permettre aux noeuds parents de posséder plus de deux noeuds enfants : c'est une généralisation de l'arbre
binaire de recherche. Ce principe minimise la taille de l'arbre et réduit le nombre d'opérations d'équilibrage. De plus un
B-arbre grandit à partir de la racine, contrairement à un arbre binaire de recherche qui croît à partir des feuilles.

Un *arbre étiqueté* est un arbre tel qu'à chaque noeud on associe une étiquette ou clé (ou bien plusieurs étiquettes ou clés
dans le cas des arbres B) prise(s) dans un ensemble donné. Donc formellement un arbre étiqueté est un couple formé d'un graphe
orienté, acyclique et connexe et d'une fonction d'étiquetage des arbres qui attribue à chaque noeud une étiquette ou une clé.
Parmi les arbres étiquetés, un *arbre B* possède quelques propriétés spécifiques supplémentaires.

Soit *L* et *U* deux entiers naturels non nuls tels que *L&#8804;U*. En toute généralité, on définit alors un *L-U arbre B* de
la manière suivante : chaque noeud, sauf la racine, possède un minimum de *L-1* clés (appelées aussi éléments), un maximum de
*U-1* clés et au plus *U* fils. Pour chaque noeud interne -- noeud qui n'est pas une feuille --, le nombre de fils est toujours
égal au nombre de clés augmenté d'une unité. Si *n* est le nombre de fils, alors on parle de *n*-noeud. Un *L-U* arbre B ne
contient que des *n*-noeuds avec L&#8804;n&#8804;U. Souvent on choisit la configuration *L=t* et *U=2.t* : *t* est appelé le
*degré minimal* de l'arbre B.

De plus, la construction des arbres B garantit qu'un arbre B est toujours équilibré. Chaque clé d'un noeud interne est en fait
une borne qui distingue les sous-arbres de ce noeud.

Un arbre B est implémenté par un arbre enraciné. Un noeud *x* est étiqueté par :

* Un entier *n* qui correspond au nombre de clefs contenues dans e noeud *x*
* *n* clefs notées *c&#8321;,...,c&#8345;*.
* Un booléen indiquant si *x* est une feuille ou non.
* *n+1* pointeurs notés *p&#8321;,...,p&#8345;&#8330;&#8321;* associés aux fils *f&#8321;,...,f&#8345;&#8330;&#8321;* de *x*.
Une feuille ne contient pas de pointeurs.

De plus, un arbre B vérifie ces propriétés :

* Toutes les feuilles ont la même profondeur, à savoir la hauteur *h* de l'arbre.
* Si *x* n'est pas une feuille :
    + pour *2&#8804;i&#8804;n*, pour toute clef *k* du fils *f&#7522;* : *c&#7522;&#8331;&#8321;&#8804;k&#8804;c&#7522;*.
    + pour toute clef *k* du fils *f&#8321;* : *k&#8804;c&#8321;*.
    + pour toute clef *k* du fils *f&#8345;&#8330;&#8321;* : *c&#8345;&#8804;k*.

* Si *x* n'est ni une feuille, ni la racine, *n* est compris entre L-1 et U-1.

La plupart du temps, la configuration est telle que *U = 2L*. On parle alors d'arbre B d'ordre *L*.
Un arbre B d'ordre *t* est défini alors plus simplement par un arbre qui satisfait les propriétés suivantes :

* Chaque noeud a au plus *2t-1* clés.
* Chaque noeud qui n'est ni racine ni feuille possède au moins *t-1* clés.
* Si l'arbre est non vide, la racine est aussi non vide.
* Un noeud qui possède *k* fils contient *k-1* clefs.
* Toutes les feuilles se situent à la même hauteur.

Comme on le verra par la suite, la hauteur d'un B-arbre est logarithmique en le nombre d'éléments. Ainsi, les opérations de
recherche, insertion et suppression sont implémentables en *O(log n)* dans le pire des cas, où *n* est le nombre d'éléments.

La recherche est effectué de la même manière que dans un arbre binaire de recherche. Partant de la racine, on parcourt
récursivement l'arbre ; à chaque noeud, on choisit le sous-arbre fils dont les clés sont comprises entre les même bornes que
celles de la clé recherchée grâce à une recherche dichotomique.

L'insertion nécessite tout d'abord de chercher le noeud où la nouvelle clé devrait être insérée, et l'insérer. La suite se
déroule récursivement, selon qu'un noeud ait ou non trop de clés : s'il possède un nombre acceptable de clés, on ne fait rien ;
autrement on le transforme en deux noeuds, chacun possédant un nombre minimum de clés, puis on fait "remonter" la clé du milieu
qui est alors insérée dans le noeud père. Ce dernier peut du coup se retrouver avec un nombre excessif de fils ; le procédé se
poursuit ainsi jusqu'à ce qu'on atteigne la racine. Si celle-ci doit être divisée, on fait "remonter" la clé du milieu dans une
nouvelle racine, laquelle génèrera comme noeuds fils les deux noeuds créés à partir de l'ancienne racine, à l'instar de l'étape
précédente. Pour que l'opération soit possible, on remarque qu'il faut que U &#8805; 2L ; sinon les nouveaux noeuds ne
possèderont pas suffisamment de clés.

Une variante consiste à éclater préventivement chaque noeud "plein" (possédant un nombre maximal de clés) rencontré lors de la
recherche du noeud où se réalisera l'insertion. De cette manière on évite une remontée dans l'arbre, puisque l'on s'assure que
le père d'un noeud à scinder en deux peut accueillir une clé supplémentaire. La contrepartie en est une légère augmentation de
la hauteur moyenne de l'arbre.

Pour la suppression, on doit d'abord chercher la clé à supprimer et la supprimer du noeud qui la contient.

* Si le noeud est interne, on procède de manière similaire aux arbres binaires de recherche en cherchant la clé *k* la plus à
gauche dans le sous-arbre droit de la clé à supprimer ou la plus à droite dans le sous-arbre gauche. Cette clé *k* appartient à
une feuille. On peut la permuter avec la clé à supprimer, que l'on supprime ensuite. Comme elle appartient à une feuille, on se
ramène au cas suivant.
* Si le noeud est une feuille, soit il possède encore suffisamment de clés et l'algorithme se termine, soit il dispose de moins
de *L-1* clés et on se trouve dans l'une des deux situations suivantes :
    + soit un de ses frères à droite ou à gauche possède suffisamment de clés pour pouvoir en "passer" une à la feuille en
    question : dans ce cas cette clé remplace la clé qui sépare les deux sous-arbres dans l'arbre père, qui va elle-même dans la
    feuille en question ;
    + soit aucun de ses frères n'a suffisamment de clés : dans ce cas, le père fait passer une de ses clés dans un des deux (ou
    le seul) frère pour permettre à la feuille de fusionner avec celui-ci. Ceci peut cependant conduire le père à ne plus avoir
    suffisamment de clés. On réitère alors l'algorithme : si le noeud a un frère avec suffisamment de clés, la clé la plus
    proche va être échangée avec la clé du père, puis la clé du père et ses nouveaux descendants sont ramenés dans le noeud qui
    a besoin d'une clé ; sinon, on effectue une fusion à l'aide d'une clé du père et ainsi de suite. Si l'on arrive à la racine
    et qu'elle possède moins de L éléments, on fusionne ses deux fils pour donner une nouvelle racine.

Notamment après une suppression, l'arbre peut être rééquilibré. Cette opération consiste à répartir équitablement les valeurs
dans les différents noeuds de l'arbre et à rétablir les propriétés de remplissage minimum des noeuds.

Le rééquilibrage commence au niveau des feuilles et progresse vers la racine, jusqu'à celle-ci. La redistribution consiste à
transférer un élément d'un noeud voisin qui possède suffisamment de valeurs vers le noeud qui en manque. Cette redistribution
est appelée **rotation**. Si aucun voisin ne peut fournir de valeur sans être lui même sous la limite, le noeud déficient doit
être **fusionné** avec un voisin. Cette opération provoque la perte d'un séparateur dans le noeud parent, celui-ci peut alors
être en déficit et a besoin d'être équilibré. La fusion et redistribution se propage jusqu'à la racine, seul élément où la
déficience en valeurs est tolérée.

Un algorithme d'équilibrage simple consiste à :

* Si le noeud voisin gauche existe et dispose de suffisamment de valeurs pour pouvoir en offrir une, réaliser une rotation
gauche.
* Sinon, si le noeud voisin droite existe et dispose de suffisamment d'éléments, réaliser une rotation droite.
* Sinon, le noeud déficient doit être fusionné avec un de ses voisins tel que la somme du nombre de leurs clés plus *1* soit
inférieure ou égale à la capacité maximale (*taille_gauche*+*taille_droite*+1&#8804;*U-1*). La valeur supplémentaire correspond
au séparateur présent dans le parent. Cette opération est toujours possible si *U-1*&#8805;*2L* avec *taille_gauche*=*L-2* et
*taille_droite*=*L-1* ou le contraire, soit un noeud immédiatement sous la limite de *L-1* clés et un noeud exactement à la
limite.

1. Copier le séparateur à la fin du noeud de gauche.
2. Ajouter tous les éléments du noeud de droite à la fin du noeud de gauche.
3. Effacer le noeud de droite et effacer le séparateur du parent puis vérifier qu'il contient assez d'éléments. Si ce n'est pas
le cas, rééquilibrer le parent avec ses voisins.

La rotation à gauche d'un cran entre deux noeuds voisins se fait en :

1. déplaçant le séparateur, présent dans le parent, à la fin du noeud gauche.
2. déplaçant le premier élément du noeud de droite en tant que séparateur dans le parent.

Ce genre d'opération peut être également utilisé pour compresser l'arbre : un arbre destiné à la lecture seule peut être vidé
d'un maximum de cases mémoires inutilisées en remplissant au maximum un minimum de noeuds.

### Arbre rouge-noir

Un **arbre bicolore**, ou **arbre rouge-noir** ou **arbre rouge et noir** est un type particulier d'arbre binaire de recherche
équilibré. Chaque noeud de l'arbre possède en plus de ses données propres un attribut binaire qui est souvent interprété comme
sa "couleur" (rouge ou noir). Cet attribut permet de garantir l'équilibre de l'arbre : lors de l'insertion ou de la suppression
d'éléments, certaines propriétés sur les relations entre les noeuds et les couleurs doivent être maintenues, ce qui empêche
l'arbre de devenir trop déséquilibré, y compris dans le pire des cas. Durant une insertion ou une suppression, les noeuds sont
parfois réarrangés ou changent leur couleur afin que ces propriétés soient conservées.

Le principal intérêt des arbres bicolores réside dans le fait que malgré les potentiels réarrangements ou coloriages des noeuds,
la complexité (en le nombre d'éléments) des opérations d'insertion, de recherche et de suppression est logarithmique. De plus,
cette structure est économe en mémoire puisqu'elle ne requiert qu'un bit supplémentaire d'information par élément par rapport à
un arbre binaire classique.

Un arbre bicolore est un cas particulier d'arbre binaire, une structure de donnée couramment utilisée en informatique pour
organiser des données pouvant être comparées, par exemple des nombres ou des chaines de caractères.

Les feuilles de l'arbre, c'est-à-dire les noeuds terminaux, ne contiennent aucune donnée. Elles peuvent être simplement
représentées sans coût mémoire par des éléments nuls (pointeurs nul en C, valeur NIL, etc.) dans le noeud parent (indiquant que
le noeud enfant est une feuille). Il peut être toutefois utile pour simplifier la mise en oeuvre de certains algorithmes que les
feuilles soient explicitement représentées soit en les instanciant séparément, soit en utilisant une sentinelle (valeur
signifiant la fin de la donnée).

Comme tous les arbres binaires de recherche, les arbres bicolores peuvent être parcourus très efficacement en ordre infixe (ou
ordre gauche - racine - droite), ce qui permet de lister les éléments dans l'ordre. La recherche d'un élément se fait en temps
logarithmique *O(log n)*, *n* étant le nombre d'éléments de l'arbre, y compris dans le pire des cas.

Un arbre bicolore est un arbre binaire de recherche dans lequel chaque noeud a un attribut supplémentaire : sa couleur, qui est
soit **rouge** soit **noire**. En plus des restrictions imposées aux arbres binaires de recherche, les règles suivantes sont
utilisées :

1. Un noeud est soit rouge soit noir ;
2. La racine est noire ;
3. Les enfants d'un noeud rouge sont noirs ;
4. Tous les noeuds ont 2 enfants. Ce sont d'autres noeuds ou des feuilles **NIL**, qui ne possèdent pas de valeurs et qui sont
les seuls noeuds sans enfants. Leur couleur est toujours **noire** et rentre donc en compte lors du calcul de la hauteur noire.
5. Le chemin de la racine à n'importe quelle feuille (**NIL**) contient le même nombre de noeuds noirs. On peut appeler ce
nombre de noeuds noirs la **hauteur noire**.

Ces contraintes impliquent une propriété importante des arbres bicolores : le chemin le plus long possible d'une racine à une
feuille (sa hauteur) ne peut être que deux fois plus long que le plus petit possible : dans le cas le plus déséquilibré, le plus
court des chemins ne comporte que des noeuds noirs, et le plus long alterne les noeuds rouges et noirs. Un arbre vérifiant ces
propriétés est ainsi presque équilibré. Comme les opérations d'insertion, de recherche et de suppression requièrent dans le pire
des cas un temps proportionnel à la hauteur de l'arbre, les arbres bicolores restent efficaces, contrairement aux arbres
binaires de recherche ordinaires.

Pour comprendre comment ces contraintes garantissent la propriété ci-dessus, il suffit de s'apercevoir qu'aucun chemin ne peut
avoir deux noeuds rouges consécutifs à cause de la propriété 3. Le plus petit chemin théorique de la racine à une feuille ne
contient alors que des noeuds noirs tandis que le plus grand alterne entre les noeuds rouges et noirs. Et comme d'après la
propriété chacun de ces chemins contient le même nombre de noeuds noirs, le plus grand chemin ne peut être deux fois plus grand
que le plus petit.

La propriété 2 n'est pas nécessaire. Les seuls cas où la racine pourrait devenir rouge étant les deux cas où sa couleur n'a pas
d'importance : soit la racine est le seul noeud, soit elle possède deux fils noirs. Cette propriété est ajouté uniquement pour
visualiser plus rapidement l'isomorphisme avec les arbres 2-3-4 : chaque noeud noir et ses éventuels fils rouges représente un
noeud d'arbre 2-3-4.

Les arbres bicolores, offrent la meilleure garantie sur le temps d'insertion, de suppression et de recherche dans les cas
défavorables. Ceci leur permet non seulement d'être alors utilisables dans des applications en temps réel, mais aussi de servir
comme fondement d'autres structures de données à temps d'exécution garanti dans les cas défavorables, par exemple en géométrie
algorithmique. L'ordonnanceur du noyau Linux, le Completely Fair Scheduler utilise également un arbre rouge-noir.

Les arbres rouge-noir sont également très utile en programmation fonctionnelle : c'est l'exemple le plus couramment utilisé de
structure de données persistante qui peut être utilisée pour construire des tableaux associatifs capables de garder en mémoires
les versions précédentes après un changement. Les versions persistantes des arbres rouge-noir requièrent *O(log n)* en mémoire
supplémentaire pour chaque insertion ou suppressions.

La recherche sur un arbre bicolore s'effectue exactement comme dans les arbres binaires de recherche. Cependant, après une
insertion ou une suppression, les propriétés de l'arbre bicolore peuvent être violées. La restauration de ces propriétés
requiert un petit nombre *O(log n)* de modifications des couleurs (qui sont très rapides en pratique) et pas plus de trois
rotations (deux pour l'insertion). Ceci permet d'avoir une insertion et une suppression en *O(log n)* mais rend l'implémentation
plus complexe à cause du grand nombre de cas particuliers à traiter.

La recherche d'un élément se déroule de la même façon que pour un arbre binaire de recherche : en partant de la racine, on
compare la valeur recherchée à celle du noeud courant de l'arbre. Si ces valeurs sont égales, la recherche est terminée et on
renvoie le noeud courant. Sinon, on choisit de descendre vers le noeud enfant gauche ou droit selon que la valeur recherchée est
inférieure ou supérieure. Si une feuille est atteinte, la valeur recherchée ne se trouve pas dans l'arbre.

La couleur des noeuds de l'arbre n'intervient pas directement dans la recherche. Toutefois à la différence d'un arbre binaire de
recherche normal, les arbres rouge-noir garantissent par construction un temps d'exécution de la recherche en *O(log n)* y
compris dans le pire des cas. En effet, un arbre binaire de recherche peut devenir déséquilibré dans les cas défavorables (par
exemple si les éléments sont insérés dans l'ordre croissant, l'arbre binaire de recherche dégénère en une liste chainée. La
complexité de l'opération dans le pire des cas est donc *O(n)* pour un arbre binaire potentiellement non équilibré. Au
contraire, pour l'arbre rouge-noir, les propriétés bicolores vues ci-dessus garantissent que l'on atteindra un noeud en au plus
*2.log n* comparaisons, donc en *O(log n)* opérations.

L'insertion commence de la même manière que sur un arbre binaire classique : en partant de la racine, on compare la valeur
insérée à celle d'un noeud courant de l'arbre, et on choisit de descendre vers le noeud enfant gauche ou droit selon que la
valeur insérée est inférieure ou supérieure. Le nouveau noeud est inséré lorsque l'on ne peut plus descendre, c'est-à-dire quand
le noeud courant est une feuille de l'arbre. Cette feuille est remplacée par le nouveau noeud.

Une fois le nouveau noeud ajouté à l'arbre, il faut vérifier que les propriétés de l'arbre bicolore sont bien respectées et,
dans le cas contraire, effectuer des opérations de changement de couleur et des rotations pour les établir. Le noeud inséré est
initialement colorié en *rouge*. Il y a ensuite plusieurs cas possibles pour rétablir les propriétés de l'arbre, à partir du
noeud inséré.

1. Le noeud inséré n'a pas de parent : il est en fait à la racine de l'arbre. La seule correction à apporter consiste à le
colorier en **noir** pour respecter la propriété 2.
2. Le noeud du parent inséré est **noir**, alors, l'arbre est valide : la propriété 3 et vérifiée, et la hauteur-noire de
l'arbre est inchangée puisque le nouveau noeud est **rouge**. Il n'y a donc rien d'autre à faire.
3. Le parent du noeud inséré est **rouge**, alors la propriété 3 est invalide. L'action à effectuer dépend de la couleur de
*l'oncle* du noeud inséré, c'est à dire le "frère" du parent du noeud inséré. En d'autres termes : en partant du noeud inséré
(N), on considère son noeud parent (P), puis le noeud parent de P, ou grand parent (G), et enfin l'oncle (U) qui est le fils de
G qui n'est pas P. Si l'oncle est **rouge**, alors le parent et l'oncle sont coloriés en **noir**, et le grand parent (qui était
nécessairement noir) est colorié en **rouge**. Ce changement de couleur a pu toutefois créer une nouvelle violation des
propriétés bicolores plus haut dans l'arbre. Il faut maintenant recommencer la même analyse de cas mais cette fois *en partant
du noeud grand parent ainsi colorié en rouge*.
4. Dans le cas où l'oncle est **noir**, il faut effectuer des rotations qui dépendent de la configuration du noeud inséré autour
de son parent et de son grand parent, afin de ramener l'équilibre dans l'arbre. Le parent vient prendre la place du grand
parent, et le grand parent celle de l'oncle. Le parent devient **noir** et le grand parent **rouge** et l'arbre respecte alors
les propriétés bicolores.

Le seul cas où la correction ne se termine pas immédiatement est le cas 3, dans lequel on change le grand parent de noir à
rouge, ce qui oblige à effectuer une nouvelle vérification du grand parent. Cependant, il est aisé de vérifier que la fonction
se termine toujours. Puisque le noeud à vérifier est toujours strictement plus haut que le précédent, on finira inévitablement
par se retrouver dans l'un des cas non récursifs (dans le pire des cas, on remontera jusqu'à atteindre la racine de l'arbre,
c'est à dire le cas 1). Il y aura donc au plus deux rotations, et un nombre de changements de couleurs inférieur à la moitié de
la hauteur de l'arbre, c'est à dire en *O(log n)*. En pratique la probabilité de tomber plusieurs fois de suite sur le cas 3 est
exponentiellement décroissante ; en moyenne le coût de la correction des propriétés est donc presque constant.

La suppression commence par une recherche du noeud à supprimer, comme dans un arbre binaire classique.

On notera qu'on peut toujours se mettre dans le cas où le noeud à supprimer a *au plus* un enfant qui ne soit pas une feuille.
Dans le cas où le noeud à retirer aurait deux enfants qui ne sont pas des feuilles, on recherche soit le plus grand élément du
sous-arbre gauche (c'est à dire l'élément précédent immédiatement le noeud à supprimer dans l'ordre de l'arbre) soit le plus
petit élément du sous-arbre droit (c'est à dire le successeur immédiat). La valeur du noeud à supprimer est remplacée par celle
du prédécesseur ou du successeur, et c'est ce dernier noeud dont on vient de recopier la valeur qui est supprimé. On notera que
la copie d'une valeur n'altère pas les propriétés bicolores de l'arbre. Le noeud qui sera effectivement supprimé de l'arbre aura
donc au plus un seul enfant qui ne soit pas une feuille. On note M le noeud à supprimer. M a donc soit un enfant non-feuille
(noté C) soit aucun (dans ce cas, on choisit l'une des feuilles pour C). Après la suppression, la position qu'occupait M dans
l'arbre sera occupée par C. Dans le cas le plus simple, le noeud supprimé M est **rouge** : il suffit de le remplacer par son
enfant C qui est nécessairement **noir** en vertu de la propriété n°3. En retirant M, on ne change pas la hauteur-noire de
l'arbre, donc les propriétés restent toutes respectées. Un autre cas simple se produit si le noeud supprimé M est **noir** mais
que son enfant C est **rouge**.En supprimant M, on diminue la hauteur-noire de l'arbre, ce qui violerait la propriété 5. De plus
le parent P de M pourrait être rouge : or C va remplacer M comme fils de P, ce qui pourrait également violer la propriété n°3.
On restaure ces propriétés simplement en coloriant C en **noir**. Le cas le plus compliqué se produit si le noeud supprimé M et
son enfant C sont tous les deux **noirs**.

### Tas

Un tas est une structure de données de type arbre tel que pour tous noeuds A et B de l'arbre tels que B soit un fils de A :
clé(A)&#8805;clé(B) (ou inversement). Les primitives du tas sont : enfiler et defiler.

Pour enfiler un élément, on le place comme feuille, puis on fait "remonter" l'élément pour maintenir la priorité du tas.
L'opération peut être réalisée en *O(log n)*.

Quand on défile un élément d'un tas, c'est toujours celui de priorité maximale. Il correspond donc à la racine du tas.
L'opération peut conserver la structure de tas, avec une complexité de *O(log n)* ; en effet, il ne reste alors qu'à réordonner
l'arbre privé de sa racine pour en faire un nouveau tas.

### Table de hachage

Une table de hachage est une structure de données qui implémente un type abstrait de tableau associatif. Une table de hachage
utilise une fonction de hachage afin de calculer l'index d'un tableau d'emplacements desquels la valeur attendue peut être
trouvée. Pendant la recherche, la clef est hachée et le hash résultant indique l'emplacement de la valeur. Durant la recherche,
la clef est hachée et le hash résultant indique où la valeur correspondante est stockée.

Idéalement, la fonction de hachage assignera chaque clef à un réceptacle unique, mais la plupart des tables de hachage utilisent
des fonctions de hachages imparfaites, qui peuvent causer des collisions quand la fonction de hachage attribue un même index
pour plus d'une clef. Ces collisions sont alors traitées de diverses manières.

Dans une table de hachage bien dimensionnée, le coût moyen (nombre d'instructions) pour chaque recherche est indépendant du
nombre d'élément stockés dans la table. Beaucoup de tables de hachages permettent également des insertions et des suppressions
arbitraires de paires clef-valeur, à un coût (amorti) moyen constant par opération.

Dans de nombreuses situations, les tables de hachage sont en moyenne plus efficaces que des arbres de recherche ou de n'importe
quelle autre structure de table de recherche. C'est pour cette raison, qu'elles sont largement utilisé dans un large panel de
logiciels informatiques, particulièrement les tableaux associatifs, l'indexation des bases de données, les caches, et les
ensembles.

L'idée du hachage est de distribuer les entrées (paires clefs/valeurs) à travers un tableau de réceptacles. Étant donné une
clef, l'algorithme calcule un index qui suggère où l'entrée peut se trouver :

    index = f(clef, taille_tableau)

Souvent cette opération est réalisée en deux étapes :

    hash = hashfunc(key)
    index = hash % array_size

Avec cette méthode, le hash est indépendant de la taille du tableau, et est réduit à postériori à un index (un nombre entre *0*
et *taille_tableau - 1*) à l'aide de l'opérateur modulo (*%*).

Dans le cas où la taille du tableau est une puissance de 2, l'opération de reste est réduite à un masquage, ce qui améliore la
performance, mais aussi fait croître le nombre de problèmes si la fonction de hachage est mauvaise.

Une condition basique pour la fonction est de fournir une distribution uniforme des valeurs de hash. Une distribution
non-uniforme augmente le nombre de collision est le coût pour les résoudre. L'uniformité est quelques fois difficile à garantir,
mais elle peut être évaluée de manière empirique via des tests statistiques.

La distribution doit être uniforme uniquement pour les tailles de tables de l'application. Si l'application utilise un
redimensionnement dynamique de la table avec un doublement ou une division par deux de la taille, alors la fonction de hachage
doit être uniforme uniquement lorsque la taille est une puissance de deux. Ici l'index peut être calculé comme un intervalle de
bits de la fonction de hachage. D'un autre côté, des algorithmes de hachage préfèrent avoir une taille exprimé à l'aide d'un
nombre premier. L'opération modulo peut fournir un petit mélange additionnel ; ceci est appréciable notamment pour une mauvaise
fonction de hachage.

Pour les schémas d'adressage ouvert, la fonction de hachage doit également éviter le *clustering*, l'adressage de deux ou de
plusieurs clefs à des emplacements consécutifs. Un tel regroupement peut être la cause d'une envolée du coût de recherche, même
si le facteur de charge est bas et que les collisions sont rares.

Les fonctions de hachages cryptographiques seraient capable de fournir de bonnes fonctions de hachages quelque soit la taille de
la table, soit par réduction modulo ou par masquage de bit. Elles peuvent également être utiles si il existe un risque
d'utilisateurs malveillants essayant de saboter un service réseau en envoyant des requêtes destinées à générées un grand nombre
de collisions dans les tables de hachage du serveur. Néanmoins, le risque de sabotage peut également être évité par des méthodes
moins coûteuses (tels qu'appliquer un sel secret à la donnée, ou utiliser une fonction de hachage universelle). Un inconvénient
des fonctions de hachages cryptographiques est qu'elles sont souvent plus lentes à être calculées, ce qui veut dire que dans
certains cas où l'uniformité pour n'importe quelle taille n'est pas nécessaire, une fonction de hachage non-cryptographique peut
être préférable.

Un hachage k-indépendant offre un moyen de prouver qu'une fonction de hachage n'a pas de mauvais ensembles de clefs pour un type
de table de hachage donné. De nombreux résultats de ce type sont connus pour des schémas de résolution de collision.

Si toutes les clefs sont connues à l'avance, une fonction de hachage parfaite peut être utilisée pour créer une table de hachage
parfaite sans aucune collision. Si un hachage parfait minimal est utilisé, chaque emplacement dans la table de hachage peut
également être utilisé.

Un hachage parfait permet un temps de recherche constant dans tout les cas. Ceci se détache de la majorité des méthodes de
chaînage et d'adressage ouvert, où le temps de recherche est bas en moyenne, mais peut être très grand, *O(n)*, par exemple
lorsque toutes les clefs sont hachées vers trop peu de valeurs.

Une statistique critique pour une table de hachage est le facteur de charge, défini comme :

facteur de charge = n/k,

où
* *n* est le nombre des entrées occupées dans la table de hachage,
* *k* est le nombre d'emplacements.

Quand le facteur de charge augmente, la table de hachage devient plus lente, et peut même cesser de fonctionner (en fonction de
la méthode utilisée). Le propriété attendue de temps constant pour une table de hachage suppose que le facteur de charge soit
gardé en deçà d'une certaine limite. Pour un nombre d'emplacement fixe, le temps pour une recherche augmente avec le nombre
d'entrées, par conséquent, le temps constant désiré n'est pas atteint. Dans plusieurs implémentations, la solution est de faire
croître automatiquement (généralement, doubler) la taille de la table lorsque la limite du facteur de charge est atteinte,
forçant de fait, un nouveau hachage de l'ensemble des entrées.

Après le facteur de charge, on peut examiner la variance du nombre d'entrées par emplacement.

Un facteur de charge bas n'est pas forcément bénéfique. Quand le facteur de charge approche 0, la proportion des aires
inutilisées dans la table de hachage augmente, mais il n'y a pas nécessairement une réduction dans le coût de recherche.

Les collision de hash sont pratiquement inévitables quand on hache un sous-ensemble aléatoire d'un large ensemble de clefs
possibles.

Par conséquent, presque toutes les implémentations de tables de hachages ont des stratégies de résolution de collisions afin de
gérer celles-ci. Toutes ces méthodes nécessitent que les clefs soient stockées dans la table, ensemble avec leurs valeurs
associées.

## Algorithmes de chiffrements

### Chiffrement symétrique AES

Le standard de chiffrement avancé (AES), aussi connu sous son nom originel Rijndael, est une spécification pour le chiffrement
des données électroniques établi par l'institut national des standards et technologies (NIST) en 2001.

AES est un sous-ensemble du chiffrement par bloc Rijndael. Rijndael est une famille de chiffres comprenant différentes clefs et
différentes tailles de blocs. Pour AES, NIST a sélectionné trois membres de la famille Rijndael, chacun ayant une taille de bloc
de 128 bits, mais trois longueurs de clefs différentes : 128, 192 et 256 bits.

AES est basé sur un principe simple connu sous le nom de réseau de substitution-permutation, et est efficace tant au niveau
logiciel que matériel.

AES opère sur un tableau de bits ordonné 4 x 4. La plupart des calculs AES sont effectués dans un champ fini particulier.

La taille de clef utilisée pour un chiffre AES spécifie le nombre de rondes de transformations qui convertissent l'entrée, appelée
texte en clair, à la sortie finale, appelée texte chiffré. Les nombres de rondes sont les suivants :

* 10 rondes pour des clefs de 128 bits.
* 12 rondes pour des clefs de 192 bits.
* 14 rondes pour des clefs de 256 bits.

Chaque ronde consiste en plusieurs étapes de calculs, incluant une étape qui dépend de la clef de chiffrement elle même. Un
ensemble de rondes inversées sont appliquées pour retransformer le texte chiffré en son texte original en clair à l'aide
de la même clef de chiffrement.

### Chiffrement asymétrique RSA

Dans un système de chiffrement à clef publique, la clef de chiffrement est publique et distincte de la clef de déchiffrement,
qui est gardée secrète (privée). Un utilisateur RSA créé et publie une clef publique sur la base de deux grand nombres premiers,
ainsi qu'une valeur auxiliaire. Les nombres premiers sont gardés secret. Les messages peuvent être chiffrés par n'importe qui,
via la clef publique, mais peut uniquement être décodé par quelqu'un connaissant les nombres premiers.

La sécurité du RSA repose sur la difficulté pratique à factoriser le produit de deux grands nombres premiers, le "problème de
factorisation".

RSA est un algorithme relativement lent. De ce fait, il n'est généralement pas directement utilisé pour chiffrer les données des
utilisateurs. Il est plus souvent utilisé pour transmettre des clefs partagées pour un chiffrement à clef symétrique, qui est
ensuite utilisé pour le chiffrement et déchiffrement des données.

La taille des clefs pour le chiffrement RSA varie entre 1024 et 4096 bits avec récemment une recommandation minimale de 2048
bits.

## Programmation orientée objet

La programmation orientée objet (POO), ou programmation par objet, est un paradigme de programmation informatique basé sur le
concept *d'objets*, qui peuvent contenir du code et des données : les données sous la forme de champs (attributs ou propriétés),
et le code sous la forme de procédures (méthodes).

Une capacité des objets est que ses procédures peuvent accéder et souvent modifier ses propres champs de données (*this* ou
*self*). En POO, les programmes informatiques sont créés de façon à pouvoir interagir les uns avec les autres. Les langages de
POO peuvent être très divers, mais les plus populaires sont basés sur la notion de classe, cela signifie que les objets sont des
instances de classes, qui déterminent également leurs types.

Une *classe* regroupe des membres, méthodes et propriétés (attributs) communs à un ensemble d'objets. La classe déclare, d'une
part, des attributs représentant l'état des objets et, d'autres part, des méthodes représentant leur comportement.

Il est possible de restreindre l'ensemble d'objets représenté par une classe A grâce à un mécanisme *d'héritage*. Dans ce cas,
on crée une nouvelle classe B liée à la classe A et qui ajoute de nouvelles propriétés.

Dans la programmation par objets, chaque objet est typé. Le type définit la syntaxe et la sémantique des messages auxquels peut
répondre un objet. Il correspond donc, à peu de chose près, à l'interface de l'objet.

Un objet peut appartenir à plus d'un type, c'est le *polymorphisme*, cela permet d'utiliser des objets de types différents là où
est attendu un objet d'un certain type.

## Compilation

Un compilateur est un programme qui transforme un code *source* en code *objet*. Généralement, le code source est écrit dans un
langage de programmation (le langage source), il est de haut niveau d'abstraction, et facilement compréhensible par l'humain. Le
code objet est généralement écrit en langage de plus bas niveau (appelé langage cible), par exemple un langage d'assemblage ou
langage machine, afin de créer un programme exécutable par une machine.

Un compilateur effectue les opérations suivantes : analyse lexicale, pré traitement (préprocesseur), analyse syntaxique
(parsing), analyse sémantique, et génération de code optimisé. La compilation est souvent suivie d'une étape d'édition de liens,
pour générer un fichier exécutable.

On distingue deux options de compilation :

* Ahead-of-time (AOT), où il faut compiler le programme avant de lancer l'application : la situation traditionnelle.
* Compilation à la volée (just-in-time, en abrégé JIT) : cette faculté est apparue dans les années 1980 (par exemple Tcl/Tk)

La tache principale d'un compilateur est de produire un code objet correct qui s'exécutera sur un ordinateur. La plupart des
compilateurs permettent d'optimiser le code, c'est-à-dire qu'ils vont chercher à améliorer la vitesse d'exécution, ou réduire
l'occupation mémoire du programme.

Un compilateur fonctionne par analyse-synthèse : au lieu de remplacer chaque construction du langage source par une suite
équivalente de constructions du langage cible, il commence par analyser le texte source pour en construire une *représentation
intermédiaire* qu'il traduit à son tour en langage cible.

On sépare le compilateur en au moins deux parties : une partie avant (ou frontale), parfois appelée "souche", qui lit le texte
source et produit la représentation intermédiaire ; et la partie arrière (ou finale), qui parcours cette représentation pour
produire le texte cible. Dans un compilateur idéal, la partie avant est indépendante du langage cible, tandis que la partie
arrière est indépendante du langage source (voir *Low Level Virtual Machine*).

L'implémentation d'un langage de programmation peut être interprétée ou compilée. Cette réalisation est un compilateur ou un
interpréteur, et un langage de programmation peut avoir une implémentation compilée, et une autre interprétée.

On parle de compilation si la traduction est faite avant l'exécution, et d'interprétation si la traduction est finie pas à pas,
durant l'exécution.

Les premiers compilateurs ont été écrits directement en langage assembleur, un langage symbolique élémentaire correspondant aux
instructions du processeur cible et quelques structures de contrôle légèrement plus évoluées. Ce langage symbolique doit être
assemblé (et non compilé) et lié pour obtenir une version exécutable. En raison de sa simplicité, un programme simple suffit à
le convertir en instruction machines.

Les compilateurs actuels sont généralement écrits dans le langage qu'ils doivent compiler : il ne dépend alors plus d'un autre
langage pour être produit.

Dans ce cas, il est complexe de détecter un bogue de compilateur. Le *bootstrap* oblige donc les programmeurs de compilateurs à
contourner les bugs des compilateurs existants.

## API

Une Interface de Programmation d'Application (API) est une interface qui définit des interactions entre des applications
logicielles diverses. Elle définit le type d'appels ou de requêtes pouvant être exécutés, comment les faire, les formats de
données qui doivent être utilisés, les conventions qui en découlent, etc. Elle peut également fournir des mécanismes d'extension
de façon à ce que les utilisateurs puissent étendre les fonctionnalités existantes de plusieurs manières et à des degrés variés.
Une API peut être entièrement personnalisé, spécifique à un composant, ou construite à partir d'un standard afin de garantir
l'interopérabilité. À travers le masquage d'information, les APIs présentent une approche modulaire et permet aux utilisateurs
d'accéder à l'interface indépendamment de son implémentation.

### SOAP

SOAP (*Simple Object Access Protocol*) est une spécification de protocole de messages pour des échanges d'information structurée
dans l'implémentation de webs services à travers un réseau informatique. Son objectif est de fournir extensibilité, neutralité,
verbosité et indépendance. Elle utilise un ensemble d'information XML pour son format de message, et s'appuie sur des protocoles
de la couche application, la plupart du temps HTTP, bien que certains anciens systèmes communiquent via SMTP, pour la
négociation et la transmission de messages.

SOAP permet aux développeurs d'invoquer des processus s'exécutant sur des systèmes d'exploitation disparates d'authentifier,
d'autoriser, et de communiquer à l'aide du langage de balisage extensible (XML). Puisque les protocoles webs tels que HTTP sont
installés et actifs sur tous les systèmes d'exploitation, SOAP permet aux clients d'invoquer des services webs et de recevoir
des réponses indépendantes du langage et des plateformes.

SOAP fournit la couche de protocole de messages de la pile de protocoles des webs services. C'est un protocole basé sur le
langage XML qui consiste en trois parties :

* Une enveloppe, qui définit la structure du message et la façon de le traiter.
* Un ensemble de règles d'encodage afin d'exprimer des instances de types de données définis par application.
* Une convention pour représenter les appels de procédures et leurs réponses.

SOAP a trois caractéristiques majeures :

1. l'extensibilité
2. la neutralité (SOAP peut opérer à travers des protocoles tels que HTTP, SMTP, TCP, UDP)
3. l'indépendance (SOAP ne contraint en aucune façon le modèle de programmation)

L'architecture SOAP consiste en plusieurs couches de spécifications pour :

* le format de message
* les motifs d'échange de message (MEP)
* les liens au protocole de transport sous-jacent
* les modèles de traitement de messages
* l'extensibilité du protocole

SOAP a évolué en tant que successeur de XML-RPC, bien qu'il emprunte son transport et la neutralité d'interaction de l'adressage
des webs services et l'enveloppe/entête/corps d'autres modèles.

La spécification SOAP peut être grossièrement définie comme étant constituée des 3 composants conceptuels suivants : les
concepts de protocole, les concepts d'encapsulation et les concepts de réseau.

**SOAP** est un ensemble de règles qui formalisent et gouvernent le format et les règles de traitement pour l'information
échangé entre l'émetteur et le receveur SOAP.

**Les noeuds SOAP** sont des machines physiques/logiques avec des unités de traitement utilisées pour transmettre/relayer,
recevoir et traiter les messages SOAP. Ils sont analogues aux noeuds d'un réseau.

**Les rôles SOAP** sont les rôles spécifiques qu'assument chacun des noeuds, à travers le chemin d'un message SOAP. Le rôle du
noeud définit l'action que le noeud doit effectuer sur le message qu'il reçoit. Par exemple, un rôle *none* signifie qu'aucun
noeud ne traitera l'entête SOAP en aucune façon et transmettra simplement le message le long de son chemin.

**Les liens aux protocoles SOAP** sont les interactions du message SOAP travaillant en conjonction des autres protocoles afin de
transférer un message SOAP à travers le réseau. Par exemple, un message SOAP peut utiliser TCP comme sous-couche protocolaire
pour le transfert de messages. Ces liens sont définis dans le cadre des liens aux protocoles sous-jacent SOAP.

**Les fonctionnalités SOAP** permettent d'étendre le cadre de message SOAP pour ajouter des fonctions telles que la fiabilité,
la sécurité etc. Il existe des règles à suivre lors de l'ajout de fonctionnalités au cadre SOAP.

**Les modules SOAP** sont une collection de spécifications concernant la sémantique des entêtes SOAP pour décrire une nouvelle
fonctionnalité étendue s'ajoutant à SOAP. Un module requiert zéro ou plusieurs fonctionnalités. SOAP a besoin des modules pour
adhérer au règles prescrites.

**Le message SOAP** représente l'information échangée entre 2 noeuds SOAP.

**L'enveloppe SOAP** est l'ensemble des éléments délimiteurs d'un message XML qui l'identifie comme étant un message SOAP.

**Le bloc d'entête SOAP** est un bloc de traitement discret contenu dans l'entête qui elle peut en contenir plusieurs. En
général, l'information du rôle SOAP est utilisée pour cibler des noeuds du chemin. Un bloc d'entête est dit être ciblé sur un
noeud SOAP si le rôle SOAP du bloc d'entête est le nom d'un rôle dans lequel le noeud SOAP opère. Par exemple le bloc d'entête
SOAP avec l'attribut de rôle *ultimateReceiver* est ciblé seulement au noeud destination qui a ce rôle. Un entête avec un
attribut de rôle *next* est ciblé pour chaque intermédiaire ainsi que le noeud destination.

**L'entête SOAP** est une collection d'un ou de plusieurs blocs d'entêtes ciblés pour chaque receveurs SOAP.

**Le corps SOAP** contient le corps du message à l'attention du receveur SOAP. L'interprétation et le traitement du corps SOAP
est défini par le blocs d'entêtes.

**L'erreur SOAP** est un élément qui contient l'information d'erreur dans le cas où un noeud SOAP échoue à traiter un message
SOAP. Cet élément est contenu dans le corps SOAP en tant qu'élément enfant.

**L'émetteur SOAP** est un noeud qui transmet un message SOAP.

**Le receveur SOAP** est un noeud recevant un message SOAP. (Il peut s'agir d'un noeud intermédiaire ou d'un noeud destination)

**Le chemin du message SOAP** est l'ensemble des noeuds que le message SOAP a traversé pour atteindre le noeud destination.

**L'émetteur initial SOAP** est le noeud à l'origine de message SOAP à transmettre. C'est la racine du chemin du message SOAP.

**L'intermédiaire SOAP** est le noeud situé entre l'émetteur initial et la destination SOAP voulue. Il traite les blocs d'entête
ciblés sur lui et agit pour relayer un message SOAP vers son ultime receveur SOAP.

**L'ultime receveur SOAP** est le receveur destinataire du message SOAP. Ce noeud est responsable du traitement du corps du
message et des blocs d'entête ciblés sur lui.

La spécification SOAP définit un cadre de messages, qui consiste en :

* Le **modèle de traitement SOAP**, définissant les règles pour traiter un message SOAP.
* Le **modèle d'extensibilité SOAP**, définissant les concepts de fonctionnalités et de modules SOAP.
* Le **cadre de liens aux protocoles sous-jacents** décrivant les règles définissant les liens aux protocoles sous-jacents
pouvant être utilisés lors des échanges de messages entre noeuds SOAP.
* Le **concept de message SOAP** définissant la structure d'un message SOAP.

Un message SOAP est un document XML ordinaire contenant les éléments suivants :

<table>
<tr>
    <th>Élément</th>
    <th>Description</th>
    <th>Requis</th>
</tr>
<tr>
    <td>Enveloppe</td>
    <td>Identifie le document XML en tant que message SOAP</td>
    <td>Oui</td>
</tr>
<tr>
    <td>Entête</td>
    <td>Contient les informations d'entête</td>
    <td>Non</td>
</tr>
<tr>
    <td>Corps</td>
    <td>Contient les informations d'appel et de réponse</td>
    <td>Oui</td>
</tr>
<tr>
    <td>Erreur</td>
    <td>Fournit des informations à propos d'erreurs ayant eu lieu lors du traitement du message</td>
    <td>Non</td>
</tr>
</table>

À la fois SMTP et HTTP sont des protocoles de la couche application valides en tant que transports pour SOAP, mais HTTP est plus
largement utilisé du fait qu'il fonctionne bien avec l'infrastructure internet ; spécifiquement, HTTP fonctionne bien avec les
pare-feu réseaux. SOAP peut également être utilisé par dessus HTTPS (qui est le même protocole qu'HTTP au niveau application,
mais utilise un protocole de transport chiffré en dessous) avec une authentification simple ou mutuelle ; c'est la méthode
utilisée pour fournir une sécurité au niveau des webs services.

L'ensemble des informations XML a été choisi comme format de message standard du fait de son très large usage dans l'industrie
et des efforts de développements open source. Typiquement, l'ensemble des informations XML est sérialisé comme du XML.

### REST

Le transfert d'état de représentations (REST) est un style d'architecture logicielle qui utilise un sous-ensemble d'HTTP. Il est
communément utilisé pour créer des applications interactives qui utilisent des services webs. Un service web qui suit ces lignes
directrices est appelé RESTful. Un tel service web doit fournir ces ressources web dans une représentation textuelle et
permettre de les lire et de les modifier à l'aide d'un protocole sans état et d'un ensemble d'opérations prédéfinies. Cette
approche permet l'interopérabilité entre les systèmes informatiques sur l'Internet qui fournissent ces services. REST est une
alternative par exemple au protocole SOAP qui permet d'accéder à un service web.

Des *ressources webs* étaient définis premièrement sur le web comme étant des documents ou des fichiers identifiés par leur
URLs. Aujourd'hui, la définition est bien plus générique et abstraite, et inclue chaque chose, entité, ou action pouvant être
identifiée, nommée, adressée, gérée ou exécutée, de quelque manière que ce soit sur le Web. Dans un service web RESTful, les
requêtes effectuées à une URI de ressource obtiennent une réponse avec un chargement formaté en HTML, XML, JSON, ou quelque
autre format. Par exemple, la réponse peut confirmer que l'état de la ressource a changé. La réponse peut également inclure des
liens hypertextes vers des ressources liées. Le protocole le plus commun pour ces requêtes et ces réponses est HTTP. Il fournit
des opérations (méthodes HTTP) telles que GET, POST, PUT, et DELETE. En utilisant des protocoles sans état et des opérations
standards, les systèmes RESTful essaient de tendre vers la performance, la fiabilité et la capacité à s'étendre en réutilisant
des composants pouvant être gérés et mis à jours sans affecter le système dans son entièreté, même en cours d'exécution.

Le but de REST est d'améliorer la performance, la mise à l'échelle, la simplicité, l'adaptabilité, la visibilité, la portabilité,
et la fiabilité. Ceci est réalisé à travers le respect des principes de REST tels que l'architecture client-serveur, l'absence
d'états, la possibilité de mise en cache, l'utilisation de systèmes multi-couches, le support de code à la demande, et l'usage
d'interfaces uniformisées. Ces principes doivent être suivis pour qu'un système soit classifié en tant que système REST.

### GraphQL

GraphQL est un langage de requêtage et de manipulation de données pour APIs, ainsi qu'un environnement d'exécution permettant de
répondre aux requêtes à l'aide de données existantes.

Il fournit une approche au développement d'APIs webs et est souvent comparé à REST et autres architectures de services webs. Il
permet aux clients de définir une structure de la donnée requise, et cette même structure de la donnée est retournée par le
serveur, prévenant ainsi le retour d'une trop grande quantité de données, mais ceci a des implications sur l'efficacité de la
mise en cache web des résultats de requêtes. La flexibilité et la richesse du langage de requêtage ajoute également une complexité
qui n'est pas forcément nécessaire pour des APIs simples. Malgré le nom, GraphQL ne fournit pas la richesse des opérations de
graphe que l'on peut trouver dans les bases de données orientées graphes tels que Neo4j, ou même certains dialectes de SQL qui
supportent les fermetures transitives. Par exemple, une interface GraphQL qui rapporte les parents d'un individu ne peut pas
retourner en une seule requête, l'ensemble de ses ancêtres.

GraphQL consiste en un système de types, un langage de requêtage et une sémantique d'exécution, une validation statique et une
introspection de type. Il supporte la lecture, l'écriture (mutation), et la souscription à des modifications de la donnée (mis à
jour en temps réel -- implémentée le plus souvent à l'aide de websockets). Les serveurs GraphQL sont disponibles pour de
nombreux langages de programmations.

### gRPC

gRPC est un système d'appel de procédures distantes. Il utilise HTTP/2 pour le transport, et protobuf en tant que langage de
description d'interface, et fournit des fonctionnalités telles que l'authentification, le streaming bidirectionnel et le
contrôle de flux, les liaisons bloquantes ou non bloquantes, l'annulation et les dépassements de délais. Il génère des liaisons
entre les clients multi-plateformes et le serveur pour de nombreux langages. Les usages généraux incluent l'interconnexion de
services dans un style architectural microservices, ou la connexion de clients mobiles à des services backend.

gRPC supporte l'usage de TLS et d'une authentification basée token. Il y a deux types de certificats : les certificats de
canaux et les certificats d'appels.

gRPC utilise protobuf pour encoder la donnée. Contrairement aux APIs HTTP avec JSON, elles obéissent à une spécification
stricte.
