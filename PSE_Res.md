# Réseau et Logiciels

#### Introduction

Cet exposé traite des parties réseau et logiciels du programme PSE.

#### Table des matières

* [Réseau](#réseau)
    + [Topologie de réseau](#topologies)
    + [Interfaces](#interfaces)
    + [VLAN](#vlan)
    + [Réseautage](#réseautage)
    + [VPN](#vpn)
    + [NAS et SAN](#nas-et-san)
    + [Modèle OSI](#modèle-osi)
    + [Architecture TCP/IP](#architecture-tcpip)
    + [Protocoles](#protocoles)
        - [Couche application](#couche-application)
        - [Couche transport](#couche-transport)
        - [Couche internet](#couche-internet)
        - [Couche liaison](#couche-liaison)
    + [MIB](#mib)
    + [Active Directory et Samba](#active-directory-et-samba)
* [Logiciels](#logiciels)
    + [Architecture logicielle](#architecture-logicielle)
        - [Client serveur](#client-serveur)
        - [Trois niveaux](#trois-niveaux)
        - [N-tiers](#n-tiers)
    + [Haute disponibilité](#haute-disponibilité)
    + [Langages de présentation](#langages-de-présentation)
    + [Construction et automatisation](#construction-et-automatisation)
        - [Maven](#maven)
        - [Jenkins](#jenkins)
        - [Cobbler](#cobbler)
        - [Puppet](#puppet)
        - [Ansible](#ansible)
    + [Métrologie et supervision Nagios](#métrologie-et-supervision-nagios)
    + [Sauvegarde](#sauvegarde)

## Réseau

### Topologie de réseau

Une topologie de réseau est un arrangement d'éléments (liens, noeuds, etc.) dans un réseau de communication.

La topologie de réseau est une structure topologique d'un réseau et peut-être décrite physiquement ou logiquement. Il s'agit
d'une application de la théorie des graphes à l'intérieur de laquelle les appareils de communication sont représentés comme des
noeuds et les connexions comme des liens ou lignes entre les noeuds. La **topologie physique** est le placement de divers
composants d'un réseau, tandis que la **topologie logique** illustre les flux de données à l'intérieur d'un réseau. La distance
entre noeuds, les interconnexions physiques, les taux de transmission, ou les types de signaux peuvent varier entre deux réseaux
différents, néanmoins leurs topologies peuvent être identiques. La topologie physique d'un réseau est un problème particulier à
la couche physique du modèle OSI.

Le médium de transmission utilisé pour relier des appareils afin de créer un réseau informatique inclut des cables électriques,
fibres optiques et ondes radios sont appelés **liens**. Dans le modèle OSI, ceux-ci sont définis au niveau de la couche physique
et de la couche liaison.

#### Noeuds

<TODO>

#### Classification

<TODO>

#### Centralisation

<TODO>

#### Decentralisation

<TODO>

### Interfaces

<TODO>

### VLAN

<TODO>

### Réseautage

<TODO>

### VPN

<TODO>

### NAS et SAN

<TODO>

### Modèle OSI

Le modèle OSI (*Open Systems Interconnection*) est un modèle conceptuel qui caractérise et standardise les fonctions de
communication d'un système de télécommunication ou informatique de manière détachée de la structure interne sous-jacente et de
la technologie. Le but est de garantir l'interopérabilité des divers systèmes de communication à l'aide de protocoles de
communication standards.

Le modèle sépare le flux des données dans un système de communication en sept couches abstraites, de l'implémentation physique
de la transmission de bits à travers un médium de communication jusqu'à la plus haute représentation des données d'une
application distribuée. Chaque couche intermédiaire fournit un classe de fonctions à la couche supérieure étant elle même servie
par la couche en dessous d'elle. Ces classes de fonctionnalités sont réalisées dans le logiciel par des protocoles de
communication standardisés.

<table>
    <thead>
        <tr>
            <th colspan="3">Couche</th>
            <th>Unité de protocole de données (PDU)</th>
            <th>protocoles TCP/IP</th>
            <th>Fonctions</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <th rowspan="4">Couches Hôtes</th>
            <td>7</td>
            <td>Application</td>
            <td rowspan="3">Donnée</td>
            <td>NFS</td>
            <td>APIs de haut-niveau, partages de ressources, accès de fichiers distants</td>
        </tr>
        <tr>
            <td>6</td>
            <td>Présentation</td>
            <td>MIME-SSL-TLS-XDR</td>
            <td>Traduction de données entre services réseau et une application ; encodage, compression et chiffrement</td>
        </tr>
        <tr>
            <td>5</td>
            <td>Session</td>
            <td>Sockets(établissement de session TCP/RTP/PPTP)</td>
            <td>Gestion de sessions de communications, i.e., échange continu d'information sous la forme de mulptiples
            va-et-vient de transmissions entre deux noeuds</td>
        </tr>
        <tr>
            <td>4</td>
            <td>Transport</td>
            <td>Segment, Datagramme</td>
            <td>TCP-UDP-SCTP-DCCP</td>
            <td>Transmission fiables de segments de données entre points d'un réseau, incluant la segmentation, l'acquittement
            et le multiplexage</td>
        </tr>
        <tr>
            <th rowspan="3">Couches médias</th>
            <td>3</td>
            <td>Réseau</td>
            <td>Paquet</td>
            <td>IP-IPsec-ICMP-IGMP-OSPF-RIP</td>
            <td>Structurant et gérant un réseau multi-noeuds, incluant l'adressage, le routage et le contrôle du traffic</td>
        </tr>
        <tr>
            <td>2</td>
            <td>Liaison</td>
            <td>Trame</td>
            <td>PPP-SLIP</td>
            <td>Transmissions fiables de trames de données entre deux noeuds connectés par une couche physique</td>
        </tr>
        <tr>
            <td>1</td>
            <td>Physique</td>
            <td>Bit</td>
            <td>Bluetooth-CAN bus-Ethernet Physical Layer-SMB-USB Physical Layer</td>
            <td>Transmissions et Réceptions de flux de bits à travers un médium physique</td>
        </tr>
    </tbody>
</table>

Les protocoles de communication permettent à une entité sur un hôte d'interagir avec une entité correspondante sur la même
couche dans un hôte différent. La définitions des services, comme le modèle OSI, décrit de manière abstraite la fonction de la
couche (N-1) pour la couche (N), ou N est une des sept couches de protocoles opérante sur l'hôte local.

A chaque niveau N, deux entités d'appareils communicants (couches N pairs) échangent des unités de protocole de données (PDUs)
par le moyen de la couche protocole N. Chaque PDU contient une charge, appelée unité de service de données (SDU), ainsi que les
entêtes et pieds reliés au protocole.

Le processus de données entre deux appareils compatibles OSI communicants est le suivant :

1. La donnée à transmettre est composée au niveau de la couche la plus haute de l'appareil transmetteur (couche *N*) dans une
unité de protocole de données.
2. Le *PDU* est passé à la couche *N-1*, ou il est reconnu comme une unité de service de données (*SDU*).
3. Au niveau de la couche *N-1* le *SDU* est concaténé avec une entête, un pied, ou les deux, produisant un *PDU de couche N-1*.
Il est alors envoyé à la couche *N-2*.
4. Le processus continue jusqu'à ce que la couche la plus basse soit atteinte, depuis laquelle la donnée est transmise à
l'appareil récepteur.
5. Au niveau de l'appareil récepteur, la donnée est passée de la couche la plus basse à la couche la plus haute comme une suite
de *SDUs* tandis quelle est pelée successivement de chaque entête ou pied de couche jusqu'à atteindre la couche la plus haute,
où la donnée restante est consommée.

### Architecture TCP/IP

La suite des protocoles internet est un modèle conceptuel et un ensemble de protocoles de communication utilisés par internet et
les réseaux informatiques similaires. Elle est connue plus communément sous le nom d'architecture **TCP/IP** du fait que les
protocoles sur lesquels elles s'appuie sont, le protocole de contrôle de transmission (TCP) et le protocole internet (IP). Son
implémentation est une pile de protocoles.

La suite des protocoles internet fournit une communication de données bout en bout en spécifiant comment la donnée doit être
empaquetée, adressée, transmise, routée et reçue. Cette fonctionnalité est organisée en quatre couches d'abstraction, qui
classifient tous les protocoles rattachés en fonction de l'étendue de leur implication réseau. De la couche la plus basse à la
couche la plus haute :

* Liaison : contient des méthodes de communications pour des données appartenant à un unique segment réseau (ou lien).
* Internet : fournit l'interconnexion entre réseaux indépendants.
* Transport : gère la communication d'hôte à hôte.
* Application : fournit l'échange de données inter-processus pour les applications.

Les trois couches les plus hautes du modèle OSI, i.e. la couche application, présentation et session, ne sont pas distinguées
séparément dans l'architecture TCP/IP. Néanmoins il n'y a aucune contrainte sur le fait que la pile de protocoles TCP/IP impose
une architecture monolithique au dessus de la couche transport. Par exemple le protocole applicatif NFS fonctionne au dessus du
protocole de présentation de représentation externe de données (XDR), qui lui-même s'appuie sur le protocole d'appel de
procedures distant (RPC). RPC fournit une transmission fiable des enregistrements, de façon qu'il puisse utiliser le protocole
de transport UDP de manière sûre.

La fonctionnalité de la couche session peut se retrouver dans des protocoles tels que HTTP et SMTP et de manière encore plus
évidente dans Telnet et le protocole d'initialisation de session (SIP). La fonctionnalité de la couche session est également
réalisée par la numérotation de port qui appartient à la couche transport de la suite TCP/IP. Les fonctions de la couche
présentation est réalisée dans les applications TCP/IP à l'aide du standard MIME dans l'échange de données.

### Protocoles

#### Couche application

* **DHCP** (*Dynamic Host Configuration Protocol*) : protocole des gestion réseau utilisé sur des réseaux IP, où un serveur DHCP
assigne dynamiquement des adresses IP et autres paramètres de configuration réseaux à chaque appareil, de façon à ce qu'ils
puissent communiquer avec d'autres réseaux IP. DHCP utilise le protocole UDP. (Port 67 pour le serveur, 68 pour le client)
* **DNS** (*Domain Name System*) : système de nommage dynamique et hiérarchisé pour appareils, services et autres ressources
connectées à Internet ou un réseau privé. DNS utilise UDP pour les requêtes de moins de 512 octets sinon il utilise TCP. (Port
53)
* **FTP** (*File Transfer Protocol*) : protocole réseau standard utilisé pour le transfert de fichiers depuis un serveur à un
client. FTP est construit sur un modèle d'architecture client-serveur en utilisant des connexions de contrôles et de données
séparées entre le client et le serveur. Les utilisateurs FTP peuvent s'authentifier eux-mêmes à l'aide d'un protocole
d'authentification en clair, généralement sous la forme d'un nom d'utilisateur et d'un mot de passe, mais ils peuvent se
connecter de manière anonyme si le serveur est configuré en ce sens. Pour des transmission sécurisées protégeant le nom
d'utilisateur et le mot de passe, et qui chiffrent le contenu, FTP est souvent sécurisé à l'aide de SSL/TLS (FTPS) ou bien
remplacé par le protocole de transfert de fichier SSH (SFTP). Le client FTP initie des connexions TCP selon différents modes.
(Port 21 pour le serveur)
* **HTTP** (*Hypertext Transfer Protocol*) : protocole de la couche application pour systèmes d'information distribués,
collaboratifs, hypermedia. HTTP est la base de la communication de données pour le World Wide Web, où des documents hypertextes
incluent des hyper liens pour d'autres ressources que l'utilisateur peut accéder facilement, par exemple par un click
utilisateur ou en tapant à l'écran dans un navigateur web. Le client initie une connexion TCP. (Port 80 ou 8080)
* **HTTPS** (*Hypertext Transfer Protocol Secure*) : est une extension de HTTP. Il est utilisé pour une communication sécurisé à
travers un réseau, et très largement répandu sur Internet. En HTTPS, le protocole de communication est crypté avec la sécurité
de la couche transport (TLS) ou, précédemment la couche de sockets sécurisée (SSL). Le protocole est par conséquent désigné
également par HTTP sur TLS, ou HTTP sur SSL. (Port 443)
* **IMAP** (*Internet Message Access Protocol*) : est un protocole Internet standard utilisé par les clients emails pour
récupérer les messages d'un serveur de messagerie à travers une connexion TCP/IP. (Port 143 et 993 pour IMAP sur SSL/TLS)
* **LDAP** (*Lightwight Directory Access Protocol*) : est un protocole applicatif standard permettant d'accéder et de maintenir
des répertoires de services d'information distribués à travers un réseau IP. Les répertoires de services jouent un rôle
important dans le développement des applications intranets et Internet en permettant le partage d'informations à propos
d'utilisateurs, de systèmes, de réseaux, de services, et d'applications à travers le réseau. LDAP utilise TCP et UDP. (Port 389
et 636 pour LDAP sur SSL/TLS)
* **NFS** (*Network File System*) : est un protocole de système de fichiers distribué qui permet à un ordinateur client
d'accéder à des fichiers à travers un réseau informatique. NFS comme de nombreux protocoles est construit au-dessus du protocole
ONC/RPC. NFS 3 et 4 utilisent le protocole TCP. (Port 2049 pour NFSv4)
* **ONC/RPC** (*Open Networking Computing/Remote Procedure Call*) : est un système d'appel procedural distant. Il sérialise la
donnée à l'aide de la représentation de données externes (XDR), qui permet également le transcodage pour l'accès sur de
multiples plateformes. ONC délivre alors la charge XDR à l'aide des protocoles UDP ou TCP. (Port 111)
* **RIP** (*Routing Information Protocol*) : est un protocole de routage IP de type vecteur s'appuyant sur l'algorithme de
détermination des routes décentralisé Bellman-Ford. Il permet à chaque routeur de communiquer aux routeurs voisins. La métrique
utilisée est la distance qui sépare un routeur d'un réseau IP déterminé quant au nombre de sauts. RIP utilise UPD. (Port 520)
* **SIP** (*Session Initiation Protocol*) : est un protocole de signalisation utilisé pour initier, maintenir et terminer des
sessions en temps réel, qui inclut des applications de messageries, vocales et vidéo. Les clients SIP utilisent TCP ou UDP.
(Port 5060 et 5061 pour SIP sur SSL/TLS)
* **SMTP** (*Simple Mail Transfer Protocol*) : est un protocole de communication pour la transmission de mail. Les serveurs
mails et autres agents de transferts utilisent SMPT pour envoyer et recevoir des messages mails. Les serveurs SMTP utilisent le
protocole TCP. (Port 25)
* **SNMP** (*Simple Network Management Protocol*) : SNMP est un protocole Internet standard utiliser pour collecter et organiser
l'information liée aux appareils sur des réseaux IP et pour modifier cette information afin de définir un nouvel état de
fonctionnement. Les appareils qui typiquement supportent SNMP sont les modems, les routeurs, les switch, les serveurs, les
postes de travail, les imprimantes, etc. SNMP est utilisé très largement pour la gestion et la surveillance réseau. SNMP expose
la gestion des données sous la forme de variables sur les systèmes gérés organisées dans une base informationnelle de gestion
(MIB) qui décrit le statut de la configuration système. Ces variables peuvent être elles même requêtées à distance (et, dans
certaines circonstances manipulées) par des applications de gestion.
* **SSH** (*Secure Shell*) : est un protocole réseau cryptographique pour des service réseaux sécurisés opérants sur des réseaux
non-sécurisés. SSH utilise une architecture client-serveur en connectant un client SSH à un serveur. SSH utilise TCP. (Port 22)
* **TLS/SSL** (*Transport Layer Security/Secure Sockets Layer*) : sont des protocoles cryptographiques permettant des
communications sécurisées à travers un réseau. Le protocole TLS a pour but principal de garantir le caractère privé et
l'intégrité de la donnée entre deux applications communicantes ou plus. Une connexion entre un client et un serveur doit quand
elle est sécurisé par TLS avoir une ou plusieurs des propriétés suivantes :
    + La connexion est privée (ou sécurisée) par un algorithme cryptographique symétrique pour chiffrer les données transmises.
    Les clefs de cette encryption symmétrique sont générées de manière unique et à chaque connexion, elle sont créées à partir
    d'un secret partagé négocié au début de la session. Le serveur et le client négocient les détails de l'algorithme
    cryptographique utilisé avant que le premier octet de données soit échangé. La négociation du secret partagé est à la fois
    sécurisé (ne peut être attaqué à l'aide d'un connexion intermédiaire) et fiable (aucun attaquant ne peut modifier les
    communications pendant le processus de négociation sans être détecté).
    + L'identité des parties en communication peut être authentifiée via une clef cryptographique publique. Cette
    authentification est requise pour le serveur et optionnelle pour le client.
    + La connexion est fiable du fait que chaque message transmis inclus un message de vérification d'intégrité en utilisant un
    message de code d'authentification pour prévenir les pertes non-détectées ou l'altération des données durant la transmission.
En plus des propriétés ci-dessus, un configuration TLS peut fournir des propriétés de sécurisation supplémentaires telles que la
confidentialité persistante, assurant qu'aucune découverte future des clefs cryptographiques ne puisse être utilisée pour
déchiffrer une communication TLS enregistrée par le passé.
* **XDR** (*External Data Representation*) : est un standard de format de sérialisation de données qui se retrouve dans de
nombreux protocoles réseau.

#### Couche transport

* **TCP** (*Transmission Control Protocol*) : est un des protocoles principals de la suite des protocoles internet. Il a été
développé à l'origine dans l'implémentation réseau initiale pour complémenter le protocole internet (IP). Par conséquent, la
suite entière est communément connue sous le nom d'architecture TCP/IP. TCP fournit de flux d'octets vérifiés ordonnés et
fiables entre applications s'exécutant sur des hôtes communiquant via un réseau IP. TCP est orienté connexion, et une connexion
entre client et serveur est établie avant qu'une donnée puisse être envoyée. Le serveur doit écouter (ouverture passive) les
requêtes de connexion des clients avant qu'une connexion soit établie. Un handshaking en trois temps (ouverture active), une
retransmission, et une détection d'erreurs permet une grande fiabilité mais ajoute de la latence. Les applications qui ne
requiert pas un service de flux de données fiable peuvent utiliser le protocole datagramme utilisateur (UDP), qui fournit un
service datagramme sans connexion qui priorise le temps à la fiabilité. TCP permet d'éviter la congestion réseau. Néanmoins, le
TCP est vulnérable aux attaques de déni de service, au piratage de connexion, attaque par veto TCP et redémarrage de la
connexion.
* **UDP** (*User Datagram Protocol*) : est un des protocoles principals de la suite des protocoles internet. UDP utilise un
modèle de communication sans connexion très simple à l'aide d'un minimum de mécanismes protocolaires. UDP fournit des sommes de
vérification pour l'intégrité des données, et des numéros de ports pour adresser différentes fonctions au niveau de la source et
de la destination du datagramme. Il ne contient pas de dialogue d'handshaking et par conséquent expose le programme utilisateur
aux problèmes éventuels de fiabilité de la connexion réseau sous-jacente ; il n'y a aucune garantie de livraison, d'ordre ni de
double protection. Si une correction d'erreur est nécessaire au niveau de l'interface réseau, une application utilisera plutôt
le protocole de contrôle de transmission (TCP) ou le protocole de transmission de contrôle de flux (SCTP) implémentés pour cet
usage. UDP est adapté aux usages où ni les contrôles ni les corrections d'erreurs ne sont nécessaires ou sont à la charge de
l'application ; UDP évite la surchage d'un tel processus dans la pile de protocole. Les applications temporellement sensibles
utilisent souvent UDP du fait qu'il est souvent préférable d'oublier des paquets plutôt que d'attendre des paquets retransmis,
ce qui peut ne pas être une option dans un système temps réel.
* **DCCP** (*Datagram Congestion Control Protocol*) : est un protocole orienté message. DCCP implémente une mise en place de
connexion et une déconnexion fiables, une notification de congestion explicite (ECN), un contrôle de congestion, et des
fonctionnalités de négociations.
* **SCTP** (*Stream Control Transmission Protocol*) : est un protocole Internet standard il permet de garder les fonctionnalités
orientées message du protocole datagramme utilisateur (UDP), tout en assurant une fiabilité et un ordonnancement des messages
ainsi que des contrôles de congestion similaires au protocole de contrôle de transmission (TCP). Contrairement à UDP et TCP, le
protocole permet le multi-homing et la redondance des chemins afin d'augmenter la résilience et la fiabilité.
* **RSVP** (*Ressource Reservation Protocol*) : est un protocole utilisé pour réserver des ressources à travers un réseau en
utilisant un modèle de services intégrés. RSVP opère à travers des réseaux IP et fournit une installation initiée par le
receveur pour la réservation de ressources pour des flux de données unicast ou multicast. Il est similaire à un protocole de
contrôle comme le protocole de messages de contrôles internet (ICMP) ou le protocole de gestion de groupes internet (IGMP).

#### Couche internet

* **IPv4** (*Internet Protocol v.4*) : est le principal protocole de communication de la suite des protocoles internet en
relayant des datagrammes à travers les frontières de réseaux. Ces fonctions de routage permettent l'aggrégation de réseaux, qui
établit essentiellement Internet. IP a pour fonction de livrer des paquets depuis un hôte source à un hôte destination
uniquement via l'adresse IP contenue dans l'entête. A cette fin, IP définit des structures de paquets qui encapsulent la donnée
à envoyer. Le protocole définit également les méthodes d'adressage utiliser pour étiqueter le datagramme des informations
concernant la source et la destination. IPv4 utilise des adresses de 32-bits qui fournissent un peu plus de 4 milliards
d'adresses. Néanmoins une grande partie de ces adresses est réservée pour des méthodes réseaux spéciales.
* **IPv6** (*Internet Protocol v.6*) : est la version la plus récente du protocole internet (IP). IPv6 a été développé pour
résoudre le problème d'épuisement du nombre d'adresse IPv4. IPv6 utilise des adresses de 128-bits soit 3,4.10^38 adresses. Les
deux protocoles ne sont pas interopérable, de fait aucune communication entre eux n'est possible. IPv6 fournit d'autres
avantages techniques en plus du plus grand espace d'adressage. En particulier, il permet des méthodes d'allocation d'adresses
hiérarchiques qui facilite l'aggrégation de routes à travers Internet, limitant l'expansion des tables de routage. L'usage de
l'adressage multicast est étendu et simplifié, il contient également d'autres optimisations pour la livraison de services. La
mobilité des appareils, la sécurité, et la configuration ont été considérés lors de la création du protocole.
* **ICMP** (*Internet Control Message Protocol*) : est un protocole de la suite des protocoles internet utilisé par les
matériels d'interconnexion pour envoyer des messages d'erreurs et autres informations opérationnelles indiquant la réussite ou
l'échec lors d'une communication avec une autre adresse IP. ICMP n'est pas utilisé pour envoyer des données applicatives entre
systèmes (à part pour des outils de diagnostics tels ping et traceroute).
* **ECN** (*Explicit Congestion Notification*) : est une extension du protocole internet (IP) et du protocole de contrôle de
transmission (TCP). ECN permet la notification bout en bout d'une congestion réseau sans oublis de paquets. ECN est une
fonctionnalité optionnelle.
* **IGMP** (*Internet Group Management Protocol*) : est un protocole de communication entres hôtes et routeurs adjacents pour
établir une appartenance à des groupes de multicasts. IGMP fait partie du multicast IP et permet au réseau de diriger les
transmissions multicasts uniquement aux hôtes qui les ont demandées.
* **IPsec** (*Internet Protocol Secure*) : est une suite de protocoles réseau sécurisée qui authentifie et chiffre les paquets
de données pour fournir une communication sécurisée à travers un réseau IP. Elle est utilisé par les réseaux privés virtuels
(VPN).

#### Couche Liaison

* **ARP** (*Address Resolution Protocol*) : est un protocole de communication utilisé pour découvrir l'adresse de la couche
liaison, telle que l'adresse MAC, associée à une une adresse de la couche internet donnée, typiquement, une adresse IP.
* **NDP** (*Neighbor Discovery Protocol*) : est un protocole de la suite des protocoles internet utilisé avec le protocole
internet version 6 (IPv6). Il est responsable de la récupération d'informations diverses requises pour la communication
internet, telles que la configuration de connexions locales et les DNS et passerelles utilisées pour communiquer avec des
systèmes plus lointains. Le protocole définit 5 paquets ICMPv6 différents pour des fonctions IPv6 similaires aux découvertes et
redirection de routeurs d'ARP et de ICMP pour IPv4. Il fournit aussi de nombreuses améliorations en ce qui concerne la
robustesse des livraisons de paquets.
* **OSPF** (*Open Shortest Path First*) : est un protocole de routage pour les réseaux IP.
* **L2TP** (*Layer 2 Tunneling Protocol*) : est un protocole de tunnellisation utilisé pour créer des réseaux privés virtuels
(VPN).
* **PPP** (*Point-to-Point Protocol*) : est un protocole de communication entre deux routeurs, sans hôte ni aucun autre
réseautage entre. Il fournit une authentification de connexion, le chiffrement des transmissions et la compression de données.
* **STP** (*Spanning Tree Protocol*) : est un protocole qui permet une topologie de réseaux Ethernet sans boucles. Le but étant
de prévenir les tempêtes de broadcast. STP permet aussi d'inclure des liens redondants ce qui fournit une tolérance aux pannes
en cas d'échec des liens actifs. STP créé un arbre couvrant qui caractérise la relation entre noeuds d'un réseau et désactive
les liens qui ne font pas partie de l'arbre couvrant, laissant un unique lien actif entre 2 noeuds.

### MIB

### Active Directory et Samba

## Logiciels

### Architecture logicielle

#### Client serveur

Le modèle client serveur est une structure d'application distribué qui sépare les tâches ou charges de travail entre
fournisseurs de ressource ou service, appelés serveurs, et les demandeurs de ce service, appelés clients. Les clients et les
serveurs communiquent souvent à travers un réseau informatique sur des matériels séparés, mais les deux peuvent également se
trouver sur la même machine. Un serveur hôte exécute un ou plusieurs programmes serveurs, qui partagent leurs ressources avec
des clients. Un client ne partage habituellement aucune de ses ressources, mais demande le contenu ou le service au serveur. Par
conséquent, les clients initient la session de communication avec les serveurs, qui attendent les requêtes entrantes.

La caractéristique client-serveur décrit la relation de programmes coopérants dans une application. Le composant serveur fournit
une fonction ou un service à un ou plusieurs clients, qui initient des requêtes pour de tels services. Les serveurs sont
classifiés en fonction du service qu'ils fournissent. Par exemple, un serveur web, sert des pages web pour un serveur de fichier
qui sert des fichiers informatiques. Une ressource partagée peut être n'importe quel composant électronique ou logiciel
informatique du serveur, des programmes et des données aux processeurs et périphériques de stockages. Le partage des ressources
d'un serveur constitue un *service*.

La condition si un ordinateur est un client, un serveur ou les deux, est déterminé par la nature de l'application qui demande
les fonctions du service. Par exemple, un seul ordinateur peut exécuter un serveur web et un logiciel serveur de fichier en même
temps pour servir différentes données aux clients effectuant diverses requêtes. Le logiciel client peut aussi communiquer avec
un logiciel serveur sur le même ordinateur. La communication entre serveurs tel que la synchronisation de données, est quelques
fois appelée communication inter-serveur ou serveur à serveur.

En général, un service est une abstraction de ressources informatiques et un client n'a pas à être concerné du comment le
serveur procède tandis qu'il exécute la requête et livre la réponse. Le client doit uniquement comprendre la réponse selon les
protocoles applicatifs connus, i.e. le contenu et le formatage de la donnée pour le service requis.

Les clients et les serveurs échangent des messages à l'aide du motif de messagerie requête-réponse. Le client envoie une
requête, et le serveur retourne une réponse. Cet échange de message est un exemple de communication inter-processus. Pour
communiquer, les ordinateurs doivent avoir un langage commun, et doivent suivre des règles communes qui doivent être définis
dans le protocole de communications. Tous les protocoles client-serveur opère au niveau de la couche application. Le protocole
de la couche application définit des motifs basiques de dialogue. Pour formaliser un peu plus loin l'échange de données, le
serveur peut implémenter une interface de programmation applicative (API). L'API est une couche d'abstraction permettant
d'accéder un service. En restreignant la communication à des contenus formatés spécifiquement, cela facilite l'analyse
syntaxique. En rendant l'accès à la donnée abstrait, on facilite l'échange de données inter-plateformes.

Un serveur peut recevoir de multiples clients distincts sur une période de temps très courte. Un ordinateur peut uniquement
exécuter un nombre limité de tâches à la fois, et s'appuie sur l'ordonnanceur système pour prioriser les requêtes entrantes des
clients pour les traiter. Afin de prévenir les abus et une disponibilité maximale, le logiciel serveur peut limiter la
disponibilité aux clients. Les attaques de déni de service sont conçues pour exploiter les obligations du serveur à traiter des
requêtes en le surchargeant avec un taux de requêtes excessif. Le chiffrement doit être mis en place si des données sensibles
sont communiquées entre le client et le serveur.

#### Trois niveaux

Une *architecture à trois niveaux* ou *architecture trois tiers* ajoute un niveau supplémentaire à l'architecture à 2 niveaux,
permettant de spécialiser les serveurs dans une tâche précise, ce qui donne un avantage de flexibilité, de sécurité et de
performance :

* un client qui demande une ressource via une interface utilisateur chargée de la présentation de la ressource ;
* un serveur d'application (appelé middleware) qui fournit la ressource, mais en faisant appel aux ressources d'un autre serveur
;
* un serveur de données qui fournit au serveur d'application les ressources requises pour répondre au client.

#### N-tiers

Une architecture à *N niveaux* ou *architecture N-tiers* n'ajoute pas de niveaux supplémentaires à l'architecture à 3 niveaux
mais introduit la notion d'objets qui offre la possibilité de distribuer les services entre les 3 niveaux selon N couches,
permettant ainsi de davantage spécialiser les serveurs.

### Haute disponibilité

La haute disponibilité est une caractéristique d'un système qui vise à assurer un certain niveau de performance opérationnelle,
généralement l'uptime (ou durée de fonctionnement), durant une période plus longue que celle attendue habituellement.

Il existe trois principes de conception système en ingénierie de fiabilité permettant d'atteindre une haute disponibilité :

* L'élimination des points de défaillances uniques. Cela signifie ajouter ou construire une redondance dans le système pour que
la défaillance d'un composant ne signifie pas la défaillance du système en entier.
* Fiabilité des points de croisements. Au niveau des systèmes redondants, le point de croisement lui-même tend à devenir un
point de défaillance unique. Les systèmes fiables doivent fournir des points de croisement fiables.
* Détection des défaillances lors de leurs occurrences. Si les deux principes ci-dessus sont observés, alors un utilisateur
pourra ne jamais voir de défaillance - mais l'activité de maintenance le doit.

### Langages de présentation

HTML/CSS

### Construction et automatisation

#### Maven

<TODO>

#### Jenkins

<TODO>
    
#### Cobbler

<TODO>

#### Puppet

<TODO>

#### Ansible

<TODO>

### Métrologie et supervision Nagios

<TODO>

### Sauvegarde

<TODO>
