# Système et données

#### Introduction

Cet exposé traite de la partie système et données du programme PSE.

Dans la première partie j'ai essayé de présenter à peu près logiquement et de manière globale mais loin d'être exhaustive, les
éléments d'un système d'exploitation (Linux). Pour aller plus loin sur l'ensemble des sujets évoqués on peut aller retrouver [la
documentation du noyau linux](https://www.kernel.org/doc/html/latest/). Les deux autres parties concernent la gestion des
données structurées (Bases de données), semi-structurées et déstructurées (Big data et lacs de données) avec une approche assez
légère pour deux sujets aussi larges.

#### Table des matières

* [Système](#système)
    + [Processus et mécanismes de communication](#processus-et-mécanismes-de-communication)
        - [Multitâche coopératif](#multitâche-coopératif)
        - [Multitâche préemptif](#multitâche-préemptif)
        - [Algorithmes d'ordonnancement](#algorithmes-dordonnancement)
        - [Synchronisation](#synchronisation)
        - [Signaux](#signaux)
        - [Socket réseau](#socket-réseau)
        - [Socket IPC](#socket-ipc)
        - [Socket Netlink](#socket-netlink)
        - [Tube anonyme](#tube-anonyme)
        - [Tube nommé](#tube-nommé)
        - [Passage de messages](#passage-de-messages)
    + [Gestion de la mémoire centrale](#gestion-de-la-mémoire-centrale)
        - [Fichier mappé en mémoire](#fichier-mappé-en-mémoire)
        - [Mémoire virtuelle](#mémoire-virtuelle)
        - [Pagination](#pagination)
        - [Segmentation](#segmentation)
    + [Gestion des périphériques](#gestion-des-périphériques)
        - [Pilotes](#pilotes)
        - [Sysfs](#sysfs)
        - [Udev](#udev)
        - [Bus](#bus)
        - [Accès direct à la mémoire](#accès-direct-à-la-mémoire)
    + [Éléments de démarrage](#éléments-de-démarrage)
        - [BIOS](#bios)
        - [UEFI](#uefi)
        - [Partitionnement de la mémoire](#partitionnement-de-la-mémoire)
        - [MBR](#mbr)
        - [GPT](#gpt)
        - [LVM](#lvm)
        - [RAID](#raid)
        - [Dm-crypt](#dm-crypt)
        - [LUKS](#luks)
        - [Chargeur d'amorçage](#chargeur-damorçage)
        - [Initramfs](#initramfs)
        - [Noyau](#noyau)
        - [Talon de démarrage UEFI et Image noyau unifiée](#talon-de-démarrage-uefi-et-image-noyau-unifiée)
        - [Cgroups](#cgroups)
        - [Espaces de noms](#espaces-de-noms)
        - [Systemd](#systemd)
        - [Shell](#shell)
    + [Éléments de sécurité](#éléments-de-sécurité)
        - [Secure Shell](#secure-shell)
        - [OpenSSH](#openssh)
        - [Netfilter](#netfilter)
        - [BPF et eBPF](#bpf-et-ebpf)
        - [IDS](#ids)
        - [Contrôle d'accès discrétionnaire et droits](#contrôle-daccès-discrétionnaire-et-droits)
        - [Listes de contrôle d'accès ACL](#listes-de-contrôle-daccès-ACL)
        - [SELinux](#seLinux)
        - [PAM](#pam)
    + [Virtualisation et conteneurisation](#virtualisation-et-conteneurisation)
        - [Systemd-nspawn](#systemd-nspawn)
        - [Conteneurisation LXC](#conteneurisation-lxc)
        - [Conteneurisation Docker](#conteneurisation-docker)
        - [Orchestrateur Kubernetes](#orchestrateur-kubernetes)
        - [Libvirt](#libvirt)
        - [Hyperviseurs](#hyperviseurs)
* [Données](#données)
    + [Bases de données](#bases-de-données)
        - [CRUD](#crud)
        - [ACID](#acid)
    + [Algèbre relationnelle](#algèbre-relationnelle)
    + [Administration SGBDR PostgreSQL](#administration-sgbdr-postgresql)
        - [Stockage et réplication](#stockage-et-réplication)
        - [Contrôle et connectivité](#contrôle-et-connectivité)
        - [Sécurité](#sécurité)
    + [Big data et lac de données](#big-data-et-lac-de-données)

## Système

### Processus et mécanismes de communication

#### Multitâche coopératif

Le multitâche coopératif est une forme simple de multitâche où chaque tâche doit explicitement permettre aux autres tâches de
s'exécuter. Cette approche simplifie l'architecture du système mais présente plusieurs inconvénients :

* Le multitâche coopératif est une forme de couplage fort. Si un des processus ne redonne pas la main à un autre processus, par
exemple si le processus est buggé, le système entier peut s'arrêter.
* Le partage de ressources (temps CPU, mémoire, accès disque, etc.) peut être inefficace.

#### Multitâche préemptif

Le multitâche préemptif désigne la capacité d'un système d'exploitation à exécuter ou arrêter une tâche planifiée en cours.

Un ordonnanceur préemptif présente l'avantage d'une meilleure réactivité du système et de son évolution, mais l'inconvénient
vient des situations de compétition (lorsque le processus d'exécution accède à la même ressource avant qu'un autre processus
(préempté) ait terminé son utilisation).

Dans un système d'exploitation multitâche préemptif, les processus ne sont pas autorisés à prendre un temps non défini pour
s'exécuter dans le processeur. Une quantité de temps définie est attribuée à chaque processus ; si la tâche n'est pas accomplie
avant la limite fixée, le processus est renvoyé dans la pile pour laisser place au processus suivant dans la file d'attente, qui
est alors exécuté par le processeur. Ce droit de préemption peut tout aussi bien survenir avec des interruptions matérielles.

Certaines tâches peuvent être affectées d'une priorité ; une tâche pouvant être spécifiée comme "préemptible" ou "non
préemptible". Une tâche préemptible peut être suspendue (mise à l'état "ready") au profit d'une tâche de priorité plus élevée ou
d'une interruption. Une tâche non préemptible ne peut être suspendue qu'au profit d'une interruption. Le temps qui lui est
accordé est plus long, et l'attente dans la file d'attente plus courte.

Au fur et à mesure de l'évolution des systèmes d'exploitation, les concepteurs ont quitté la logique binaire "préemptible/non
préemptible" au profit de systèmes plus fins de priorités multiples. Le principe est conservé, mais les priorités des programmes
sont échelonnées.

Pendant la préemption, l'état du processus (drapeaux, registres et pointeurs d'instruction) est sauvé dans la mémoire. Il doit
être rechargé dans le processeur pour que le code soit exécuté à nouveau : c'est la commutation de contexte.

Un système d'exploitation préemptif conserve en permanence la haute main sur les tâches exécutées par le processeur,
contrairement à un système d'exploitation non préemptif, ou collaboratif, dans lequel c'est le processus en cours d'exécution
qui prend la main et décide seul du moment où il la rend. L'avantage le plus évident d'un système préemptif est qu'il peut en
permanence décider d'interrompre un processus, principalement si celui-ci échoue et provoque l'instabilité du système.

#### Algorithmes d'ordonnancement

L'ordonnanceur désigne le composant du noyau du système d'exploitation choisissant l'ordre d'exécution des processus sur les
processeurs d'un ordinateur.

Un processus a besoin de la ressource processeur pour exécuter des calculs ; il l'abandonne quand se produit une interruption,
etc. De nombreux anciens processeurs ne peuvent effectuer qu'un traitement à la fois. Pour les autres, un ordonnanceur reste
nécessaire pour déterminer quel processus sera exécuté sur quel processeur (c'est la notion d'affinité, très importante pour ne
pas dégrader les performances). Au-delà des classiques processeurs multicoeurs, la notion d'hyperthreading rend la question de
l'ordonnancement encore un peu plus complexe.

A un instant donné, il y a souvent davantage de processus à exécuter que de processeurs.

Un des rôles de système d'exploitation et plus précisément de l'ordonnanceur du noyau, est de permettre à tous ces processus de
s'exécuter à un moment ou un autre et d'utiliser au mieux le processeur pour l'utilisateur. Pour que chaque tâche s'exécute sans
se préoccuper des autres et/ou aussi pour exécuter les tâches selon les contraintes imposées au système, l'ordonnanceur du noyau
du système effectue des commutations de contexte de celui-ci.

A intervalles réguliers, le système appelle une procédure d'ordonnancement qui *élit* le prochain processus à exécuter. Si le
nouveau processus est différent de l'ancien, un changement de contexte (opération consistant à sauvegarder le contexte
d'exécution de l'ancienne tâche comme les registres du processeur) a lieu. Cette structure de données est généralement appelée
PCB (process control block). Le système d'exploitation restaure l'ancien PCB de la tâche élue, qui s'exécute alors en reprenant
là où elle s'était arrêtée précédemment.

Du choix de l'algorithme d'ordonnancement dépend le comportement du système. Il existe deux grandes classes d'ordonnancement :

* **L'ordonnancement en temps partagé** présent sur la plupart des ordinateurs "classiques". Par exemple l'ordonnancement
"decay" ; qui est celui par défaut sous Unix. Il consiste en un système de priorités adaptatives, par exemple il privilégie les
tâches interactives pour que leur temps de réponse soit bon. Une sous-classe de l'ordonnancement en temps partagé sont les
ordonnanceurs dits "proportional share", eux sont plus destinés aux stations de calcul et permettent une gestion rigoureuse des
ressources. On peut citer notamment "lottery" et "stride".
* **L'ordonnancement en temps réel** qui assure qu'une certaine tâche sera terminée dans un délai donné. Cela est indispensable
dans les systèmes embarqués. Un ordonnanceur temps réel est dit optimal pour un système de tâches s'il trouve une solution
d'ordonnancement du système lorsque cette solution existe. S'il ne trouve pas de solution à ce système, alors aucun autre
ordonnanceur ne peut en trouver une.

Algorithmes d'ordonnancement :

* **Round-robin (ou méthode du tourniquet)** : Une petite unité de temps appelé quantum de temps est définie. La file d'attente
est gérée comme une file circulaire. L'ordonnanceur parcourt cette file et alloue un temps processeur à chacun des processus
pour un intervalle de temps de l'ordre d'un quantum au maximum. La performance de round-robin dépend fortement du choix du
quantum de base.
* **Rate-monotonic scheduling (RMS)** : L'ordonnancement à taux monotone est un algorithme d'ordonnancement temps réel en ligne
à priorité constante (statique). Il attribue la priorité la plus forte à la tâche qui possède la plus petite période. RMS est
optimal dans le cadre d'un système de tâches périodiques, synchrones, indépendantes et à échéance sur requête avec un
ordonnanceur préemptif. De ce fait, il n'est généralement utilisé que pour ordonnancer des tâches vérifiant ces propriétés.
* **Earliest deadline first scheduling (EDF)** : C'est un algorithme d'ordonnancement préemptif à priorité dynamique, utilisé
dans les systèmes temps réels. Il attribue une priorité à chaque requête en fonction de l'échéance de cette dernière selon la
règle : Plus l'échéance d'une tâche est proche, plus sa priorité est grande. De cette manière, au plus vite le travail doit être
réalisé, au plus il a des chances d'être exécuté.
* **FIFO** : Les premiers processus ajoutés à la file seront les premières à être exécutés.
* **Shortest job first (SJF, ou SJN-Shortest Job Next)** : Le choix se fait en fonction du temps d'exécution estimé du
processus. Ainsi l'ordonnanceur va laisser passer en priorité le plus court des processus de la file d'attente.
* **Completely Fair Scheduler (CFS)** : L'ordonnanceur des tâches pour le noyau Linux. Il gère l'allocation de ressource
processeur pour l'exécution des processus, en maximisant l'utilisation globale CPU tout en optimisant l'interactivité. CFS
utilise un arbre rouge-noir implémentant une chronologie des futures exécutions des tâches. En effet, l'arbre trie les processus
selon une valeur representative du manque de ces processus en temps d'allocation du processeur, par rapport au temps qu'aurait
alloué un processeur dit multitâche idéal. De plus, l'ordonnanceur utilise une granularité temporelle à la nanoseconde, rendant
redondante la notion de tranche de temps, les unités atomiques utilisées pour le partage du CPU entre processus. Cette
connaissance précise signifie également qu'aucune heuristique (basée sur la statistique, donc pouvant commettre des erreurs)
n'est requise pour déterminer l'interactivité d'un processus.

#### Synchronisation

En programmation concurrente, la synchronisation se réfère à deux concepts distincts mais liés : la synchronisation de processus
et la synchronisation des données. La synchronisation de processus est un mécanisme qui vise à bloquer l'exécution de certains
processus à des points précis de leur flux d'exécution, de manière que tous les processus se rejoignent à des étapes relais
données, tel que prévu par le programmeur. La synchronisation de données, elle, est un mécanisme qui vise à conserver la
cohérence des données telles que vues par différents processus, dans un environnement multitâche. Initialement, la notion de
synchronisation est apparue pour la synchronisation de données.

Ces problèmes dits "de synchronisation" et même plus généralement ceux de communication inter-processus dont ils dépendent
rendent pratiquement toujours la programmation concurrente plus difficile. Certaines méthodes de programmation, appelées
synchronisation non-blocante, cherchent à éviter d'utiliser des verrous, mais elles sont encore plus difficiles à mettre en
oeuvre et nécessitent la mise en place de structure de données particulières. La mémoire transactionnelle logicielle en est une.

La synchronisation de processus cherche par exemple à empêcher des programmes d'exécuter la même partie de code en même temps,
ou au contraire forcer l'exécution de deux partie de code en même temps. Dans la première hypothèse, le premier processus bloque
l'accès à la section critique avant d'y entrer et libère l'accès après y être sorti. Ce mécanisme peut être implémenté de
multiples manières.

Ces mécanismes sont par exemple la barrière de synchronisation, l'usage conjoint des sémaphores et des verrous, les spinlock, le
moniteur.

* **Barrière de synchronisation** : Permet de garantir qu'un certain nombre de tâches aient passé un point spécifique. Ainsi,
chaque tâche qui arrivera sur cette barrière devra attendre jusqu'à ce que le nombre spécifié de tâches soient arrivées à cette
barrière.
* **Sémaphore** : Variable partagée par différents "acteurs" qui garantit que ceux-ci ne peuvent accéder de façon séquentielle à
travers des opérations atomiques, et constitue la méthode utilisée couramment pour restreindre l'accès à des ressources
partagées et synchroniser les processus dans un environnement de programmation concurrente.
* **Verrous** : Permet d'assurer qu'un seul processus accède à une ressource à un instant donné. Un verrou peut être posé pour
protéger un accès en lecture et permettra à plusieurs processus de lire, mais aucun d'écrire. On dit alors que c'est un verrou
partagé. Un verrou est dit exclusif lorsqu'il interdit toute écriture et toute lecture en dehors du processus qui l'a posé. La
granularité d'un verrou constitue l'étendue des éléments qu'il protège. Par exemple dans les bases de données, un verrou peut
être posé sur une ligne, un lot de ligne, une table etc.
* **Spinlocks** : Mécanisme simple de synchronisation basé sur l'attente active.
* **Moniteur** : Mécanisme de synchronisation qui permet à plusieurs threads de bénéficier de l'exclusion mutuelle et la
possibilités d'attendre (*block*) l'invalidation d'une condition. Les moniteurs ont également un mécanisme qui permet aux autres
threads de signaler que leur condition est validé. Il est constitué d'un mutex et de variables conditionnelles.

La connaissance des dépendances entre les données est fondamentale dans la mise en oeuvre d'algorithmes parallèles, d'autant
qu'un calcul peut dépendre de plusieurs calculs préalables. Les *conditions de Bernstein* permettent de déterminer les
conditions sur les données lorsque deux parties de programme peuvent être exécutées en parallèle.

#### Signaux

Un signal est une forme de communication entre processus utilisée par les systèmes de type Unix et ceux respectant les standards
POSIX. Il s'agit d'une notification asynchrone envoyée à un processus pour lui signaler l'apparition d'un événement. Quand un
signal est envoyé à un processus, le système d'exploitation interrompt l'exécution normale de celui-ci. Si le processus possède
une routine de traitement pour le signal reçu, il lance son exécution. Dans le cas contraire, il exécute la routine des signaux
par défaut.

La norme POSIX (et la documentation de Linux) limite les fonctions directement ou indirectement appelables depuis cette routine
de traitements des signaux. Cette norme donne une liste exhaustive de fonctions primitives dites *async-signal safe* (en
pratique les appels systèmes) qui sont les seules à pouvoir être appelées depuis une routine de traitement de signal sans avoir
un comportement indéfini. Il est donc suggéré d'avoir une routine de traitement de signal qui positionne simplement un drapeau
déclaré *volatile sig_atomic_t* qui serait testé ailleurs dans le programme.

L'appel système kill(2) permet d'envoyer, si cela est permis, un signal à un processus. La commande kill(1) utilise cet appel
système pour faire de même depuis le shell. La fonction raise(3) permet d'envoyer un signal au processus courant.

Les exceptions comme les erreurs de segmentation ou les divisions par zéro génèrent des signaux. Ici les signaux générés seront
respectivement SIGSEGV et SIGFPE. Un processus recevant ces signaux se terminera et générera un core dump par défaut.

Le noyau peut générer des signaux pour notifier les processus que quelque chose s'est passé. Par exemple, SIGPIPE est envoyé à
un processus qui essaye d'écrire dans un pipe qui a été fermé par celui qui lit. Par défaut, le programme se termine alors. Ce
comportement rend la construction de pipeline en shell aisée.

#### Socket réseau

Une socket réseau est une structure logicielle comprise dans un noeud réseau qui sert de point d'arrivée pour les données
envoyées et reçues à travers le réseau. La structure et les propriétés d'une socket sont définies par une interface de
programmation (API) de l'architecture réseau. Les sockets sont créées uniquement durant l'intervalle de temps d'un processus
d'une application s'exécutant dans le noeud.

Du fait de la standardisation des protocoles TCP/IP au cours du développement d'Internet, le terme *socket réseau* est plus
communément utilisé dans le contexte de la *Suite des protocoles Internets*. On parle alors aussi de socket internet. Dans ce
contexte, une socket est identifiée extérieurement par les autres machines par son *adresse socket*, qui est la triade du
protocole de transfert, de l'adresse IP et du numéro de port.

Une pile de protocole, habituellement fournie par le système d'exploitation est un ensemble de services permettant aux processus
de communiquer à travers un réseau utilisant les protocoles que la pile implémente. Le système d'exploitation fait passer les
données utiles des paquets IP entrants à l'application correspondante en lisant l'information de l'adresse socket des entêtes
des protocoles IP et transport et en enlevant ces entêtes des données applications.

L'interface de programmation que les programmes utilisent pour communiquer avec la pile de protocole, utilisant les sockets
réseaux, est appelée **socket API**. Les API sockets internet sont généralement basées sur le standard socket de Berkeley. Dans
le standard socket de Berkeley, les socket sont une forme de descripteur de fichier (*read, write, open, close*).

Dans les protocoles internet standards TCP et UDP, une adresse socket est la combinaison d'une adresse IP et d'un numéro de
port. Les sockets n'ont pas besoin d'adresse source, mais si un programme lie la socket à une adresse source, la socket peut
être utilisée pour recevoir et envoyer des données à cette adresse. Basé sur cette adresse, les sockets internet délivrent les
paquets applicatifs entrants au processus applicatif approprié.

Plusieurs types de sockets internet sont disponibles :

* **Datagram** : Des sockets non connectées, qui utilisent le protocole UDP (*User Datagram Protocol*). Chaque paquet envoyé ou
reçu avec un socket datagramme est adressé et routé individuellement. L'ordre ainsi que la fiabilité ne sont pas garantis, par
conséquent plusieurs paquets envoyés depuis un processus à un autre peuvent arriver dans n'importe quel ordre ou bien ne pas
arriver du tout. Certaines configurations spéciales peuvent être requises pour envoyer en broadcast un socket datagramme.
* **Stream** : Des socket connectés, qui utilisent les protocoles TCP (*Transmission Control Protocol*), SCTP (*Stream Control
Transmission Protocol*) ou DCCP (*Datagram Congestion Control Protocol*). Un socket flux fournit un flot de données sans
erreurs, séquencé, unique et ininterrompu avec des mécanismes prédéfinis pour créer et détruire des connexions et rapporter des
erreurs. Un socket flux transmet des données de manière fiable, ordonnée sans requérir l'établissement préalable d'un canal de
communication.
* **Raw** : Permet l'envoie et la réception de paquets IP sans aucun formatage spécifique à un protocole de la couche transport.
Avec les autres types de socket, la donnée est automatiquement encapsulée selon le protocole de la couche transport choisi (TCP,
UDP etc.), et l'utilisateur du socket n'a pas connaissance de l'existence des entêtes du protocole. Quand on lit d'un socket
brut, les entêtes sont généralement inclus. Lorsqu'on transmet des paquets depuis un socket brut, l'addition automatique d'une
entête est optionnelle.

#### Socket IPC

Un socket de domaine Unix ou socket IPC (*inter-process communication*) et un point d'arrivée des données de communications qui
permet d'échanger des données entre des processus s'exécutant sur le même système d'exploitation hôte. Les types de socket
valides dans le domaine UNIX sont :

* **SOCK_STREAM** (à comparer au TCP) - pour un socket orienté flux
* **SOCK_DGRAM** (à comparer à UDP) - pour un socket orienté datagramme qui préserve les limites des messages (comme la plupart
des implémentations UNIX, les socket de domaine UNIX datagram sont toujours fiables et ne réordonnent pas les datagrammes)
* **SOCK_SEQPACKET** (à comparer à SCTP) - pour un socket à paquets séquencés orienté connexion, qui préserve les limites des
messages, et livre les paquets dans l'ordre d'envoi.

Les sockets de domaine Unix sont une composante standard des systèmes d'exploitation POSIX.

Les interfaces de programmation (API) pour les sockets de domaine Unix sont similaires à celles des sockets internet, mais au
lieu d'utiliser un protocole réseau sous-jacent, toutes les communications se placent à l'intérieur du noyau du système
d'exploitation. Les socket de domaine Unix peuvent utiliser le système de fichiers comme adresse d'espace de noms. (Certains
systèmes d'exploitation, comme Linux, offrent des espaces de noms additionnels.) Les processus référencent les sockets de
domaine Unix comme des inodes du système de fichier, ainsi 2 processus peuvent communiquer en ouvrant la même socket.

En plus de permettre l'envoi de données, les processus peuvent envoyer des descripteurs de fichiers à travers une connexion de
socket de domaine Unix en utilisant les appels systèmes sendmsg() et recvmsg(). Ceci permet au processus qui envoie d'autoriser
le processus qui reçoit à accéder au descripteur de fichier auquel autrement le processus qui reçoit n'a pas accès. Ceci permet
d'implémenter une forme rudimentaire de sécurité basée sur l'accessibilité.

#### Socket Netlink

La famille de socket Netlink est une interface du noyau Linux utilisée pour des communications inter-processus entre les
processus de l'espace utilisateur et du noyau et entre différents processus utilisateurs. La différence entre les sockets
Netlink et les socket IPC et qu'au lieu d'utiliser l'espace de noms du système de fichiers, les processus Netlink sont
généralement désignés par leurs PIDs.

Netlink fournit une interface socket standard pour les processus utilisateurs, et une API côté noyau pour un usage interne par
les modules du noyau.

#### Tube anonyme

Un tube anonyme est un mécanisme de gestion de flux de donnée. Ce mécanisme inventé pour UNIX est principalement utilisé dans la
communication inter-processus. Ouvrir un tube anonyme permet de créer un flux de donnée unidirectionnel FIFO entre un processus
et un autre. Ces tubes sont détruits lorsque le processus qui les a créés disparaît, contrairement aux tubes nommés qui sont
liés au système d'exploitation et qui doivent être explicitement détruits.

Ce mécanisme permet la création de filtres.

Pour les système d'exploitation de type Unix, un tube anonyme est créé grâce à un appel système qui retourne un descripteur de
fichier à la suite de la création d'un Fork qui permet d'assigner à chacun des processus son rôle de récepteur ou d'émetteur.

#### Tube nommé

Comme les tubes anonymes, les tubes nommés sont des zones de données organisées en FIFO mais contrairement à ceux-ci qui sont
détruits lorsque le processus qui les a créés disparait, les tubes nommés sont liés au système d'exploitation et ils doivent
être explicitement détruits.

#### Passage de messages

Le modèle de passage de messages et une technique permettant de demander l'exécution d'un programme. Le passage de message
utilise un modèle objet afin de distinguer la fonction générale de ses implémentations spécifiques. Le programme appelant envoie
un message et se fie à l'objet afin de sélectionner et d'exécuter le code approprié. L'utilisation d'une couche intermédiaire,
est justifiée par des besoins de distribution et d'encapsulation.

L'encapsulation suit l'idée que les objets logiciels devraient être capables d'invoquer les services d'autres objets sans avoir
aucune connaissance spécifique de leurs implémentations. L'encapsulation permet de réduire les lignes de codes ainsi qu'une plus
grande maintenabilité des systèmes.

Le passage de messages distribué permet au développeur, à l'aide d'une couche fournissant les services de base de construire des
systèmes constitués de sous-systèmes s'exécutant sur des ordinateurs disparates, à différents endroit et à des horaires
différents. Lorsqu'un objet distribué envoie un message, la couche message s'occupe de :

* Trouver d'où et de quel processus le message est issu.
* Sauvegarder le message dans une file si l'objet approprié au traitement du message n'est pas en cours d'exécution et s'occuper
de l'envoyer dès que l'objet est disponible. Ainsi que de stocker le résultat si besoin, jusqu'à ce que l'objet qui a envoyé le
message est prêt à le recevoir.
* Contrôler diverses dépendances transactionnelles pour les transactions distribuées.

### Gestion de la mémoire centrale

#### Fichier mappé en mémoire

Un fichier mappé en mémoire est un segment de mémoire virtuelle qui est la copie d'une portion de fichier ou d'une ressource de
type fichier. Cette ressource est typiquement un fichier présent sur le disque, mais cela peut également être un périphérique,
un objet en mémoire partagée, ou toute autre ressource que le système d'exploitation peut référencer à l'aide d'un descripteur
de fichier. Une fois présente en mémoire, cette corrélation entre le fichier et l'espace mémoire permet aux applications de
traiter la partie mappée comme s'il s'agissait de la mémoire primaire.

Le bénéfice d'utiliser le mappage en mémoire est d'augmenter les performances d'entrée/sortie notamment sur les fichiers de gros
volume. Pour les petits fichiers, les fichiers mappés peuvent engendrer des problèmes de fragmentation interne du fait que les
maps mémoires sont toujours aligné sur la taille de la page (généralement 4Ko). Par conséquent, un fichier de 5Ko allouera 8Ko
et gâchera 3Ko. Accéder aux fichiers mappés en mémoire est plus rapide que d'utiliser des opérations de lecture et d'écriture
directement pour 2 raisons. Premièrement, un appel système est bien plus lent qu'un accès vers la mémoire locale du programme.
Deuxièmement, dans la plupart des systèmes d'exploitation la région mémoire mappée est la page cache du noyau, c'est à dire que
cela ne nécessite aucune copie en espace utilisateur.

Le processus de mapping mémoire est géré par le gestionnaire de mémoire virtuelle, qui est le même sous-système responsable de
la pagination. Les fichiers mappés sont chargés une page entière à la fois. La taille de la page est choisi par le système
d'exploitation pour un maximum de performance. Sachant que la pagination est un élément critique du gestionnaire de mémoire
virtuelle, le chargement des portions de la taille d'une page d'un fichier en mémoire physique est généralement une fonction
système très optimisée.

L'usage le plus commun de fichier mappé en mémoire est le chargement de processus. Lors de la création d'un processus, le
système d'exploitation utilise un fichier mappé en mémoire pour faire apparaître le fichier exécutable ainsi que tous les
modules chargeable en mémoire pour exécution. La technique la plus utilisée est la demande de pages, le fichier est chargé en
mémoire physique par section (chacune d'une page), et seulement quand cette page est référencée. Dans le cas spécifique des
exécutables, cela permet à l'OS de charger de manière sélective uniquement les portions de l'image processus qui nécessitent
réellement une exécution.

Un autre usage répandu pour les fichiers mappés en mémoire est le partage de fichiers entre processus multiples. Ceci permet
d'éviter les fautes de pages ainsi que les violations de segmentation.

#### Mémoire virtuelle

Le principe de mémoire virtuelle repose sur l'utilisation de traduction à la volée des adresses virtuelles vue du logiciel, en
adresses physiques de mémoire vive. La mémoire virtuelle permet :

* d'utiliser de la mémoire de masse comme extension de la mémoire vive ;
* d'augmenter le taux de multiprogrammation ;
* de mettre en place des mécanismes de protection de la mémoire ;
* de partager la mémoire entre processus.

#### Pagination

Les adresses mémoires émises par le processeur sont des adresses virtuelles, indiquant la position d'un mot dans la mémoire
virtuelle. Cette mémoire virtuelle est formée de zones de même taille, appelées pages. Une adresse virtuelle est donc un couple
(numéro de page, déplacement dans la page). La taille des pages est une puissance entière de deux, de façon à déterminer sans
calcul le déplacement (10 bits de poids faible de l'adresse virtuelle pour des pages de 1024 mots), et le numéro de page (les
autres bits). La mémoire vive est également composées de zones de même taille, appelées cadres (*frames*), dans lesquelles
prennent place les pages (un cadre contient une page : taille d'un cadre = taille d'une page). La taille de l'ensemble des
cadres en mémoire vive utilisés par un processus est appelé *Resident set size*. Un mécanisme de traduction (*translation*)
assure la conversion des adresses virtuelles en adresses physiques, en consultant une table des pages (*page table*) pour
connaître le numéro du cadre qui contient la page recherchée. L'adresse physique obtenue est le couple (numéro de cadre,
déplacement). Il peut y avoir plus de pages que de cadres (c'est là tout l'intérêt) : les pages qui ne sont pas en mémoire sont
stockées sur un autre support (disque), elle seront ramenées dans un cadre quand on en aura besoin.

La table des pages est indexée par le numéro de page. Chaque ligne est appelée "entrée dans la table des pages (*pages table
entry* PTE), et contient le numéro de cadre. La table des pages pouvant être située n'importe où en mémoire, un registre spécial
(PTBR pour *Page Table Base Register*) conserve son adresse.

En pratique, le mécanisme de traduction fait partie d'un circuit électronique appelé MMU (*memory management unit*) qui contient
également une partie de la table des pages, stockée dans une mémoire associative formée de registres rapides. Ceci évite d'avoir
à consulter la table des pages (en mémoire) pour chaque accès mémoire.

#### Segmentation

La segmentation est une technique de découpage de la mémoire. Elle est gérée par l'unité de segmentation de l'unité de gestion
mémoire (*MMU*), utilisée sur les systèmes d'exploitation modernes, qui divise la mémoire physique (dans le cas de la
segmentation pure) ou la mémoire virtuelle (dans le cas de la segmentation avec pagination) en segments caractérisés par leur
adresse de début et leur taille (*décalage*).

La segmentation permet la séparation des données du programme (entre autres segments) dans des espaces logiquement indépendants
facilitant alors la programmation, l'édition de liens et le partage inter-processus. La segmentation permet également d'offrir
une plus grande protection grâce au niveau de privilège de chaque segment.

Lorsque l'unité de gestion mémoire (MMU) doit traduire une adresse logique en adresse linéaire, l'unité de segmentation doit
dans un premier temps utiliser la première partie de l'adresse, c'est à dire le sélecteur de segment, pour retrouver les
caractéristiques du segment (base, limit, DPL, etc.) dans la table de descripteurs (GDT ou LDT). Puis il utilise la valeur de
décalage qui référence l'adresse à l'intérieur du segment.

Il existe sur la majorité des processeurs actuels, des registres de segments (CS, DS, SS, etc.) qui contiennent le sélecteur de
segment dernièrement utilisé par le processeur qui sont utilisés pour accélérer l'accès à ces sélecteurs.

Sur les processeurs récents, il existe également des registres associés à chaque registre de segment qui contiennent le
descripteur de segment associé pour un accès plus rapide aux descripteurs.

Un segment mémoire est un espace d'adressage indépendant défini par deux valeurs :

* L'adresse où il commence (aussi appelée *base*, ou *adresse de base*)
* Sa taille ou son *décalage* (aussi appelée *limite* ou *offset*)

Un segment constitue donc dans la mémoire principale un plage d'adresse continue. Les segments se chevauchent. On peut donc
adresser la même zone mémoire avec plusieurs couples segment/offset.

Il existe différents types de segment :

* Les segments de données statiques
* Les segments de données globales
* Les segments de code
* Les segments d'état de tâche

### Gestion des périphériques

#### Pilotes

Un pilote est un programme qui opère et contrôle un périphérique. Un pilote fournit une interface logicielle au matériel,
permettant au système d'exploitation et aux autres programmes d'accéder aux fonctions matérielles sans avoir besoin de connaître
en détail le périphérique à utiliser.

Le pilote communique avec le périphérique via le bus informatique auquel celui-ci est connecté. Lorsqu'un programme appelant
invoque une routine du pilote, celui-ci va envoyer une commande au périphérique. Une fois que le périphérique renvoie des
données au pilote, le pilote peut invoquer des routines du programme à l'origine de l'appel.

Les pilotes sont dépendant du matériel et spécifiques au système d'exploitation. Il fournissent généralement la gestion des
interruptions à n'importe quelle interface matérielle asynchrone nécessaire.

#### Sysfs

Sysfs est un système de fichiers virtuel qui permet d'exporter depuis l'espace noyau vers l'espace utilisateur des informations
sur les périphériques du système et leur pilotes, et est également utilisé pour configurer certaines fonctionnalités du noyau.

Pour chaque objet ajouté à l'arbre des modèles de pilotes (pilotes, périphériques, classe de périphériques), un répertoire est
créé dans sysfs. La relation parent/enfant est représentée sous la forme de sous-répertoires dans */sys/devices/* (représentant
la couche physique). Le sous-répertoire */sys/bus/* est peuplé de liens symboliques, représentant la manière dont chaque
périphérique appartient aux différents bus. */sys/class/* montre les périphérique regroupés en classes, comme les périphériques
réseau par exemple, pendant que */sys/block/* contient les périphériques de type bloc.

Pour les périphériques et leurs pilotes, des attributs peuvent être créés. Ce sont de simples fichiers, la seule contrainte est
qu'ils ne peuvent contenir chacun qu'une seule valeur et/ou n'autoriser le renseignement que d'une valeur (au contraire de
certains fichiers de procfs, qui nécessitent d'être longuement parcourus). Ces fichiers sont placés dans le sous-répertoire du
pilote correspondant au périphérique. L'utilisation de groupes d'attributs est possible en créant un sous-répertoire peuplé
d'attributs.

Sysfs est utilisé par quelques utilitaires pour accéder aux informations concernant le matériel et ses pilotes (des modules du
noyau comme udev par exemple). Des scripts ont été écrits pour accéder aux informations obtenues précédemment via procfs, et
certains scripts permettent la configuration du matériel et de leur pilote via leurs attributs.

Sysfs s'appuie sur ramfs. Un système de fichiers temporaire très simple monté en RAM.

#### Udev

Udev est un gestionnaire de périphérique pour le noyau Linux. Udev gère principalement des noeuds périphériques dans le
répertoire
*/dev/*. Udev traite également tous les événements dans l'espace utilisateurs lors de l'ajout ou de la suppression d'un
périphérique, ainsi que du chargement des microgiciels.

Les pilotes font parti du noyau Linux, dans le sens où leurs fonctions principales incluent la découverte de périphérique, la
détection des changements d'états, et autres fonctions matérielles similaires de bas niveau. Après chargement du pilote de
périphérique en mémoire depuis le noyau, les événements détectés sont envoyés au daemon de l'espace utilisateur *udevd*. C'est
le gestionnaire de périphérique, *udevd*, qui récupère tout ces événements et qui ensuite décide de la suite à donner. A cette
fin, *udevd* dispose d'un ensemble de fichiers de configurations, pouvant être ajustés par l'administrateur suivant ses besoins.

* Dans le cas d'un nouvel appareil de stockage USB, *udevd* est notifié par le noyau qui lui-même notifie le udisksd-daemon. Ce
daemon pourra alors monter le système de fichiers.
* Dans le cas d'une nouvelle connexion de câble Ethernet à la carte d'interface réseau Ethernet (NIC), *udevd* est notifié par
le noyau qui lui-même notifie le NetworkManager-daemon. Le NetworkManager-daemon pourra alors démarrer le daemon client dhcp
pour cette NIC, ou bien configurer la connexion à l'aide d'une configuration manuelle quelconque.

Contrairement aux systèmes traditionnels UNIX, ou les noeuds périphériques contenus dans le répertoire */dev* était un ensemble
de fichiers statique, le gestionnaire de périphérique Linux udev fournit dynamiquement, uniquement les noeuds des périphériques
actuellement disponibles au système :

* udev fournit un nommage de périphérique persistant, qui ne dépend pas de, par exemple, l'ordre de connexion des appareils au
système.
* udev s'exécute entièrement en espace utilisateur. Une conséquence est que udev peut exécuter des programmes arbitraires pour
composer un nom pour le périphérique fonction de ses propriétés, avant que le noeud soit créé; d'ailleurs, l'ensemble du
processus de nommage est également interruptible et s'exécute avec une priorité basse.

Udev est divisé en trois parties :

* La bibliothèque *libudev* qui permet l'accès aux informations des périphériques; qui est maintenant inclue dans *systemd*.
* Le daemon de l'espace utilisateur *udevd* qui gère */dev* virtuel.
* L'utilitaire d'administration en ligne de commande *udevadm* pour des diagnostics.

Le système reçoit des appels depuis le noyau via des sockets netlink.

#### Bus

Un bus est un système de communication qui permet de transférer des données entre les composants d'un ordinateur ou entre
ordinateurs. Cette expression couvre tous les composants matériels (fils, fibre optique, etc.) et logiciels, protocoles de
communications inclus. Les bus informatiques modernes peuvent à la fois utiliser des connexions parallèles et séries avec hubs.
C'est par exemple le cas de l'USB.

Un bus d'adresses est un bus utilisé pour spécifier une adresse physique. Quand un processeur ou un périphérique bénéficiant
d'un accès direct à la mémoire (*DMA-enabled*) a besoin de lire ou d'écrire en mémoire, il spécifie en emplacement mémoire sur
le bus d'adresses (la valeur à lire ou à écrire est alors envoyé sur le bus de données). La largeur du bus d'adresse détermine
la quantité de mémoire qu'un système peut adresser.

Accéder un octet individuel requiert généralement d'écrire ou de lire une largeur de bus complète (un mot) à la fois. Dans ce
cas, les bits les moins signifiants du bus d'adresses peuvent même ne pas être implémentés - il revient en effet au contrôleur
d'isoler l'octet individuel demandé du mot complet transmis.

#### Accès direct à la mémoire

L'accès direct à la mémoire (*DMA*) est un procédé informatique où les données circulant de, ou vers un périphérique sont
transférées directement par un contrôleur adapté vers la mémoire principale, sans intervention du microprocesseur si ce n'est
pour lancer et conclure le transfert. La conclusion du transfert ou la disponibilité du périphérique peuvent être signalés par
interruption.

### Éléments de démarrage

#### BIOS

Le **BIOS** est un microgiciel utilisé pour l'initialisation du matériel pendant le processus de démarrage, et pour fournir des
services à l'exécution pour les systèmes d'exploitation et les programmes. Le microgiciel BIOS est préinstallé sur la carte
mère, c'est le premier logiciel à s'exécuter lors de la mise sous tension.

Le BIOS dans les PCs modernes initialise et teste les composants matériels du système, et charge un chargeur d'amorçage depuis
l'appareil de stockage de masse qui initialise un système d'exploitation.

La plupart des implémentations du BIOS sont spécifiques à un modèle de carte mère, en s'interfaçant avec différents appareil et
en particulier le chipset. Anciennement, le microgiciel BIOS était stocké sur une puce ROM de la carte mère. De nos jours, le
contenu du BIOS est stocké sur de la mémoire flash de façon à pouvoir être réécrite sans enlever la puce de la carte mère. Ceci
permet une mise à jour aisée du BIOS, mais également l'infection de l'ordinateur via des rootkits du BIOS. De plus, si une mise
à jour du BIOS échoue cela peut potentiellement rendre la carte mère inutilisable.

L'interface microgicielle extensible unifiée (UEFI) est un successeur du BIOS, il vise à résoudre ses limitations techniques.

#### UEFI

**L'interface microgicielle extensible unifiée (UEFI)** est une spécification qui définit une interface logicielle entre un
système d'exploitation et une plateforme microgicielle. UEFI remplace l'ancienne interface microgicielle BIOS dont elle reprend
généralement l'ensemble des services. L'UEFI peut supporter les diagnostics et réparations distants, même sans système
d'exploitation.

L'interface défini par la spécification EFI inclut des tables de données qui contiennent des informations de plateforme, ainsi
que des services de démarrage et d'exécution disponible au chargeur d'amorçage et au système d'exploitation. Le microgiciel
UEFI fournit un certain nombre d'avantages techniques par rapport à un système BIOS traditionnel :

* La possibilité de démarrer un disque contenant de grandes partitions (plus de 2TB) à l'aide d'une table de partition GUID
(GPT)
* Un environnement pre-SE flexible, incluant des capacités réseaux, une interface graphique utilisateur, le multi-langage
* Un environnement pre-SE 32 ou 64 bits.
* Une programmation en langage C
* Une architecture modulaire
* Une compatibilité

Contrairement au BIOS, UEFI ne s'appuie pas sur des secteurs de démarrage, il définit un gestionnaire de démarrage comme faisant
parti des spécifications UEFI. Quand l'ordinateur est allumé, le gestionnaire de démarrage vérifie la configuration de démarrage
et en fonction de ses paramètres, exécute ensuite le chargeur d'amorçage spécifié ou noyau de système d'exploitation
(généralement chargeur d'amorçage). La configuration de démarrage est définie par des variables stockées en NVRAM, incluant des
variables qui indiquent les chemins du système de fichier vers les chargeurs d'amorçages ou noyaux.

Les chargeurs d'amorçage peuvent également être détectés automatiquement par UEFI grâce à des chemins standardisés contenant des
fichiers au format *.efi*.

La spécification UEFI spécifie que les exécutables portables (PE) de Microsoft sont le format d'exécutable standard dans les
environnements EFI.

#### Partitionnement de la mémoire

Le partitionnement de la mémoire est la création d'une ou plusieurs régions dans la mémoire secondaire, telle que chaque région
puisse être gérée séparément. Ces régions sont appelées partitions. C'est généralement la première chose à faire sur un nouveau
disque installé, avant qu'un système de fichier soit créé. Le disque stocke l'information sur le positionnement et la taille des
partitions dans une aire appelée table de partition que le système d'exploitation lit avant toute autre région du disque. Chaque
partition apparait alors comme un disque "logique" du point de vue du système d'exploitation qui fait usage d'une partie du
disque réel. Les administrateurs systèmes utilisent alors un programme appelé éditeur de partitions pour créer, retailler,
supprimer et manipuler les partitions. Le partitionnement permet l'usage de différents systèmes de fichiers afin de stocker tout
types de fichiers. Séparer les données utilisateur des données système permet d'empêcher que la partition système soit pleine ce
qui rendrait le système inutilisable. Le partitionnement permet aussi de simplifier la sauvegarde. Un désavantage du
partitionnement est qu'il peut être difficile d'allouer la taille adéquate à chacune des partitions, ce qui peut avoir pour
conséquence de laisser une partition avec énormément d'espace libre et une autre totalement saturée.

#### MBR

Un **enregistrement de démarrage maître (MBR)** est type spécial de secteur de démarrage situé au commencement d'un périphérique
de stockage informatique partitionné tel qu'un disque dur.

Le MBR contient l'information du comment les partitions logiques, contenant le système de fichiers, sont organisées sur ce
périphérique. Le MBR contient également du code exécutable pour fonctionner comme chargeur pour un système d'exploitation
installé généralement en laissant la main au second étage du chargeur, ou en conjonction avec chacun des enregistrements de
démarrage de volume (VBR). Ce code MBR est généralement désigné comme un chargeur d'amorçage.

L'organisation de la table de partition au niveau du MBR limite le maximum d'espace de stockage adressable d'un disque
partitionné à 2To. Par conséquent, le schema de partitionnement MBR est progressivement remplacé par le schema de table de
partitionnement GUID (GPT).

Le MBR consiste généralement à 512 octets situés sur le premier secteur du disque.

Il peut contenir un(e) ou plusieurs :

* Table de partition décrivant les partitions sur un périphérique de stockage. Dans ce contexte, le secteur de démarrage peut
également être appelé secteur de partitionnement.
* Code de bootstrap : Instructions permettant d'identifier la partition de démarrage configurée, puis charge et exécute son
volume d'enregistrement de démarrage (VBR) en tant que chargeur chaîné.
* Horodatage disque 32-bits optionnel
* Signature disque 32-bits optionnel

#### GPT

La **table de partition GUID (GPT)** est un standard pour la disposition de tables de partitions sur un appareil de stockage
informatique, tels que les disques durs ou les SSDs, en utilisant des identifiants uniques universels, aussi connus en tant
qu'identifiants uniques globaux (GUIDs). Ce standard fait parti des standards d'interface microgicielle extensible unifiée
(UEFI), il peut néanmoins être utilisé pour des systèmes BIOS, du fait des limitations des tables de partitions MBR.

Tous les systèmes d'exploitation récents supportent GPT.

De la même façon que MBR, GPT utilise un adressage de blocs logique (LBA) à la place de l'adressage historique
cylindre-tête-secteur (CHS). Le MBR est stocké au niveau de LBA 0, l'entête GPT à LBA 1. L'entête GPT contient un pointeur vers
la table de partition (typiquement situé à LBA 2). Chaque entrée dans la table de partition a une taille de 128 octets. Les
spécifications UEFI stipulent qu'un minimum de 16 384 octets, soit attribué à la table de partition. Ainsi, sur un disque avec
des secteurs de 512 octets, au moins 32 secteurs sont utilisés pour la table de partition, et le premier bloc utilisable est le
LBA 34 ou supérieur.

#### LVM

La gestion de volumes logiques (LVM) utilise la fonctionnalité du composant *device-mapper* du noyau linux pour fournir un
système de partitions indépendant de l'arrangement des disques sous-jacents. Avec LVM on obtient des **partitions virtuelles**.
Cette abstraction facilite la gestion des volumes de stockage (potentiellement toujours sujette aux limitations du système de
fichier).

Ces partitions virtuelles permettent l'ajout et la suppression sans avoir à ce soucier si l'espace contigu est suffisant sur un
disque en particulier, sans se faire avoir en essayant de formater un disque en cours d'utilisation (et se demander si le noyau
utilise l'ancienne ou la nouvelle table de partition), ou , avoir à déplacer des partitions pour en agrandir une. 

Les composants basiques de LVM sont les :

* **Volumes physiques (PV)** : Un noeud de périphérique de bloc Unix, utilisable pour du stockage par LVM. Il accueille une
entête LVM.
* **Groupes de volumes (VG)** : Groupe de volumes physiques (PVs) qui sert de contenant aux volumes logiques (LVs). Les
extensions physiques (PEs) sont allouées depuis un groupe de volume (VG) pour un volume logique (LV).
* **Volumes Logiques (LV)** : "Partition virtuelle/logique" résidant dans un groupe de volume (VG) et composé d'extensions
physiques (PEs). Les Volumes logiques (LVs) sont des périphériques de blocs Unix analogues à des partitions physiques, c'est à
dire qu'elles peuvent être directement formatées à l'aide d'un système de fichier.
* **Extentions physiques (PE)** : La plus petite extension contigüe (par défaut 4MiB) dans le volume physique (PV) qui peut être
assigné à un volume logique (LV).

LVM permet une plus grande flexibilité comparé à l'utilisation d'une partition de disque dur classique :

* Utiliser plusieurs disques comme un seul gros disque.
* Avoir des volumes logiques étendus sur plusieurs disques.
* Créer de petits volumes logiques et les retailler "dynamiquement" quand ils se remplissent.
* Redimensionner des volumes logiques quelque soit leur ordre sur le disque.
* Capturer une sauvegarde d'une copie gelée du système de fichier, en gardant une forte disponibilité.
* Supporter différentes cibles de *device-mapper*, incluant le chiffrement de systèmes de fichier transparent et la mise en
cache de données fréquemment utilisées. Ceci permet de créer un système avec un ou plusieurs disques physiques (chiffrés avec
LUKS) et LVM par dessus pour permettre de redimensionner et de gérer les volumes séparés sans avoir à entrer une clef de
nombreuses fois au démarrage.

Les désavantages étant :

* Des étapes supplémentaire lors de l'installation du système. Nécessite l'exécution d'un certain nombre de daemons.
* Le dual booting avec des SE ne supportant pas LVM (Windows).
* Dans le cas de volumes physique qui ne sont pas en RAID-1, RAID-5 ou RAID-6 perdre un disque peut signifier perdre un ou
plusieurs volumes logiques si on étale (ou étend) des volumes logiques sur un ensemble de disques non redondants.

#### RAID

RAID est une technologie de stockage qui combine plusieurs composants de disques durs (typiquement des disques durs ou leurs
partitions) en une unité logique. Selon l'implémentation du RAID, cette unité logique peut être un système de fichier ou une
couche transparente additionnelle qui détient plusieurs partitions. Les données sont distribuées sur les disques d'une ou
plusieurs manières appelés niveaux de RAID. Le niveau de RAID choisit peut ainsi empêcher des pertes de données dans le cas
d'une perte de disque dur, améliorer la performance ou une combinaison des deux.

Il existe de nombreux niveaux de RAID différents ; ceux listés ci-dessous sont les plus communs :

* **RAID 0** : Entrelacement de disques. Augmente la vitesse de lecture et d'écriture sans redondance.
* **RAID 1** : Disques en miroir.
* **RAID 5** : Nécessite au minimum 3 disques et combine la redondance du RAID 1 et les bénéfices de vitesse du RAID 0.
* **RAID 6** : Nécessite au minimum 4 disques fournit les bénéfices du RAID 5 mais avec la sécurité contre la perte de deux
disques.
* **RAID 1+0** : RAID imbriqué qui combine deux niveaux standards du RAID pour gagner en performance et redondance
additionnelle.

Les périphériques de RAID peuvent être gérés de plusieurs façons :

* **RAID Logiciel** : La grappe est gérée par le système d'exploitation soit par :
    - Une couche d'abstraction (mdadm);
    - Un gestionnaire de volume logique (LVM);
    - Un composant du système de fichier (ZFS, Btrfs);
* **RAID Matériel** : La grappe est directement gérée par une carte matérielle dédiée installée dans le PC auxquels les disques
sont directement connectés. La logique RAID s'exécute sur un processeur embarqué indépendamment du processeur hôte (CPU). Bien
que cette solution soit indépendante du système d'exploitation, cette dernière nécessite un pilote pour fonctionner correctement
avec le contrôleur de RAID matériel. La grappe de RAID peut soit être configurée via une interface microgicielle ou, selon le
fabricant, à l'aide d'application dédiées après installation du système d'exploitation. La configuration est transparente pour
le noyau Linux : il ne voit pas les disques séparément.
* **FakeRAID** : Ce type de raid est correctement appelé RAID microgiciel. La grappe est gérée par un pseudo contrôleur RAID où
la logique RAID est implémentée dans une partie microgicielle , mais sans toutes les capacité des contrôleurs RAID matériels.

#### Dm-crypt

Dm-crypt est la cible cryptographique du *device-mapper*. Il s'agit d'un sous-système de chiffrement de disque transparent du
noyau Linux. Il est implémenté en tant que cible du *device-mapper* et peut être emplilé sur d'autres transformations de
celui-ci. Il peut ainsi chiffrer des disques, des partitions, des volumes logiciels de RAID, des volumes logiques, ainsi que des
fichiers. Il apparaît comme un périphérique de bloc, qui peut être utilisé pour supporter des systèmes de fichiers, du swap ou
un volume physique LVM.

#### LUKS

LUKS est une spécification de chiffrement de disque. LUKS implémente un format sur disque standard indépendant pour
l'utilisation de divers outils. Cela permet une compatibilité et une interopérabilité parmis différents programmes, mais
s'assure également qu'ils implémentent une gestion des mots de passe sécurisée et documentée.

LUKS utilise dm-crypt comme backend pour le chiffrement de disque.

#### Chargeur d'amorçage

Un chargeur d'amorçage est un logiciel démarré par le microgiciel (BIOS ou UEFI). Il est responsable du chargement du noyau avec
les paramètres recommandés et du disque RAM initial (initrd) selon  des fichiers de configuration. Dans le cas de l'UEFI, le
noyau lui-même peut directement être lancé par l'EFI boot stub qui permet au microgiciel EFI un chargement du noyau en tant
qu'exécutable EFI. Un chargeur d'amorçage séparé ou gestionnaire de démarrage peut toujours être utilisé afin d'éditer les
paramètres du noyau avant le démarrage.

Un chargeur d'amorçage doit pouvoir accéder au images du noyau et de l'initramfs, autrement le système ne pourra pas démarrer.
Par conséquent, il doit typiquement pouvoir accéder à **/boot**. C'est à dire, qu'il doit pouvoir supporter un ensemble allant
des périphériques de blocs, aux périphériques de blocs empilés (LVM, RAID, dm-crypt; LUKS, etc.) jusqu'au système de fichier sur
lequel réside les images du(des) noyau(x) et de(des) initramfs.

Le chargement du disque RAM initial peut également inclure un microcode processeur du fabricant qui fournit des mises à jours de
sécurité et de stabilité.

Les mises à jour du microcode sont généralement incluses dans le microgiciel de la carte mère et appliquées durant
l'initialisation du microgiciel. Du fait que les fabriquants des équipements d'origine ne mettaient pas forcément à jour leur
microgiciel de manière régulière et que les vieux système ne bénéficiaient d'aucune mise à jour, la possibilité d'appliquer ses
mises à jour du microcode processeur a été ajouté au moment du démarrage pour le noyau Linux.

#### Initramfs

Une fois que le chargeur d'amorçage a chargé le noyau et les possibles fichiers initramfs et exécute le noyau, le noyau extrait
l'initramfs (système de fichier RAM initial) dans le rootfs (système de fichier racine initial, spécifiquemement un ramfs ou
tmpfs) qui était vide jusqu'alors. La premier initramfs extrait a été inclus dans le binaire du noyau lors de la construction de
celui-ci, ensuite de possibles fichiers initramfs externes sont extraits. Par conséquent les initramfs externes surchargent des
fichiers portant le même nom dans l'initramfs inclus. Le noyau execute alors **init** (dans le rootfs) en tant que premier
processus. Les prémisses de l'espace utilisateur démarrent.

Les images externes initramfs peuvent être générées à l'aide de **mkinitcpio**, **dracut** ou **booster**.

Le but de l'initramfs est d'amener le système jusqu'au point où il accède au système de fichier racine. Cela signifie que tout
module requis pour des périphériques tels qu'IDE, SCSI, SATA, USB/FW (si démarrage depuis un disque externe) doit pouvoir être
chargé depuis l'initramfs si il n'est pas inclus dans le noyau ; une fois que les modules sont chargés (soit explicitement via
un programme ou un script, ou implicitement via udev), le processus de démarrage continue. Pour cette raison, l'initramfs a
uniquement besoin des modules nécessaires à l'accès au système de fichier racine. La majorité des modules sera chargé à
postériori par udev, durant le processus d'init (systemd).

#### Noyau

Le noyau est un programme informatique au cœur d'un système d'exploitation informatique et a un contrôle complet sur l'ensemble
du système. Il s'agit de la partie du code du système d'exploitation qui réside en permanence en mémoire, et facilite les
interactions entre composants matériels et logiciels. Un noyau complet contrôle l'ensemble des ressources matérielles (ex. E/S,
mémoire, cryptographie) via des pilotes, arbitre les conflit entre processus a propos de ces ressources, et optimise
l'utilisation des ressources communes ex. CPU & utilisation des caches, systèmes de fichiers, et sockets réseaux. Sur la
majorité des systèmes, le noyau est le premier programme chargé au démarrage (après le chargeur d'amorçage). Il gère le reste du
processus de démarrage ainsi que la mémoire, les périphériques, les requêtes d'entrées/sorties (E/S) du logiciel, en les
traduisant en instructions de traitements de données pour le processeur.

Le code critique du noyau est généralement chargé dans une aire de la mémoire séparée, qui est protégée des accès via les
applications logicielles ou d'autres parties moins critiques d'un système d'exploitation. Le noyau effectue des tâches telles
que, l'exécution de processus, la gestions de périphériques matériels tels que le disque dur, et la gestion des interruptions,
dans l'espace noyau protégé. Par contraste, des programmes applicatifs tels que des navigateurs, des traitements de texte, ou
des lecteurs audio ou vidéo utilisent une aire séparée de la mémoire, l'espace utilisateur. Cette séparation empêche les données
utilisateurs et les données noyau d'interférer l'une avec l'autre causant instabilité et lenteurs, empêchant également des
applications défectueuses d'affecter d'autres applications ou même le système d'exploitation dans son ensemble.

L'interface noyau est une couche d'abstraction de bas niveau. Quand un processus demande un service au noyau, il doit invoquer
un appel système, généralement à travers une fonction englobante.

Il existe différentes architectures de noyau. Les noyaux monolithiques s'exécutent entièrement dans un espace d'adressage unique
avec le processeur en mode supervision, principalement pour des questions de rapidité. Les micro-noyaus exécutent la plupart de
leurs services dans l'espace utilisateur, de la même manière que les processus utilisateurs, principalement pour des
considérations de résilience et de modularité. Le noyau Linux est monolithique, bien qu'également modulaire, puisqu'il peut
insérer et supprimer des module noyaux chargeable à l'exécution.

#### Talon de démarrage UEFI et Image noyau unifiée

Une image noyau unifiée (UKI) est un exécutable unique qui peut être démarré directement depuis le microgiciel UEFI, ou sourcé
automatiquement par les chargeurs d'amorçage avec une configuration légère voire inexistante.

Une image unifiée permet d'inclure :

* un talon de démarrage UEFI tel que *systemd-stub*,
* une image noyau,
* une image initramfs,
* l'interface ligne de commande du noyau,
* optionnelement, un écran de démarrage.

Grâce au talon, l'exécutable résultant, et par conséquent tous ses éléments, peuvent être facilement signés afin d'utiliser la
fonctionnalité de démarrage sécurisé UEFI (Secure Boot).

#### Cgroups

Les cgroups sont une fonctionnalité du noyau Linux qui limite, compte et isole l'utilisation des ressources (CPU, mémoire,
entrée/sortie, réseau, etc.) d'une collection de processus.

Ces groupes de contrôle peuvent être utilisés de multiples façons :

* En accédant le système de fichier virtuel du cgroup manuellement.
* En créant et gérant des groupes à la volée en utilisant des outils tels que *cgcreate*, *cgexec* et *cgclassify*
(*libcgroup*).
* A travers le "daemon moteur de règles" qui peut déplacer les processus de certains utilisateurs, groupes de manière
automatique ou commander aux cgroups comme cela a été spécifié dans sa configuration.
* Indirectement à travers d'autres logiciels utilisant les cgroups, tels que Docker, LXC, libvirt, systemd, etc.

#### Espaces de noms

Un espace de noms encapsule dans une abstraction une ressource système globale qui la fait apparaître aux processus contenus
dans l'espace de noms qui ont leur propre instance isolée de la ressource globale. Les changements de la ressource globale sont
visibles aux autres processus membre de l'espace de noms, mais invisible aux autres processus. Les espaces de noms sont utilisés
pour implémenter les conteneurs.

Les types d'espace de noms disponibles dans Linux sont les suivants : Cgroup, IPC, Network, mount, PID, Time, User, UTS.

#### Systemd

Systemd est un système et un gestionnaire de service pour les systèmes d'exploitation Linux. Lorsqu'il est exécuté en tant que
premier processus au démarrage, il agit comme le système init et met en place et gère les services de l'espace utilisateur. Des
instances séparées sont démarrées pour les utilisateurs connectés afin de démarrer leurs services.

D'habitude, systemd n'est pas invoqué directement par l'utilisateur, mais est installé comme le lien symbolique /sbin/init et
lancé au début du démarrage.

Lorsqu'il est exécuté en tant qu'instance système, systemd interprète le fichier de configuration system.conf et les fichiers
présents dans les répertoires de system.conf.d ; lorsque lancé en tant qu'instance utilisateur, systemd interprète le fichier de
configuration user.conf et les fichiers dans les répertoires user.conf.d

systemd fournit un système de dépendances entre des entités variées appelées "unités" de 11 types différents. Les unités
encapsulent des objets utiles pour le démarrage et la gestion du système. La grande majorité des unités sont configurées dans
des fichiers de configuration d'unité, néanmoins certaines peuvent être créées automatiquement depuis d'autres configurations,
dynamiquement d'un état système ou de façon programmée à l'exécution. Une unité peut être "active" (i.e. démarrée, liée,
connectée, etc. selon le type d'unité), ou "inactive" (i.e. stoppée, déliée, déconnectée, etc.) ou bien dans le processus
d'activation ou de désactivation, c'est à dire entre les deux états. Un état spécial "échoué" est également disponible, très
similaire à "inactive" il se déclenche lorsque le service a échoué d'une certaine façon (le processus a renvoyé un code
d'erreur, ou a crashé, une opération a time out, ou après un nombre trop important de redémarrages). Si l'état apparaît, la
cause sera tracée pour référence ultérieure. Il est à noté que plusieurs types d'unité peuvent avoir un nombre de sous-états
additionnels, qui sont reliés aux 5 états d'unité généraux décrits ici.

Les unités suivantes sont disponibles :

1. Les unités de **services**, qui démarrent et contrôlent des daemons et les processus qui les composent.
2. Les unités de **sockets**, qui encapsulent des IPC locaux ou des sockets réseaux du système, utiles pour l'activation basée
sur des sockets.
3. Les unités de **cibles** utilisées afin de grouper des unités, ou de fournir des points de synchronisation connus au
démarrage.
4. Les unités de **périphériques** qui expose les périphériques de noyau dans systemd et peuvent être utilisées pour de
l'activation basée sur des périphériques.
5. Les unités de **montages**, pour contrôler les points de montage dans le système de fichiers.
6. Les unités de **montages automatiques**, afin d'automatiser les opérations de montage à la demande et le démarrage
parallélisé.
7. Les unités de **timers** afin de déclencher l'activation d'autres unités sur la base de timers.
8. Les unités de **swap**, très similaires aux unités de montages qui encapsulent les partitions mémoires dédiées au swap ou des
fichiers du système d'exploitation.
9. Les unités de **chemins** qui peuvent être utilisées afin d'activer d'autres service lorsque les objets du système de
fichiers changent ou sont modifiés.
10. Les unités de **tranches** qui sont utilisées afin de grouper des unités qui gèrent des processus systèmes dans un arbre
hiérarchique pour des questions de gestion des ressources.
11. Les unités de **scope** similaires aux unités de services mais pour gérer et démarrer des processus externes. Ils sont créés
de façon programmée en utilisant les interfaces de bus de systemd.

> Units : service, socket, target, device, mount, automount, timer, swap, path, slice, scope

Systemd chargera implicitement et automatiquement les unités depuis le disque - si celles-ci ne sont pas déjà chargées - dès
qu'elles seront requises par des opérations. Ainsi, le fait qu'une unité est chargée ou non est invisible aux clients. On
utilise `systemctl list-units --all pour lister de manière compréhensive les unités actuellement chargées. N'importe quelle
unité pour laquelle aucune de conditions ci-dessus ne s'applique est rapidement déchargée. Il est à noter que quand une unité
est déchargée de la mémoire courante ses données sont également libérées. Néanmoins les données ne sont généralement pas
perdues, du fait qu'un enregistrement dans le journal de log est généré déclarant les ressources consommées à l'arrêt d'une
unité.

Les processus lancés par systemd sont placés dans des cgroups nommés d'après l'unité à laquelle ils appartiennent dans la
hiérarchie privée de systemd. systemd fait usage de cela afin de garder efficacement la trace des processus. L'information du
Control group est gérée par le noyau, et est accessible dans la hiérarchie du système de fichier (sous /sys/fs/cgroup/systemd/),
ou bien dans des outils tels que systemd-cgls.

Systemd contient un système transactionnel minimal : si une unité doit s'activer ou se désactiver, il l'ajoutera avec toutes ses
dépendances dans une transaction temporaire. Il vérifiera alors si la transaction est cohérente (i.e. si l'ordonnancement de
toutes les unités ne contient pas de boucle). Si ce n'est pas le cas, systemd essaiera de résoudre le problème, et supprimera
les jobs non essentiels de la transaction ce qui pourra faire disparaître la boucle. Systemd essaiera également de supprimer les
jobs non essentiels qui arrêteraient un service déjà actif. Enfin il vérifiera si les jobs de la transaction contredisent des
jobs déjà en attente dans la file et optionnellement annulera la transaction. Si tout a marché et que la transaction est
cohérente et minimale dans son impact, elle est fusionnée avec tous les jobs en attente dans la file d'exécution. Cela signifie
qu'avant l'exécution d'une opération demandée, systemd vérifiera en premier lieu qu'elle ait un sens, essaiera de résoudre si
possible, et échouera uniquement s'il n'est pas possible de la faire fonctionner.

Il est à noter que les transactions sont générées indépendamment de l'état des unités à l'exécution, ce qui a pour conséquence
par exemple que si un job de démarrage est requis sur une unité déjà active, il génèrera quand même une transaction et
réveillera toute dépendance inactive (et causera par propagation la demande d'autres jobs tels que définis par leurs relations).
Ceci du fait que le job enfilé est comparé à l'exécution à l'état de l'unité cible et est marqué comme réussi et complet quand
les deux sont satisfaits. Cependant ce job ressort également les autres dépendances du fait de la relation définie et mène donc,
dans notre exemple, à l'enfilage des jobs de démarrage de toutes ces unités inactives.

Systemd contient des implémentation natives de tâches diverses faisant parti du processus de démarrage et ayant besoins d'être
exécutées. Par exemple, il fixe le nom de l'hôte ou configure le périphérique réseau de loopback. Il sert également à mettre en
place et monter plusieurs systèmes de fichiers d'API, tels que /sys/ ou /proc/.

#### Shell

Le Shell est un programme qui permet d'interpréter les commandes de l'utilisateur. C'est l'un des tout premiers moyens
d'interagir avec un ordinateur. Le shell est généralement plus puissant qu'une interface graphique utilisateur (GUI), dans le
sens où il permet d'accéder très efficacement aux fonctionnalités internes du système d'exploitation (OS).

Souvent les outils textuels dont il dispose sont construits de manière à pouvoir être composés. Ainsi de multiples assemblages
permettent à la fois une simplicité dans la décomposition des tâches, et une facilité de mise en oeuvre dans l'automatisation.

Les shells peuvent généralement dépendre des OS, sachant qu'il en existe une quantité pour chacun d'entre eux.

### Éléments de sécurité

#### Secure Shell

Secure Shell (SSH) est un protocole réseau cryptographique pour opérer des services réseaux de manière sécurisé à travers un
réseau non sécurisé. Les application typiques incluent la connexion distante en ligne de commande et l'exécution de commandes
distantes, mais n'importe quel service réseau peut être sécurisé via SSH.

Un serveur SSH, écoute par défaut sur le port 22 TCP. Un programme client SSH est généralement utilisé pour établir la connexion
vers un daemon sshd qui accepte les connexions distantes.

#### OpenSSH

OpenSSH (OpenBSD Secure Shell) est un ensemble de programmes informatiques fournissant des sessions de communications chiffrées
à travers un réseau en utilisant le protocole Secure Shell. Il a été créé en tant qu'alternative open source à la suite
logicielle propriétaire Secure Shell offerte par SSH Communications Security.

On utilise généralement la suite d'outils OpenSSH suivante :

* **ssh-keygen** pour générer une paire clef publique/clef privée.
* **ssh-add** pour ajouter les identités de clefs privées à l'agent d'authentification *ssh-agent*.
* **ssh-agent** en tant que service gestionnaire de clefs.
* **ssh** en tant que client du protocole.
* **sshd** en tant que serveur du protocole.

#### Netfilter

Netfilter est un framework fournit par le noyau Linux et qui permet diverses opérations en lien avec le réseau pouvant être
implémentées sous la forme de gestionnaires configurables. Netfilter offre plusieurs fonctions et opérations pour le filtrage
des paquets, la traduction d'adresses et de ports réseaux, ce qui fournit la fonctionnalité requise pour diriger les paquets à
travers le réseau et d'interdire à certains l'accès aux lieux sensibles d'un réseau.

Netfilter présente un ensemble de hooks à l'intérieur du noyau Linux, permettant à des modules du noyau spécifiques
d'enregistrer des fonctions de rappel avec la pile réseau du noyau. Ces fonctions s'appliquent généralement au traffic sous
forme de règles de modification et de filtrage qui sont appelées pour chaque paquet qui traverse le hook respectif dans la pile
réseau.

nftables est un sous-système du noyau Linux et la nouvelle partie de Netfilter qui fournit la classification et le filtrage des
paquets réseaux. Elles sont disponibles depuis le noyau Linux 3.13. nftables se substitue aux iptables, elles présentent les
avantages d'une moindre réplication du code et d'une plus grande simplicité d'extension pour de nouveaux protocoles. nftables
est configuré via l'utilitaire d'espace utilisateur *nft*.

Nftables utilise les blocs de construction de l'infrastructure Netfilter, tels que les hooks existant dans la pile réseau, la
connexion au système de traçage, le composant d'enfilage de l'espace utilisateur, et le sous-système de logs.

Le moteur du noyau nftables ajoute une machine virtuelle simpliste au noyau Linux, capable d'exécuter un bytecode pour inspecter
un paquet réseau et prendre les décisions concernant la gestion de ce paquet. Elle peut obtenir des données de la part du paquet
lui-même, regarder les méta données associées (par exemple l'interface d'entrée), et gérer les données de traçage de
connexions. Les opérateurs de comparaison arithmétiques et bits à bits peuvent être utilisés pour prendre des décisions en
fonction de ces données. La machine virtuelle est aussi capable de réaliser des manipulations sur des ensembles de donnéees
(typiquement des adresses IP), en permettant de remplacer de multiples opérations de comparaisons par un ensemble d'inspections.

Nftables offre une API améliorée côté espace utilisateur qui permet des remplacements atomiques d'une ou plusieurs règles
pare-feu dans une seule transaction Netlink. Ceci accélère les changements de configuration pare-feu pour les installations
ayant de grands ensembles de règles ; cela peut également permettre d'éviter des situations de compétition durant le changement
de règle.

#### BPF et eBPF

Le filtre de paquet BSD (BPF) est une architecture de capture de paquets en espace utilisateur. Celle-ci repose sur une machine
virtuelle à registre qui fonctionne dans le noyau et permet d'évaluer les règles de filtrage sans recopies des paquets. Ce
mécanisme est utilisé par des outils standards comme *tcpdump* pour sélectionner les paquets à capturer.

Récemment ce mécanisme à évolué vers une version étendue (eBPF) qui est la version moderne d'architecture utilisée : 

* passage de 2 registres 32 bits à 10 registres 64 bits
* possibilité d'appeler certaines fonctions du noyau

eBPF permet d'exécuter des programmes contrôlés (absence de boucles, pointeurs vérifiés etc.) dans l'espace noyau à l'aide d'un
compilateur JIT qui transforme le programme BPF en code natif.

Les programmes eBPF peuvent être utilisés pour implémenter des fonctionnalités de supervision, de sécurité et de réseautage
hautes performances.

#### IDS

Un système de détection d'intrusion (IDS) est un appareil ou une application logicielle qui surveille un réseau ou des systèmes
afin de cibler et de détecter des activités malveillantes ou des violations de la politique de sécurité. Chaque activité
d'intrusion ou de violation est généralement rapportée soit à un administrateur ou centralisée dans un système de gestion
d'événement et d'information de sécurité (SIEM). Un système SIEM combine les sorties de sources multiples et utilise des
techniques de filtrage d'alarme afin de distinguer une activité malveillante d'une fausse alarme.

#### Contrôle d'accès discrétionnaire et droits

Le contrôle d'accès discrétionnaire (DAC) est un moyen de limiter l'accès aux objets basés sur l'identité des sujets ou des
groupes auxquels ils appartiennent. Le contrôle est discrétionnaire car un sujet avec une certaine autorisation d'accès est
capable de transmettre cette permission à n'importe quel autre sujet (sauf restriction du contrôle d'accès obligatoire).

Sous linux, les acteurs sont :

* l'utilisateur propriétaire (*user*)
* le groupe principal de l'utilisateur (*group*)
* les autres (*others*)

Les droits usuels s'organisent ainsi :

* pour les fichiers :
    - r autorise à lire le contenu du fichier
    - w autorise à modifier le contenu du fichier
    - x autorise l'exécution du fichier.
* pour les répertoires :
    - r autorise à lister le contenu d'un répertoire
    - w autorise la création, renommage et suppression de fichier
    - x autorise à traverser (entrer) dans le répertoire.

D'autres droits existent notamment :
    - t le sticky bit principalement utilisé sur les répertoires et qui permet d'éviter la suppression du répertoire par
      d'autres utilisateurs bien qu'ils puissent avoir des droits en écriture sur le contenu de ce même répertoire.
    - s le setuid et setgid qui permettent d'exécuter un fichier avec les mêmes permissions que son propiétaire ou
      respectivement son groupe. Setgid permet également de modifier le comportement d'un répertoire en fixant le groupe d'un
      fichier à sa création.

#### Listes de contrôle d'accès ACL

Les listes de contrôle d'accès sont une extension du contrôle d'accès discrétionnaire et des droits usuels.

#### SELinux

SELinux (*Security Enhanced Linux*) est un module de sécurité du noyau Linux qui fournit des politiques de sécurité de contrôle
d'accès, dont le contrôle d'accès obligatoire (MAC) en addition au contrôle d'accès discrétionnaire existant (DAC). SELinux
répond fondamentalement aux questions telles que :

"Est-ce que **le sujet** peut faire cette **action** à cet **objet** ?" e.g : "Est-ce qu'un serveur web peut accéder aux
fichiers dans le répertoire home des utilisateurs ?"

Un noyau Linux intégrant SELinux impose des politiques de contrôles d'accès obligatoire qui confinent les programmes
utilisateurs et les services systèmes, ainsi que les accès aux fichiers et aux ressources réseaux. Limiter les privilèges au
minimum requis pour fonctionner réduit ou élimine les capacités de ces programmes et daemons à causer des dommages si ceux-ci
sont compromis ou défaillants (par exemple via des dépassements de tampons ou des mauvaises configurations). Ce mécanisme de
confinement fonctionne indépendamment des mécanismes de contrôle d'accès discrétionnaire traditionnels de Linux. Le concept de
super utilisateur n'existe pas, et ne partage pas les raccourcis bien connus des mécanismes de sécurité traditionnels, tel
qu'une dépendance aux binaires setuid/setgid.

Lorsqu'une application ou un processus, reconnu comme un sujet, effectue une requête d'accès à un objet, tel qu'un fichier,
SELinux vérifie à l'aide d'un cache de vecteur d'accès (AVC), où les permissions des sujets et des objets sont mises en cache.

Si SELinux ne trouve pas matière à trancher à propos de cet accès dans le cache, il envoit une requête au serveur de sécurité.
Le serveur de sécurité vérifie le contexte de sécurité de l'application ou du processus et du fichier. Le contexte de sécurité
est appliqué depuis la base de données de politiques SELinux. La permission est donnée ou refusée.

Si la permission est refusée, un message "avc: denied" sera visible dans */var/log.messages*.

Chaque processus et ressource système a une étiquette spéciale de sécurité appelée contexte SELinux. Un contexte SELinux est un
identifiant qui s'abstrait des détails du systèmes et se concentre sur les propriétés de sécurité de l'objet. Cela permet un
référencement des objets cohérent dans la politique SELinux mais supprime également toute ambiguité que l'on peut retrouver avec
d'autres méthodes d'identification ; par exemple, un fichier peut avoir plusieurs noms de chemins valides sur un système qui
utilise des montages liés.

La politique SELinux utilise ces contextes dans une série de règles qui définissent comment un processus peut intéragir avec les
autres ainsi que les différentes ressources systèmes. Par défaut, la politique ne permet aucune intéraction à moins qu'une régle
explicite en permette l'accès.

Le contexte SELinux contient plusieurs champs : utilisateur, role, type, et niveau de sécurité. L'information du type SELinux
est sans doute la plus importante quand il s'agit de la politique SELinux, du fait que la règle la plus commune de la politique
SELinux qui définit les intéractions autorisées entre les processus et les ressources systèmes utilise les types SELinux et non
le contexte en entier. Le type SELinux finit habituellement par *_t* (e.g. *httpd_t*).

#### PAM

Les modules d'authentification attachables linux (PAM) fournissent un quadriciel pour une authentification de l'utilisateur sur
l'ensemble du système. Cela permet de développer des programmes indépendants du schéma d'authentification. Ces programmes ont
besoin que des "modules d'authentification" leurs soient attachés à l'exécution pour fonctionner. Le module d'authentification
attaché au programme est à la discrétion de l'administrateur système et dépend de l'installation locale.

### Virtualisation et conteneurisation

#### Systemd-nspawn

Systemd-nspawn peut être utilisé pour exécuter une commande ou un OS dans un conteneur léger d'espace de noms. Il est plus
puissant que *chroot* puisqu'il virtualise la hiérarchie du système de fichiers, mais aussi l'arbre des processus, les
différents sous-systèmes IPC ainsi que le nom de l'hôte et du domaine.

Systemd-nspawn limite l'accès en lecture seule à différentes interfaces du noyau dans le conteneur, telles que **/sys**,
**/proc/sys** ou **/sys/fs/selinux**. Les interfaces réseaux et l'horloge système ne peuvent pas être modifiées depuis
l'intérieur du conteneur. Les fichiers spéciaux ou fichiers de périphérique ne peuvent  pas non plus être créés. Le système hôte
ne peut pas être redémarré et des modules du noyau ne peuvent pas être chargés depuis le conteneur.

Les conteneurs ainsi créés peuvent être gérés à l'aide de la commande *machinectl*.

#### Conteneurisation LXC

LXC est une méthode de virtualisation au niveau du système d'exploitation permettant d'exécuter plusieurs systèmes isolés Linux
sur un système hôte de contrôle en utilisant un unique noyau Linux.

Le noyau Linux fournit la fonctionnalité des cgroups qui permet une limitation et une priorisation des ressources (CPU, mémoire,
entrées/sorties, réseau, etc.) sans besoin d'aucune machine virtuelle, ainsi que la fonction d'isolation par espace de noms qui
permet l'isolation complète d'une application du point de vue de l'environnement opérant, incluant l'arbre des processus, la
configuration réseau, les identifiants utilisateurs et les systèmes de fichiers montés.

LXC combine les cgroups du noyau et inclut l'isolation des espaces de nom pour fournir un environnement isolé à des
applications.

LXC permet d'exécuter des conteneurs en tant que simple utilisateur sur l'hôte à l'aide des conteneurs dits "non-privilégiés".

#### Conteneurisation Docker

Docker est un ensemble de produits de Plateforme en tant que Service qui utilise la virtualisation au niveau du système
d'exploitation pour livrer des logiciels dans des paquets appelé conteneurs. Les conteneurs sont isolés les uns des autres et
embarquent leurs propres logiciels, bibliothèques et fichiers de configuration ; ils peuvent communiquer entre eux à travers des
canaux bien définis. Tous les conteneurs sont exécuté par un noyau de système d'exploitation unique et par conséquent utilisent
moins de ressources que des machines virtuelles.

Docker peut empaqueter une application et ses dépendances dans un conteneur virtuel qui peut s'exécuter sur n'importe quel
ordinateur Linux, Windows, ou macOS. Ceci permet à l'application de s'exécuter dans toute une variété d'environnement, par
exemple, sur sa propre machine, dans un cloud public ou privé. Quand il s'exécute sur Linux, Docker utilise les mécanismes
d'isolation fournis par le noyau (tels que les cgroups et les espaces de noms) et les systèmes de fichiers capables de montage
en union, pour permettre aux conteneurs de s'exécuter sur une instance Linux unique, évitant la surcharge du démarrage et de la
maintenance de machines virtuelles.

Le support des espaces de nom du noyau Linux permet d'isoler l'environnement système de la vue de l'application comme l'arbre
des processus, le réseau, les identifiants utilisateurs et les systèmes de fichiers montés, tandis que les cgroups fournissent
la limitation de ressources mémoire et CPU. Docker inclut également sa propre bibliothèque *libcontainer* permettant d'accéder
directement les fonctions de virtualisation fournies par le noyau Linux en plus de l'utilisation d'interfaces de virtualisation
telles que *libvirt*, *LXC* et *systemd-nspawn*.

Docker implémente une API de haut niveau pour fournir des conteneurs légers exécutant des processus isolés.

Le logiciel Docker en tant qu'offre de services consiste en trois composants :

* **Le Logiciel** : Le daemon Docker, *dockerd*, est un processus persistant qui gère les conteneurs Docker ainsi que les objets
du conteneur. Le daemon est à l'écoute des requêtes envoyés via l'API du Docker Engine. Et le programme client *docker* fournit
une interface de ligne de commande qui permet d'interagir avec le daemon.
* **Les objets** : Les objets docker sont des entités diverses utilisées pour assembler une application dans Docker. Les classes
principales des objets Dockers sont les images, les conteneurs et les services.
    + Un conteneur Docker est un environnement encapsulé, standardisé qui exécute des applications. Un conteneur est géré à
    travers l'API Docker ou la ligne de commande.
    + Une image Docker est un modèle en lecture seule utilisée pour construire des conteneurs. Les images sont utilisées pour
    stocker et envoyer des applications.
    + Un service Docker permet une mise à l'échelle des conteneurs sur de multiples daemons. Ceci étant appelé une nuée
    (*swarm*), un ensemble de daemon coopérant, communiquant à travers l'API Docker.
* **Les registres** : Un registre Docker est un dépôt d'image Docker. Les clients Docker se connectent aux registres pour cloner
(*pull*) des images à utiliser ou déposer (*push*) des images qu'ils ont construites. Les dépôts peuvent être publics ou privés.
Les deux principaux dépôts publics sont Docker Hub et Docker Cloud. Docker Hub est le registre par défaut ou Docker recherche
des images. Des registres Docker permettent également la création de notifications basée sur des évènements.

Le logiciel Docker dispose également d'outils :

* **Docker Compose** qui est un outil qui permet de définir et d'exécuter des applications Docker multi-conteneurs. Il utilise
des fichier YAML pour configurer les services de l'application et créé et démarre les processus de tous les conteneurs à l'aide
d'une seule commande. L'interface en ligne de commande *docker-compose* permet aux utilisateurs de passer des commandes sur des
ensembles de conteneurs, par exemple pour construire des images, déployer des conteneurs, redémarrer des conteneurs, etc. Les
commandes liées à la manipulation d'image, ou les options interactives sont inutiles dans Docker Compose car elle s'adressent à
un conteneur unique. Le fichier docker-compose.yml est utiliser pour définir les services de l'application et inclut plusieurs
options de configuration. Par exemple, l'option *build* définit les options de configuration telles que le chemin du Dockerfile,
l'option *command* permet de surcharger les commandes Docker par défaut, etc.
* **Docker Swarm** fournit nativement un fonctionnalité de grappe pour les conteneurs Docker qui transforme un groupe de Docker
engine en un unique Docker engine virtuel. L'interface ligne de commande *docker swarm* permet aux utilisateurs d'exécuter des
conteneurs Swarm, de créer des mots-clefs, lister les noeuds de la grappe, etc. L'interface ligne de commande *docker node*
permet aux utilisateurs d'exécuter diverses commandes pour gérer des noeuds dans la nuée, par exemple, lister les noeuds de la
nuée, mettre à jour les noeuds, supprimer les noeuds d'une nuée. Docker gère les nuées en utilisant l'algorithme de consensus
Raft. Selon Raft, pour qu'une mise à jour se fasse, la majorité des noeuds de la nuée doivent s'accorder sur celle-ci.

#### Orchestrateur Kubernetes

Kubernetes est un système d'orchestration de conteneurs open source permettant d'automatiser le déploiement, la mise à l'échelle
et la gestion d'applications informatiques.

Beaucoup de services de cloud offrent des plateformes ou infrastructures en tant que service (PaaS, IaaS) basées sur Kubernetes
sur lesquelles Kubernetes peut être déployé comme service fournisseur de plateformes.

Kubernetes définit un ensemble de primitives, qui collectivement fournissent des mécanismes de déploiement, de maintien et de
mise à l'échelle d'applications basé sur les ressources CPU, mémoire et autres métriques personnalisées. Kubernetes est
lâchement couplé et extensible pour s'accorder à différentes charges de travail. Cette extensibilité est fournit en grande
partie par l'API Kubernetes, utilisée par des composants internes ainsi que les extensions et conteneurs exécutés sur
Kubernetes. La plateforme exerce son contrôle sur les ressources de calcul et de stockage et définissant ces ressources comme
objets, ceux-ci pouvant par la suite être gérés comme tels.

Kubernetes suit l'architecture Maître-Esclave. Les composants de Kubernetes peuvent être divisés entre ceux qui gèrent les
noeuds individuels et ceux qui font partie du plan de contrôle.

Le maître Kubernetes est l'unité principale de contrôle du cluster, il gère la charge de travail et dirige les communications à
travers le système. Le plan de contrôle de Kubernetes consiste en divers composants, chacun ayant sa propre tâche, pouvant être
exécuté soit sur un simple noeud maître, soit sur plusieurs maîtres pour des clusters à haute disponibilité. Les différents
composants du plan de contrôle Kubernetes sont les suivants :

* **etcd** : etcd est une base de données clef-valeur, légère, persistante et distribuée qui stocke de manière fiable les
données de configuration du cluster, donnant une représentation de l'état global du cluster à l'instant t. *etcd* est un système
qui favorise la cohérence à la disponibilité dans l'éventualité d'une partition réseau. Cette cohérence est cruciale pour
ordonnancer correctement les services opérants. Le serveur de l'API Kubernetes utilise l'API de visualisation d'etcd pour
surveiller le cluster et déployer des changements configuration critiques ou simplement restaurer n'importe quelle divergence
d'état du cluster tels qu'il était déclaré par celui qui l'a déployé.
* **Le serveur d'API** : Le serveur d'API est un composant clé qui sert l'API Kubernetes via des JSON en HTTP, qui fournit à la
fois les interfaces internes et externes à Kubernetes. Le serveur d'API traite et valide les requêtes REST et met à jour l'état
des objets dans etcd, de fait permettant aux clients de configurer les charges de travail et les conteneurs à travers les
noeuds.
* **L'ordonnanceur** : L'ordonnanceur est un composant qui, sur la base de la disponibilité ressource, sélectionne un noeud sur
lequel s'exécute un pod non ordonnancé (entité de base géré par l'ordonnanceur). L'ordonnanceur suit l'usage ressource sur
chacun des noeuds afin de s'assurer que la charge de travail n'est pas planifiée en excès de la ressource disponible. A cette
fin, l'ordonnanceur doit connaître les conditions et disponibilités de la ressource et autres contraintes définies par
l'utilisateur, les directives politiques telles que la qualité de service, les conditions d'affinité ou de non-affinité, la
localisation des données etc. Le rôle de l'ordonnanceur est d'accorder la ressource disponible à la charge de travail demandée.
* **Le gestionnaire de contrôle** : Un contrôleur est une boucle de réconciliation qui amène l'état courant du cluster vers
l'état désiré du cluster, en communicant via le serveur d'API pour créer, mettre à jour et supprimer les ressources qu'il gère
(pods, services, extrémités, etc.). Le gestionnaire de contrôle est un processus qui gère un ensemble de contrôleurs du noyau
Kubernetes. Un type de contrôleur est un contrôleur de réplication, qui s'occupe de la réplication et de la mise à l'échelle en
exécutant un nombre de copies de pods spécifiées à travers le cluster. Il s'occupe également de créer des pods de remplacement,
si les noeuds sous-jacents sont en erreur. D'autres contrôleurs qui sont une partie du noyau Kubernetes incluent le contrôleur
DaemonSet pour exécuter exactement un pod sur chaque machine (ou sous-ensemble de machines), et un contrôleur de travail pour
exécuter des pods jusqu'à fin d'exécution par exemple pour des traitements batchs. L'ensemble des pods qu'un contrôleur gère est
déterminé par l'étiquette des sélecteurs faisant partie de la définition du contrôleur.

Un noeud, est une machine où des conteneurs (charge de travail) sont déployés. Chaque noeud à l'intérieur du cluster doit
exécuter un environnement d'exécution de conteneurs tel que Docker, ainsi que les composants mentionnés ci-dessous, à des fins
de communication avec le maître pour la configuration réseau de ces conteneurs.

* **Kubelet** : Kubelet est responsable de l'état d'exécution de chaque noeud, il s'assure que tous les conteneurs du noeud sont
sains. Il prend en charge le démarrage, l'arrêt et la maintenance des conteneurs d'application organisés en pods tels que l'a
décidé le plan de contrôle. Kublet surveille l'état d'un pod, s'il n'est pas dans l'état désiré, le pod est redéployé sur le
même noeud. Le statut du noeud est relayé sur une période de quelques secondes via des messages au maître. Si le maître détecte
un échec de noeud, le contrôleur de réplication observe ce changement de statut et lance des pods sur d'autres noeuds sains.
* **Kube-proxy** : Le Kube-proxy est une implémentation d'un proxy réseau et d'un répartiteur de charge, il prend en charge
l'abstraction de service ainsi que d'autres opérations réseau. Il est responsable du routage du traffic vers le conteneur
approprié basé sur l'adresse IP et le numéro de port de la requête qui arrive.
* **Environnement** d'exécution de conteneur : Un conteneur réside dans un pod. Le conteneur est le niveau de micro-service le
plus bas, qui contient l'application en cours d'exécution, les bibliothèques et leurs dépendances. Ils peuvent également avoir
une adresse IP externe.

L'unité d'ordonnancement de base dans Kubernetes est un pod. Un pod est un groupement de composants conteneurisés. Un pod
consiste en un ou plusieurs conteneurs garantis de se trouver sur le même noeud.

Chaque pod dans Kubernetes est assigné à une adresse IP unique à l'intérieur du cluster, permettant aux applications d'utiliser
des ports sans risques de conflits. A l'intérieur du pod, chaque conteneur peut faire référence à chaque autre sur le localhost,
mais un conteneur à l'intérieur d'un pod n'a aucun moyen de s'adresser directement à un autre conteneur dans un autre pod ; pour
cela, il doit utiliser l'adresse IP du pod.

Un pod peut définir un volume, tel qu'un répertoire du disque local ou un disque réseau, et l'exposer aux conteneurs du pod. Les
pods peuvent être gérés manuellement via l'API Kubernetes, ou leur gestion peut être déléguée à un contrôleur. De tels volumes
sont aussi la base des fonctionnalités Kubernetes de ConfigMaps (pour fournir un accès à la configuration à travers le système
de fichier visible au conteneur) et Secrets (pour fournir les certificats nécessaires à l'accès sécurisé à des ressources
distantes, en donnant uniquement aux conteneurs autorisés, ces certificats sur leur système de fichier visible).

La fonction d'un ReplicaSet est de maintenir un ensemble stable de pods répliqués pouvant être exécutés à tout moment. En tant
que tel, il est souvent utilisé pour garantir la disponibilité d'un nombre de pods identiques spécifique.

Les ReplicaSets est également un mécanisme de rassemblement qui permet à Kubernetes de maintenir pour un pod donné un nombre
d'instance défini à l'avance. La définition d'un ensemble de réplique utilise un sélecteur, dont l'évaluation résulte en
l'évaluation de tous les pods qui lui sont associés.

Un service Kubernetes est un ensemble de pods travaillant de concert, tel une couche d'une application multi-couche. L'ensemble
de pods qui constitue un service sont définis par un sélecteur d'étiquette. Kubernetes fournit deux modes de découverte de
service, en utilisant des variables d'environnement, ou en utilisant le DNS Kubernetes. La découverte de service assigne un
adresse IP fixe et un nom DNS au service, et réparti la charge du traffic en utilisant un DNS round-robin pour les connexions
réseaux à cette adresse IP au milieu des pods vérifiant ce sélecteur (même si des erreurs peuvent amener les pods à passer d'une
machine à une autre). Par défaut un service est exposé à l'intérieur d'un cluster (par exemple les pods back-end peuvent être
groupés en service, recevant des requêtes de la part des pods front-end réparties entre eux), mais un service peut également
être exposé en dehors d'un cluster (par exemple pour que les clients puissent accéder aux pods front-end).

Les systèmes de fichier dans le conteneur Kubernetes fournit par défaut un stockage éphémère. Cela signifie qu'un redémarrage du
pod effacera toute les données sur ces conteneurs, et par conséquent, cette forme de stockage est, excepté dans le cas
d'applications triviales, relativement limitante. Un volume Kubernetes fournit un stockage permanent qui existe pendant la durée
d'existence du pod lui-même. Ce stockage peut également être utilisé comme disque partagé pour les conteneurs du pod. Ces
volumes sont montés à des points de montages spécifique à l'intérieur du conteneur définit par la configuration du pod, et ne
peuvent être montés aux autres volumes ou liés à ceux-ci. Le même volume peut être monté à différent endroits dans l'arbre du
système de fichiers par différents conteneurs.

Kubernetes fournit un partitionnement des ressources qu'il gère dans des ensembles disjoints appelés espace de noms. L'usage de
ces espaces de noms est destiné aux environnements possédant un grand nombre d'utilisateurs répartis dans plusieurs équipes, ou
projets, ou même à séparer des environnements tels que le développement, l'intégration et la production.

Un problème applicatif thématique est de décider ou stocker et gérer les informations de configuration, dont certaines peuvent
contenir des données sensibles. Les données de configuration peuvent être relativement hétérogènes comme de petits fichiers de
configurations individuels ou de grands fichiers de configuration ou des documents JSON/XML. Kubernetes permet de traiter ce
genre de problème à l'aide de deux mécanismes assez proches : "configmaps" et "secrets", les deux permettent des changements de
configuration sans nécessiter la reconstruction de l'application. Les données de configmaps et de secrets sont accessibles à
chaque objets de l'instance de l'application auxquels ils ont été liés au déploiement. Un secret et/ou un configmaps sont
uniquement envoyé à un noeud si un pod sur ce noeud le demande. Kubernetes le gardera en mémoire sur ce noeud. Un fois que le
pod qui dépend du secret ou de la configmap est supprimé, la copie en mémoire de tous les secrets et configmaps liés est
également supprimée. La donnée est accessible au pod de deux façons : en tant que variables d'environnements (que Kubernetes
créé lors du démarrage du pod) ou disponible dans le système de fichier du conteneur visible dans le pod.

La donnée elle même est stockée sur le maître qui est hautement sécurisé et dont personne ne doit avoir d'accès de connexion. La
plus grande différence entre un secret et une configmap est que le contenu de la donnée dans un secret est encodé en base 64. Il
peut également être chiffré. Les secrets sont souvent utilisés pour stocker des certificats, des mots de passe et des clefs ssh.

Il est très facile de réaliser une mise à l'échelle d'applications sans conditions d'état : Il suffit d'ajouter plus de pods
d'exécution - quelque chose que Kubernetes fait très bien. Les charges de travail avec condition d'état sont bien plus
difficiles, de fait que l'état doit être conservé si un pod est redémarré, et si l'application doit être redimensionnée alors
l'état devra possiblement être redistribué. Les bases de données sont des exemples de charge de travail avec condition d'état.
Lors d'une exécution en mode haute disponibilité, beaucoup de bases de données ont la notion d'instance primaire et
d'instance(s) secondaires. Dans ce cas la notion d'ordonnancement d'instances est important.

#### Libvirt

Libvirt est une API open-source, ainsi qu'un daemon et un outil de gestion pour gérer des plateformes de virtualisation. Elle
peut être utilisée pour gérer KVM, Xen, VMware, ESXi, QEMU et autres technologies de virtualisation. Ces APIs sont utilisées
très largement au niveau de la couche d'orchestration des hyperviseurs pour le développement de solutions de clouds.

#### Hyperviseurs

Un hyperviseur est un logiciel, microgiciel ou matériel qui créé et exécute des machines virtuelles. Un ordinateur sur lequel un
hyperviseur exécute une ou plusieurs machines virtuelles est appelé hôte, et chaque machine virtuelle est appelée invitée.
L'hyperviseur présente le système d'exploitation invité à l'aide d'une plateforme d'exploitation virtuelle et gère l'exécution
des systèmes d'exploitations invités. Plusieurs instances de divers systèmes d'exploitation peuvent partager les même ressources
matérielles virtualisées. A l'instar des mécanismes de virtualisation implémentés au niveau du système d'exploitation
(conteneurs), les systèmes d'exploitations invités ne doivent pas forcément partager un même noyau.

Il existe deux types d'hyperviseurs :

* **Type-1 natif** : Ces hyperviseurs s'exécutent directement sur le matériel de l'hôte pour contrôler le matériel et gérer les
systèmes d'exploitation invités.
* **Type-2 hébergés** : Ces hyperviseurs s'exécutent sur un système d'exploitation conventionnel comme n'importe quel autre
programme. Un système d'exploitation invité s'exécute en tant que processus de l'hôte. Les hyperviseur de type-2 créent une
abstraction pour les systèmes d'exploitation invités depuis le système d'exploitation hôte.

Cette distinction entre les deux types n'est pas toujours claire. Par exemple KVM et bhyve sont des modules noyau qui transforme
le système d'exploitation hôte en hyperviseur de type-1. En même temps, les distributions Linux et FreeBSD sont aussi des
systèmes d'exploitations conventionnels, où des applications entrent en concurrence les unes avec les autres pour les ressources
VM et ils peuvent être aussi catégorisé comme des hyperviseurs de type-2.

## Données

### Bases de données

Une base de données est une collection organisée de données stockées et accédées de manière électronique depuis un système
informatique. Elles sont généralement développées en utilisant des techniques d'architecture et de modélisation formelles.

Les **système de gestion de bases de données (SGBD)** est le logiciel qui interagit avec l'utilisateur final, les applications,
et la base de données elle-même pour capturer et analyser la donnée. Le logiciel SGBD comprend également les principaux outils
d'administration de bases de données.

On peut classifier les système de gestion de bases de données d'après les modèles de bases de données supportés.

Généralement les modèles logiques de données les plus rencontrés sont :

* **Modèle hiérarchiques** : Modèle dans lequel la donnée est organisée dans une structure de type arbre. La donnée est stockée
en tant qu'enregistrements connectés les uns aux autres par des liens. Un enregistrement est une collection de champs, avec
chaque champs contenant une unique valeur. Le type d'enregistrement définit les champs que l'enregistrement contient.
* **Modèle réseau** : Modèle conçu comme étant une façon flexible de représenter des objets et leurs relations. Sa
caractéristique principale est que le schéma, vu comme un graphe dans lequel les types d'objets sont des noeuds et les relations
entre types sont des liens, n'est pas restreint à être hiérarchique ou maillé.
* **Modèle objet** : Modèle dans lequel l'information est représentée sous la forme d'objets et utilisés en programmation
orientée objet. Les bases de données objets sont différentes des bases de données relationnelles qui sont orientées tables.
Les bases de données objet-relationnel sont un hybride des deux approches.
* **Modèle relationnel** : Modèle dans lequel toute donnée est représentée en terme de liste ordonnée finie, groupée dans des
relations.

#### CRUD

**Create Read Update Delete (CRUD)** sont quatre opérations basiques de stockage persistant. CRUD est également utilisé quelques
fois pour décrire des conventions d'interface utilisateur permettant l'affichage, la recherche et le changement d'informations à
l'aide de formulaires et de rapports.

L'acronyme CRUD fait référence à des opérations majeure implémentées par les bases de données. Chaque lettre peut être mis en
lien avec une commande SQL standard :

|CRUD    |SQL   |
|--------|------|
|Create  |INSERT|
|Read    |SELECT|
|Update  |UPDATE|
|Delete  |DELETE|

L'acronyme CRUD peut également apparaître avec les APIs dites RESTful. Chaque lettre de l'acronyme peut être lié à une méthode
HTTP :

|CRUD    |HTTP  |
|--------|------|
|Create  |PUT   |
|Read    |GET   |
|Update  |PUT   |
|Delete  |DELETE|

#### ACID

**Atomicité Consistence Isolation Durabilité (ACID)** est un ensemble de propriétés des transactions de bases de données devant
permettre de garantir une validité de données malgré les erreurs, pertes d'électricité et autres mésaventures. Dans le contexte
des bases de données, une séquence d'opération répondant eux propriétés ACID est appelé transaction.

Les caractéristiques de ces quatre propriétés sont les suivantes :

* **Atomicité** : Les transactions sont souvent composées de déclarations multiples. L'atomicité garantit que chaque transaction
est traitée en tant qu'unité, qui soit réussit totalement, soit échoue totalement : si la moindre déclaration constituante
échoue, la transaction échoue, la transaction dans son ensemble échoue et la base de donnée reste inchangée. Un système atomique
doit garantir l'atomicité dans chaque situation. Par conséquent, une transaction ne peut pas être observée comme étant en
progrès par un autre client de la base de données.
* **Consistence (validité)** : La consistence s'assure qu'une transaction peut uniquement amener la base de données d'un état
correct vers un autre, en gardant les invariants de la base de données : toute donnée rentrée en base doit être valide selon
toutes les règles définies, cela inclut les contraintes, les cascades, les déclenchements, et leurs combinaisons. Cela empêche
une corruption de la base de données mais ne garantit pas qu'une transaction soit correcte. L'intégrité référentielle garantit
la relation clef primaire - clef étrangère.
* **Isolation** : Les transactions sont souvent exécutées de façon concurrente. L'isolation garantit que l'exécution concurrente
de transactions laisse la base de données dans le même état que si ces transactions avaient été exécutées de façon séquentielle.
* **Durabilité** : La durabilité garantit qu'une fois l'exécution de la transaction finie, elle sera toujours enregistrée même
dans le cas d'une panne. Cela signifie généralement que les transactions complétées sont enregistrées dans une mémoire non
volatile.

### Algèbre relationnelle

Langage SQL

### Administration SGBDR PostgreSQL

PostgreSQL est un système de gestion de base de données relationnel (SGBDR) libre et open source mettant l'accent sur
la notion d'extensibilité ainsi que le respect du standard SQL.

PostgreSQL a pour fonctionnalités des transactions avec les propiétés *ACID*, des vues misent à jours automatiquement, des vues
matérialisées, des déclencheurs, des clefs étrangères, et des procédures stockées. PostgreSQL est construit de façon à gérer
tout type de charge de travail, depuis un simple ordinateur jusqu'aux entrepôts de données ou des services webs répondant à de
nombreux utilisateurs concurrents.

#### Stockage et réplication

PostgreSQL inclus une réplication binaire basé sur l'import de changements (write-ahead logs (WAL)) pour répliquer des noeuds de
manière asynchrone, avec la possibilité d'exécuter des requêtes de lecture seule sur ces noeuds répliqués. Ceci permet de
séparer le traffic en lecture sur de multiples noeuds efficacement.

PostgreSQL inclus un mécanisme de réplication synchrone qui garantit que, pour chaque transaction d'écriture, le maître attende
jusqu'à ce qu'au moins un noeud répliqué ait écrit la donné dans sa log de transactions. Contrairement à d'autres systèmes de
base de données, la durabilité d'une transaction (selon qu'elle soit synchrone ou asynchrone) peut être spécifiée par base de
données, par utilisateur, par session ou même par transaction. Cela peut se révéler utile pour des charges qui ne demandent pas
de telles garanties, et peut ne pas être recherchée pour l'ensemble des données car cela diminue les performances du fait de la
nécessité de confirmation de la transaction pour atteindre la veille synchrone.

Les serveurs de veille peuvent être synchrones ou asynchrones. Des serveurs de veille synchrones peuvent être spécifiés dans la
configuration qui détermine quels serveurs sont candidats pour la réplication synchrone. Le premier dans la liste qui diffuse
activement sera utilisé comme serveur synchrone courant. Quand il échoue, le système retombe sur le suivant dans la liste.

La réplication multi-maître synchrone n'est par inclus dans PostgreSQL. Postgres-XC basé sur PostgreSQL fournit la réplication
multi-maître synchrone. Réplication bidirectionnelle (BDR) est un système de réplication multi-maître asynchrone pour
PostgreSQL.

Des outils tels que *remgr* permettent de gérer des groupes de réplication plus facilement.

PostgreSQL inclus un support pour arbre B et index de tables de hachages, ainsi que quatre méthodes d'accès aux index : les
arbres de recherche généralisés (GiST), les index inversés généralisés (GIN), GiST partitionnés espace (SP-GiST) et les index de
portée de bloc (BRIN). De plus, des méthodes d'index définis par l'utilisateur peuvent être crées. Les index dans PostgreSQL
supporte également les fonctionnalités suivantes :

* Les index d'expression peuvent être créés à l'aide d'un index du résultat d'une expression ou fonction, au lieu de la simple
valeur d'une colonne.
* Les index partiels, qui indexent seulement une partie d'une table, qui peut être créé en ajoutant une clause WHERE à la fin de
la déclaration CREATE INDEX. Ceci permet la création d'index plus petits.
* Le planificateur est capable d'utiliser des index multiples ensemble pour répondre à des requêtes complexes, en utilisant des
opérations temporaire d'index de tableaux de bits en mémoire.
* L'indexation des k-proche voisins (KNN-GiST) fournit une recherche efficace des "plus proches valeurs" à celle spécifiée,
utile pour trouver des mots similaires, ou des objets proches ou des lieux avec des informations géographiques.
* Les balayages uniquement-index permettent souvent au système de récupérer la donnée depuis des index sans jamais avoir à
accéder la table principale.

Dans PostgreSQL, un schéma détient tous les objets, à l'exception des rôles et des tablespaces. Les schémas agissent
efficacement comme des espaces de noms, permettant à des objets de même noms de coexister dans la même base de données. Par
défaut, les bases de données ont un schéma appelé *public*, mais d'autres schémas peuvent être créés, et le schéma public n'est
pas obligatoire.


Un paramètre *search_path* détermine l'ordre dans lequel PostgreSQL vérifie les schémas les objets sans qualifications (sans
schéma préfixé). Par défaut, il est défini à *$user, public* (*$user* fait référence à l'utilisateur de la base de données
connecté). Ce paramètre peut être changé sur une base de données ou un niveau de rôle, mais il s'agit d'un paramètre de session,
il peut être changé librement (de multiples fois) pendant une session client, affectant uniquement cette session.

Les schémas non existants, listés dans *search_path* sont silencieusement ignorés pendant la recherche d'objets.

De nouveaux objets sont créés dans n'importe quel schéma valide qui apparaît en premier dans le *search_path*

Une grande variété de types de données natifs sont supportés, incluant :

* Booléens
* Nombre à précision arbitraire
* Caractère (texte, varchar, char)
* Binaire
* Date/heure (horodatage/heure avec/sans fuseau horaire, date, intervalle)
* Monnaie
* Énumération
* Chaîne de bits
* Texte de recherche de type
* Composé
* HStore
* Tableaux
* Primitives géométriques
* Adresses IPv4 et IPv6
* CIDR et adresses MAC
* XML
* UUID
* JSON, JSONB

De plus, les utilisateurs peuvent créer leurs propres types de données qui peut généralement être fait complètement indexable
via les infrastructures d'indexation PostgreSQL - GiST, GIN, SP-GiST.

Il existe également un type de données appelé un *domaine*, qui est le même que n'importe quel type de données mais avec des
contraintes optionnelles définies par le créateur de ce domaine. Cela signifie que toute donnée entrée dans une colonne
utilisant le domaine devra se conformer aux contraintes définies comme faisant partie du domaine.

Un type qui représente un intervalle de donnée peut être utilisé et est appelé type intervalle. Ces intervalles peuvent être
discrets ou continus. Les types inclus disponibles sont les intervalles d'entiers, les nombres décimaux, les horodatages et les
dates.

Des types avec intervalles personnalisés peuvent être créés pour faire de nouveau types d'intervalles disponibles, tels que des
adresses IP qui utilisent un type inet comme base. Les types intervalle sont aussi compatibles avec des opérateurs existants
utilisés pour vérifier des intersections, l'inclusion, etc.

De nouveaux types de la plupart des objets à l'intérieur de la base de données peuvent être créés, tels que :

* Distributions
* Conversions
* Types de données
* Domaines de données
* Fonctions
* Index
* Opérateurs
* Langages procéduraux

Des tables peuvent être paramétrées pour hériter leurs caractéristiques d'une table parente. Les données d'une table descendante
apparaîtront comme existantes dans les tables parentes, à moins que les données ne soit lues depuis la table parente en
utilisant le mot clef ONLY. Ajouter une colonne dans la table parente la fera apparaître dans la table descendante.

L'héritage peut être utilisé pour implémenter un partitionnement de table, en utilisant soit des déclencheurs soit des règles
pour insérer directement depuis la table parente dans les tables descendante concernées.

L'héritage permet de fournir un moyen de relier les fonctions de généralisation hiérarchiques décrites dans des diagrammes
relations entités (ERDs) directement dans la base de données PostgreSQL.

#### Contrôle et connectivité

PostgreSQL peut se connecter à d'autres systèmes pour récupérer des données via des encapsulateurs de données étrangères (FDWs).
Ils peuvent prendre la forme de n'importe quelle source de données, telle qu'un système de fichier, un autre SGBDR, ou un
service web. Cela signifie que des requêtes classiques de base de données peuvent utiliser ces sources de données comme des
tables, et même joindre ensemble plusieurs sources de données.

Pour se connecter à des applications, PostgreSQL inclus l'interface libpq (l'interface d'applications C officielle) et ECPG (un
système C embarqué). Des bibliothèques tierces pour se connecter à PostgreSQL sont disponibles pour de nombreux langages de
programmation.

Des langages procéduraux permettent aux développeurs d'étendre la base de donnée à l'aide de fonctions personnalisées, souvent
appelées **procédures stockées**. Ces fonctions peuvent être utilisées pour construire des déclencheurs de base de données
(fonctions invoquées suite à la modification d'une donnée particulière), des types de données personnalisés et des fonctions
d'agrégations. Des langages procéduraux peuvent également être invoqués sans définir de fonction en utilisant une commande DO au
niveau du SQL.

Les langages sont divisés en deux groupes : les procédures écrites en langage sûr sont exécutées dans des bacs à sable et
peuvent être créées et utilisées par n'importe quel utilisateur. Les procédure écrites en langage non sûr sont uniquement créées
par des super utilisateurs, du fait qu'elles permettent un court circuit des restrictions de sécurité de la base de données,
mais également d'accéder à des ressources externe à la base de données. Quelques langages tel que Perl fournissent à la fois des
versions sûre et non sûres.

PostgreSQL fournit un support pour trois langages procéduraux :

* SQL (sûr). Des fonctions SQL plus simple peuvent être étendues dans une requête appelante, qui évite la surcharge à l'appel
de fonction et permet à l'optimiseur de requête de "voir à l'intérieur" de la fonction.
* Langage procédural PostgreSQL (PL/pgSQL)(sûr), qui ressemble au langage procédural Oracle pour langage procédural SQL (PL/SQL)
et les modules stockés persistants SQL (SQL/PSM).
* C (non sûr), qui permet de charger une ou plusieurs bibliothèques partagées personnalisées en base de données. Les fonctions
écrites en C offrent une meilleure performance, mais un bogue dans le code peut faire tomber et corrompre la base de données.
La plupart des fonctions incluses sont écrites en C.

De plus, PostgreSQL permet aux langages procéduraux d'être chargés en base de données à l'aide d'extensions. Trois extensions
sont incluses dans PostgreSQL pour Perl, Tcl et Python.

Des déclencheurs sont des évènements déclenchés par l'action de déclarations de langage de manipulation de données SQL (DML). La
plupart des déclencheurs sont activés uniquement pour des déclarations INSERT ou UPDATE.

Les déclencheurs peuvent être liés à des tables. Ils peuvent être par colonnes et conditionnels, les déclencheurs UPDATE peuvent
cibler les colonnes spécifiques d'une table, et les déclencheurs peuvent être exécutés selon un ensemble de conditions spécifiés
par une clause WHERE. Les déclencheurs peuvent être liés à des vues en utilisant la condition INSTEAD OF. De multiples
déclencheurs sont traités par ordre alphabétique. En plus d'appeler des fonctions écrites en PL/pgSQL natif, les déclencheurs
peuvent également invoquer des fonctions écrites dans d'autres langages PL/Python ou PL/Perl.

PostgreSQL fournit un système de messagerie asynchrone qui peut être accédé à travers les commandes NOTIFY, LISTEN et UNLISTEN.
Une session peut émettre une commande NOTIFY, avec un canal spécifique à un utilisateur et une charge optionnelle, pour marquer
l'occurrence d'un évènement en particulier. D'autres sessions sont capables de détecter ces évènements en soumettant une
commande LISTEN, qui permet d'écouter un canal en particulier. Cette fonctionnalité peut être utilisée pour une grande variété
de situations, tels que faire savoir à d'autres sessions quand une table a été mise à jour, ou pour des applications séparées de
détecter quand une action particulière a été effectuée. Un tel système prévient le besoin de récupération d'informations par les
applications et réduit les surcharges inutiles. Les notifications sont pleinement transactionnelles, dans le sens où les
messages ne sont pas envoyés tant que la transaction desquels ils proviennent n'a été enregistrée. Cela élimine le problèmes des
messages envoyés pour une action en cours qui est ensuite redéroulée en sens inverse.

Beaucoup de connecteurs pour PostgreSQL fournissent un support pour le système de notification (incluant libpq, JDBC, Npgsql,
psycopg, Npgsql, psycopg et node.js) pour qu'il puisse être utilisé par des applications internes.

Les règles permettent l'arbre de requêtage d'une requête arrivante d'être réécrit. Des "règles de réécriture de requête" sont
liées à une table/classe et "réécrire" le DML arrivant (select, insert, update et/ou delete) en une ou plusieurs requêtes
supplémentaires qui soit remplacent la déclaration DML originale soit s'ajoutent à lui. Les réécriture de requêtes ont lieu
après l'analyse syntaxique de la déclaration de requête, mais avant la planification de requête.

D'autres fonctionnalités du requêtage incluent :

* Transactions
* Recherche plein texte
* Vues
    + Vues matérialisées
    + Vues actualisables
    + Vues récursives
* Joins internes, externes et croisés
* Sous-selects
    + Sous-requêtes corrélées
* Expressions régulières
* Expressions de tables communes et expression de table commune inscriptible
* Connexions chiffrées via sécurité de couche transport (TLS)
* Domaines
* Points de sauvegardes
* Enregistrement en deux temps
* La technique de stockage d'attributs surdimensionnés (TOAST) est utilisée pour stocker de manière transparente de grand
attributs de table dans une aire séparée, avec compression automatique.
* SQL embarqué est implémenté en utilisant un préprocesseur. Un code SQL est tout d'abord été écrite et embarquée dans du code
C. Puis le code est exécuté à travers un préprocesseur ECPG, qui remplace le SQL avec des appels au code de la bibliothèque.
Puis le code peut être compilé en utilisant un compilateur C.

Le serveur PostgreSQL fonctionne au niveau des processus (et pas des fils d'exécution), et utilise un processus de système
d'exploitation par session de base de données. Des sessions multiples sont automatiquement réparties sur tout les processeurs
disponibles par le système d'exploitation. De nombreux types de requêtes peuvent aussi être parallélisés à travers plusieurs
processus en arrière plan, pour profiter des nombreux processeurs ou cœurs. Les applications clientes peuvent utiliser des fils
d'exécution et créer plusieurs connexions à la base de données depuis chaque fil d'exécution.

#### Sécurité

PostgreSQL gère sa sa sécurité interne à l'aide de rôles. Un rôle peut généralement être un utilisateur (un rôle pouvant se
connecter), ou un groupe (un rôle duquel d'autres rôles sont membres). Les permissions peuvent être accordées ou révoquées sur
tout objet jusqu'au niveau des colonnes, et peuvent également permettre/interdire la création de nouveaux objets au niveau de la
base de données, du schéma ou de la table.

La fonctionnalité PostgreSQL SECURITY LABEL permet un sécurité additionnelle; avec un module chargeable inclus permettant un
contrôle d'accès obligatoire d'étiquettes (MAC) basé sur la politique de sécurité SELinux.

PostgreSQL supporte nativement un large panel de mécanismes externes d'authentification, tels que :

* Mot de passe : soit SCRAM-SHA-256, soit MD5, soit en clair
* GSSAPI
* SSPI
* Kerberos
* ident (relie utilisateur système et utilisateur base de données par serveur ident)
* peer (relie utilisateur local et utilisateur base de données)
* LDAP
    + AD
* RADIUS
* Certificat
* PAM

Les méthodes par GSSAPI, SSPI, Kerberos, pees, ident et certificats peuvent également spécifier un fichier "map" qui liste quels
utilisateurs vérifiés par le système d'authentification sont autorisés à se connecter spécifiquement en tant qu'utilisateur de
base de données.

Ces méthodes sont spécifiées dans le fichier de configuration d'authentification de groupe basé sur l'hôte (pg_hba.conf), qui
détermine quelles connexions sont autorisées. Cela permet un contrôle sur quel utilisateur peut se connecter à quelle base de
données, depuis quelle adresse IP, intervalle d'adresse IP ou domaine socket, avec quel système d'authentification, et si la
connexion doit utiliser la couche de transport de sécurité (TLS)

### Big data et lac de données

Le big data désigne les ressources d'informations dont les caractéristiques en terme de volume, de vélocité et de variété
imposent l'utilisation de technologies et de méthodes analytiques particulières pour générer de la valeur, et qui dépassent en
général les capacités d'une seule et unique machine et nécessitent des traitements parallélisés.

L'explosion quantitative (et souvent redondante) des données numériques permet une nouvelle approche pour analyser le monde. Le
volume colossal de données numériques disponibles, implique de mettre en oeuvre de nouveaux ordres de grandeur concernant la
capture, le stockage, la recherche, le partage, l'analyse et la visualisation des données, celles-ci proviennent de nombreuses
sources numériques : les réseaux sociaux, l'OpenData, le Web, des bases de données privées, publiques à caractère commercial ou
scientifique.

La maturité grandissante du concept permet de se différencier de manière plus marqué vis à vis de l'informatique décisionnelle :

* L'informatique décisionnelle utilise des outils de mathématiques appliquées ainsi que des statistiques descriptives à l'aide
de données à haute densité d'informations pour mesurer, observer des tendances etc.  Le big data utilise l'analyse mathématique,
l'optimisation, les statistiques inductives ainsi que des concepts d'identification de systèmes non linéaires pour inférer des
règles (régressions, relations non linéaires et effets causaux) de larges ensembles de données avec une densité d'information
faible afin de révéler des relations et dépendances, ou pour fournir des prédictions de résultats et de comportements.

Le big data peut être décrit par les caractéristiques suivantes :

* **Volume** : La quantité de la donnée générée et stockée. La taille de la donnée détermine la valeur, la potentielle
perspicacité, et si elle peut être considérée big data ou non. La taille du big data est généralement supérieure aux térabits et
pétabits.
* **Variété** : Le type et la nature de la donnée. Les technologies plus anciennes tels que les SGBDR étaient capable de traiter
des données structurées de manière performante et efficace. Néanmoins, les changements du type et de la nature des données
structurées à semi-structurées puis déstructurées à mis à l'épreuve les outils et technologies existants. Les technologies big
data ont évoluées avec l'intention première de capturer, stocker et traiter la donnée semi-structurée et déstructurée (variété)
générée très rapidement  (vitesse), et avec une taille immense (volume). Plus récemment, ces outils et technologies sont
utilisés pour s'occuper de données structurées mais préférablement pour le stockage. Finalement, le traitement de données
structurées était toujours gardé en option, soit en utilisant du big data soit avec des SGBDR traditionnels. Ceci permet
d'analyser les données vers un usage efficace des perspicacités cachées exposées par les données collectées via les réseaux
sociaux, les fichiers de logs, les capteurs, etc. que le big data dessine depuis du texte, des images, audio, vidéo ; et
complète les pièces manquantes via la fusion de données.
* **Vitesse** : La rapidité à laquelle la donnée est générée et traitée pour satisfaire la demande et les nouveaux défis que
représentent le chemin de la croissance et du développement. Le big data est souvent disponible en temps réel. En comparaison
avec le small data, le big data est produit de façon plus continu. Deux types de vitesses liés au big data sont la fréquence de
production et la fréquence de gestion, enregistrement et publication.
* **Véracité** : La véracité et la fiabilité de la donnée, qui fait référence à sa qualité et sa valeur. Le big data ne doit pas
être uniquement grand par la taille, mais également fiable afin d'obtenir de la valeur dans son analyse. La qualité de la donnée
capturée peut largement varier et affecter la précision de l'analyse.
* **Valeur** : La valeur d'une information qui peut être obtenu par le traitement et l'analyse de grands ensembles de données.
La valeur peut aussi être mesurée par une évaluation des autres qualités du big data. La valeur peut aussi représenter la
profitabilité de l'information qui est extraite de l'analyse du big data.
* **Variabilité** : La caractéristiques des formats changeants, structure, ou sources de big data. Le big data peut inclure des
données structurées ou déstructurées ou encore une combinaison des deux. L'analyse du big data peut intégrer des données brutes
de sources multiples. Le traitement des données brutes peut également faire intervenir des transformations de données
déstructurées en données structurées.

D'autres caractéristiques du big data sont :

* **Exhaustivité** : Si le système dans son ensemble est capturé (enregistré) ou non. Le big data peut inclure toutes les
données depuis les sources ou pas.
* **Fin et uniquement lexical** : Respectivement, la proportion de données spécifiques de chaque élément par élément collecté et
si l'élément et ses caractéristiques sont correctement indexés ou identifiés.
* **Relationnel** : Si la donnée collectée contient des champs communs qui pourrait permettre une intersection, ou méta-analyse,
de différents ensembles de données.
* **Extensible** : Si de nouveaux champs dans chaque élément de la donnée collectée peuvent être ajoutés ou changés facilement.
* **Mise à l'échelle** : Si la taille du système de stockage big data peut rapidement s'agrandir.

L'architecture du big data est généralement multi-couche. Une architecture parallèle distribuée distribue les données à travers
de multiples serveurs, ces exécutions dans des environnements parallèles permet d'améliorer considérablement les temps de
traitements. Ce type d'architecture rentre les données dans des systèmes de gestions de bases de données parallèles, qui
implémentent l'usage du modèle de programmation *MapReduce* de Google. Le concept *MapReduce* fournit un modèle de traitement
parallèle. Les requêtes sont réparties et distribuées à travers des noeuds parallèles et traités en parallèle. Les résultats
sont ensuite récupérés, rassemblés et fournis. Ce type de framework cherche à rendre transparent le pouvoir de traitement à
l'utilisateur final en utilisant des interfaces de serveurs d'applications.

Le lac de données peut permettre à une organisation de changer son fusil d'épaule en passant d'un contrôle centralisé à un
modèle partagé pour répondre aux changements dynamiques de la gestion d'information.

Un lac de données est un système ou bibliothèque de données stockées dans leurs formats naturels/bruts, généralement des objets
blobs ou des fichiers. Un lac de données est généralement un emplacement de stockage unique de données qui incluent des copies
brutes de données système, des données de capteurs, des données sociales etc. et des données transformées utilisées pour des
tâches comme la visualisation, l'analyse et l'apprentissage automatique. Un lac de données peut inclure des données structurées
de bases de données relationnelles, semi-structurées (CSV, logs, XML, JSON), ou déstructurées (emails, documents, PDFs) et des
données binaires (images, audio, vidéo).

Les principaux composants qui caractérisent un écosystème big data sont les suivants :

* Des techniques d'analyse de données, telles que les tests A/B, l'apprentissage automatique, et le traitement automatique des
langages.  Des technologies big data, telles que l'informatique décisionnelle, l'informatique en nuage, et les bases de données.
* Une visualisation de la donnée, à travers des diagrammes, des graphes, ou d'autres types d'affichages.
