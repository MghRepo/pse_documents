# Environnement du système d'information

#### Introduction

Cet exposé traite de la partie environnement du système d'information du programme PSE.

#### Table des matières

* [Normalisation](#normalisation)
    + [ITIL](#itil)
    + [COBIT](#cobit)
* [Notions générales sur le droit de l'informatique](#notions-générales-sur-le-droit-de-linformatique)
    + [Protection des données individuelles](#protection-des-données-individuelles)
    + [L'usage de la messagerie](#lusage-de-la-messagerie)
    + [Rôle de la CNIL](#rôle-de-la-cnil)
    + [Licences](#licences)
* [Organisation du travail](#organisation-du-travail)
    + [Méthode agile](#méthode-agile)
    + [Devops](#devops)
* [Fonctions de PSE](#fonctions-de-pse)
* [Plan de secours](#plan-de-secours)

## Normalisation

### ITIL

ITIL (*Information Technology Infrastructure Library*) est un ensemble d'ouvrages recensant les bonnes pratiques de gestion du
système d'information.

C'est un référentiel méthodologique très large qui aborde les sujets suivants :

* Comment organiser un système d'information ?
* Comment améliorer l'efficacité d'un système d'information ?
* Comment réduire les risques ?
* Comment augmenter la qualité des services informatiques ?

Les recommandations d'ITIL positionnent des blocs organisationnels et des flux d'information.

### COBIT

COBIT (*Control Objectives for Information and related Technology*) est un référentiel de bonnes pratiques d'audit informatique
et de gouvernance des systèmes d'information.

## Notions générales sur le droit de l'informatique

### Protection des données individuelles

Le règlement général sur la protection des données (RGPD) est un règlement de l'Union européenne qui constitue le texte de
référence en matière de protection des données à caractère personnel. Il renforce et unifie la protection des données pour les
individus au sein de l'Union européenne.

Les principaux objectifs du RGPD sont d'accroître à la fois la protection des personnes concernées par un traitement de leurs
données à caractère personnel et la responsabilisation des acteurs de ce traitement. Ces principes pourront être appliqués grâce
à l'augmentation du pouvoir des autorités de contrôle.

### L'usage de la messagerie

### Rôle de la CNIL

La Commission nationale de l'informatique et des libertés (CNIL) est une autorité administrative indépendante française. La CNIL
est chargée de veiller à ce que l'information soit au service du citoyen et qu'elle ne porte atteinte ni à l'identité humaine,
ni aux droits de l'homme, ni à la vie privée, ni aux libertés individuelles ou publiques.

#### Missions Principales

* **Informer** : La CNIL est investie d'une mission générale d'information des personnes sur leurs droits et leurs obligations.
Elle aide les citoyens dans l'exercice de leurs droits. Elle établit chaque année un rapport public rendant compte de
l'exécution de sa mission.
* **Réguler** :  La CNIL régule et recense les fichiers, autorise les traitements les plus sensibles avant leur mise en place.
L'avis de la CNIL doit d'ailleurs être sollicité avant toute transmission au Parlement d'un projet de loi relatif à la
protection des données personnelles ; il doit aussi être sollicité par le Gouvernement avant d'autoriser les traitements
intéressant la sûreté de l'État, la défense ou la sécurité publique. La CNIL établit des normes simplifiées, afin que les
traitements les plus courants fassent l'objet de formalités allégées. Elle peut aussi décider de dispenser de toute déclaration
des catégories de traitement sans risque pour les libertés individuelles. Elle agit par voie de recommandations.
* **Protéger** : La CNIL doit veiller à ce que les citoyens soient informés des données contenues dans les traitements les
concernant et qu'ils puissent y accéder facilement. Elle reçoit et instruit les plaintes des personnes qui rencontrent des
difficultés à exercer leurs droits. Elle exerce, pour le compte des citoyens qui le souhaitent, l'accès aux fichiers intéressant
la sûreté de l'État, la défense et la sécurité publique, notamment des services de renseignements de la police judiciaire.
* **Contrôler** : La CNIL vérifie que la loi est respectée en contrôlant les traitements informatiques. Elle peut de sa propre
initiative se rendre dans tout local professionnel et vérifier sur place et sur pièce les fichiers. La Commission use de ses
pouvoirs d'investigation pour instruire les plaintes et disposer d'une meilleure connaissance de certains fichiers. La CNIL
surveille par ailleurs la sécurité des système d'information en s'assurant que toutes les précautions sont prises pour empêcher
que les données ne soient déformées ou communiquées à des personnes non autorisées.
* **Sanctionner** : Lorsqu'elle constate un manquement à la loi, la CNIL peut, après avoir mis en demeure les intéressés de
mettre fin à ce manquement, prononcer diverses sanctions.
* **Anticiper** : La CNIL doit s'attacher à comprendre et anticiper les développements des technologies de l'information afin
d'être en mesure d'apprécier les conséquences qui en résulte pour l'exercice des droits et libertés. Elle propose au
Gouvernement les mesures législatives ou réglementaires de nature à adapter la protection des libertés et de la vie privée à
l'évolution des techniques.

#### Droits

* **Droit d'information** : Toute personne peut s'adresser directement à un organisme pour savoir si elle est fichée ou pas.
* **Droit d'accès** : Sauf pour les fichiers relevant du droit d'accès indirect, toute personne peut, gratuitement, sur simple
demande avoir accès à l'intégralité des informations la concernant sous une forme accessible. Elle peut également obtenir copie
moyennant le paiement, le cas échéant, des frais de reproduction.
* **Droit de rectification et de radiation** : Toute personne peut demander directement que les informations détenues sur elles
soient rectifiées (si elles sont inexactes), complétées ou clarifiées (si elles sont incomplètes ou équivoques), mises à jour
(si elles sont périmées) ou effacées (si ces informations ne pouvaient pas être légalement collectées par l'organisme concerné).
* **Droit d'opposition** : Toute personne peut s'opposer à ce qu'il soit fait un usage des informations la concernant à des fins
publicitaires ou de prospection commerciale ou que ces informations la concernant soient cédées à des tiers à telles fins. La
personne concernée doit être mise en mesure d'exercer son droit d'opposition à la cession de ses données à des tiers dès leur
collecte.
* **Droit d'accès indirect** : Toute personne peut demander à la CNIL de vérifier les informations la concernant éventuellement
enregistrées dans des fichiers intéressant la sûreté de l'État, la défense, ou la sécurité publique (droit d'accès indirect). La
CNIL mandate l'un de ses membres magistrats (ou anciens magistrats) afin de vérifier leur rectification ou leur suppression.
Avec l'accord du responsable du traitement, les information concernant une personne peuvent lui être communiquées.

#### Obligation des responsables du traitement

* Notifier la mise en oeuvre du fichier de ses caractéristiques à la CNIL, sauf cas de dispense prévus par la loi ou par la
CNIL.
* Mettre les personnes concernées en mesure d'exercer leur droits en les informant.
* Assurer la sécurité des informations afin d'empêcher qu'elles soient déformées, endommagées ou que des tiers non autorisés n'y
ait accès. La loi prévoit une obligation de mesures techniques et d'organisation, un obligation de moyens, dénuée d'obligation
de résultat.
* Se soumettre aux contrôles et vérifications sur place de la CNIL et répondre à toute demande de renseignement qu'elle formule
dans le cadre de ses missions.


### Licences

Une licence de logiciel est un contrat par lequel le titulaire des droits d'auteur sur un programme informatique définit avec
son cocontractant (exploitant ou utilisateur) les conditions dans lesquelles ce programme peut être utilisé ou modifié.

Une licence peut être :

* **nominative ou fixe** : conçue pour être installée sur un ordinateur particulier.
* **flottante** : fonctionne à l'aide d'un serveur de licences. Celui-ci décompte le nombre de licences utilisées à un instant T
sur le réseau.
* **shareware** : ou partagiciel, attribue un droit temporaire et/ou avec des fonctionnalités limitées d'utilisation. Après
cette période d'essai, l'utilisateur devra rétribuer l'auteur pour continuer à utiliser le logiciel ou avoir accès à la version
complète.
* **libre** : qui donnent le droit d'usage de l'oeuvre, d'étude de l'oeuvre pour en comprendre le fonctionnement ou l'adapter à
ces besoins, de modification et de redistribution de l'oeuvre et des oeuvres dérivées, c'est à dire sa diffusion à d'autres
usages, y compris commercialement.
* **propriétaire** : ne permet pas légalement ou techniquement, ou par quelque autre moyen que ce soit d'exercer simultanément
les quatre libertés logicielles.

## Organisation du travail

### Méthode agile

### Devops

![dev-ops](https://blogs.vmware.com/management/files/2020/03/2020-03-02_14-18-57.png "dev-ops")

## Fonctions de PSE

### DISI

* Veiller à l'optimisation des performances des matériels et à celle du taux de disponibilité du système informatique.
* Assurer la gestion des sécurités d'accès et des sauvegardes, ainsi que l'administration des bases de données.
* Assurer la mise en oeuvre d'automates d'exploitation et de programmes utilitaires.
* Exercer des missions d'assistance et de conseil des équipes d'exploitation.
* Exercer un support technique pour les cellules d'assistance directe et pour l'administration des configurations informatiques
implantées dans les services locaux.

### Service centraux

* Participer à la conception technique des systèmes de données et de traitements afin d'optimiser l'usage du système
informatique et préparer les modalités de mise en exploitation.
* Procéder aux études préalables, aux acquisitions de matériels informatiques et de logiciels système.
* Participer aux travaux sur les systèmes gros systèmes, X86, plateformes virtualisées et Cloud.
* Gérer les techniques de cette exploitation ainsi que le suivi et l'environnement de cette exploitation.

## Plan de secours

### Plan de continuité d'activité

Un plan de continuité d'activité (PCA), a pour but de garantir la survie de l'entreprise en cas de sinistre important touchant
le système informatique. Il s'agit de redémarrer l'activité le plus rapidement possible avec le minimum de perte de données. Ce
plan est un des points essentiels de la politique de sécurité informatique d'une entreprise.

Pour qu'un plan de continuité soit réellement adapté aux exigences de l'entreprise, il doit reposer sur une analyse de risque et
une analyse d'impact :

* **L'analyse de risque** débute par une identification des menaces sur l'informatique. Les menaces peuvent être internes ou
externes à l'entreprise. On déduit ensuite le risque qui découle des menaces identifiées ; on mesure l'impact possible de ces
risques. Enfin, on décide de mettre en oeuvre des mesures d'atténuation des risques en se concentrant sur ceux qui ont un impact
significatif. Par exemple, si le risque de panne d'un équipement risque de tout paralyser, on installe un équipement redondant.
Les mesures d'atténuation de risque qui sont mises en oeuvre diminuent le niveau de risque, mais elles ne l'annulent pas : il
subsiste toujours un risque résiduel, qui sera couvert soit par le plan de continuité, soit par d'autres moyens (assurance,
voire acceptation du risque).
* **L'analyse d'impact** consiste à évaluer quel est l'impact d'un risque qui se matérialise et à déterminer à partir de quand
cet impact est intolérable, généralement parce qu'il met en danger les processus essentiels (donc, la survie) de
l'entreprise.L'analyse d'impact se fait sur la base de désastres : on considère des désastres extrêmes voire improbables (par
exemple, la destruction totale du bâtiment) et on détermine les impacts financiers, humains, légaux, etc., pour des durées
d'interruption de plus en plus longues jusqu'à ce qu'on atteigne l'impact maximal tolérable. Le résultat principal de l'analyse
d'impact est donc une donnée temporelle : c'est la durée maximale admissible d'une interruption de chaque processus de
l'entreprise. En tenant compte des ressources informatiques (réseaux, serveurs, PCs, etc.) dont chaque processus dépend, on peut
déduire le temps maximal d'indisponibilité de chacune de ces ressources, en d'autres termes, le temps maximal après lequel une
ressource informatique doit avoir été remise en fonction.

Une analyse de risque réussie est le résultat d'une action collective impliquant tous les acteurs du système d'information :
techniciens, utilisateurs et managers.

Il existe plusieurs méthodes pour assurer la continuité de service d'un système d'information. Certaines sont techniques (choix
des outils, méthodes de protection d'accès et de sauvegarde des données), d'autres reposent sur le comportement individuel des
utilisateurs (extinction des postes informatiques après usage, utilisation raisonnable des capacités de transfert
d'informations, respect des mesures de sécurité); sur des règles et connaissances collectives (protection incendie, sécurité
d'accès aux locaux, connaissance de l'organisation informatique interne de l'entreprise) et de plus en plus sur des conventions
passées avec des prestataires (copie des programmes, mise à disposition de matériel de secours, assistance au dépannage).

Les méthodes se distinguent entre préventives (éviter la discontinuité) et curatives (rétablir la continuité après un sinistre).
Les méthodes préventives sont souvent privilégiées, mais décrire les méthodes curatives est une nécessité car aucun système
n'est fiable à 100%.

La préservation des données passe par des copies de sauvegarde régulières. Il est important de ne pas stocker ces copies de
sauvegarde à côté du matériel informatique, voire dans la même pièce car elles disparaîtraient en même temps que les données à
sauvegarder en cas d'incendie, de dégât des eaux, de vol, etc. Lorsqu'il est probable que les sauvegardes disparaissent avec le
matériel, le stockage des copies de sauvegarde peut alors être nécessaire dans un autre lieu différent et distant.

L'analyse d'impact a fourni des exigences exprimées en temps maximal de rétablissement des ressources après un désastre (RTO :
*Recovery Time Objective* ou Durée maximale d'interruption admissible) et la perte maximale de données (RPO : *Recovery Point
Objective* ou Perte de données maximale admissible). La stratégie doit garantir que ces exigences seront observées.

Il s'agit de disposer d'un système informatique équivalent à celui pour lequel on veut limiter l'indisponibilité : ordinateurs,
périphériques, systèmes d'exploitation, programmes particuliers, etc. Une des solutions consiste à créer et maintenir un **site
de secours**, contenant un système en ordre de marche capable de prendre le relais de système défaillant. Selon que le système
de secours sera implanté sur le site d'exploitation ou sur un lieu géographiquement différent, on parlera d'un secours *in situ*
ou *déporté*.

Pour répondre aux problématiques de recouvrement de désastre, on utilise de plus en plus fréquemment des sites délocalisés,
c'est à dire physiquement séparés des utilisateurs de plusieurs centaines de mètres à plusieurs centaines de kilomètres : plus
le site est éloigné, moins il risque d'être touché par un désastre affectant le site de production. Mais la solution étant
d'autant plus chère, car la bande passante qui permet de transférer des données d'un site vers l'autre est alors généralement
plus coûteuse et risque d'être moins performante. Cependant la généralisation des réseaux longues distances et la baisse des
coûts de transmission rendent moins contraignante la notion de distance : le coût du site ou la compétence des opérateurs (leur
capacité à démarrer le secours rapidement et de rendre l'accès aux utilisateurs) sont d'autres arguments de choix.

Les sites de secours (*in situ* ou déportés) se classent selon les types suivants :

* **salle blanche** (une salle machine protégée par des procédure d'accès particulières, généralement secourue électriquement).
Par extension on parle de *salle noire* pour une salle blanche entièrement pilotée à distance, sans aucun opérateur à
l'intérieur.
* **site chaud** : site de secours où l'ensemble des serveurs et autres systèmes sont allumés, à jour, interconnectés,
paramétrés, alimentés à partir des données sauvegardées et prêt à fonctionner. Le site doit aussi fournir l'ensemble des
infrastructures pour accueillir l'ensemble du personnel à tout moment et permet une reprise de l'activité dans des délais
relativement courts (quelques heures). Un tel site revient quasiment à doubler les capacités informatiques de l'entreprise (on
parle de **redondance**) et présente donc un poids budgétaire non négligeable.
* **site froid** : site de secours qui peut avoir une autre utilisation en temps normal. Les serveurs et autres systèmes sont
stockés mais non installés, connectés, etc. Lors d'un sinistre, un important travail doit être effectué pour mettre en service
le site ce qui conduit à des temps de reprise long (quelques jours). Mais sont coût de fonctionnement, hors période d'activation
est faible voire nul.

Il est aussi possible d'utiliser des systèmes distribués sur plusieurs sites (diminution du risque de panne par effet de
foisonnement) ou un **site de secours mobile**.

Plus les temps de rétablissement garantis sont courts, plus la stratégie est coûteuse. Il faut donc choisir la stratégie qui
offre le meilleur équilibre entre le coût et la rapidité de reprise.

D'autre part pour des problème de haute disponibilité on a recours aussi à de la redondance mais de manière plus locale.

* Doublement d'alimentation des baies des serveurs
* Redondance des disques en utilisant la technologie RAID
* Redondance de serveurs avec des systèmes de répartition de charge ou de *heartbeat* (un serveur demande régulièrement sur le
réseau si son homologue est en fonctionnement et lorsque l'autre serveur ne répond pas, le serveur de secours prend le relais).

Il est aussi possible de recourir à un site secondaire de haute disponibilité qui se situe généralement près du site de
production (moins de 10km) afin de permettre de les relier avec de la fibre optique et synchroniser les données des deux sites
quasiment en temps réel de manière synchrone ou asynchrone selon les technologies utilisées, les besoins et les contraintes
techniques.

Quel que soit le degré d'automatisation et de sécurisation d'un système informatique, la composante humaine reste un facteur
important. Pour limiter le risque de panne, les acteurs d'un service informatique doivent adopter les comportements les moins
risqués pour le système et éventuellement savoir accomplir des gestes techniques.

* Pour les utilisateurs, il s'agit :
    + de respecter les normes d'utilisation de leurs ordinateurs : n'utiliser que les applications référencées par les
    mainteneur du SI, ne pas surcharger les réseaux par des communications inutiles, respecter la confidentialité des codes
    d'accès.
    + de savoir reconnaître les symptômes d'une panne (distinguer un blocage d'accès d'un délai de réponse anormalement long par
    exemple) et savoir en rendre compte le plus vite possible.
* Pour les opérateurs du SI, il s'agit d'avoir la meilleure connaissance du système en terme d'architecture (*cartographie* du
SI) et de fonctionnement (en temps réel si possible), de faire régulièrement les sauvegardes et de **s'assurer qu'elles sont
utilisables**.
* Pour les responsables, il s'agit de faire les choix entre réalisations internes et prestations externes de manière à couvrir
en totalité le champ des actions à conduire en cas de panne, de passer les contrats avec les prestataires, d'organiser les
relations entre les opérateurs du SI et les utilisateurs, de décider et mettre en oeuvre les exercices de secours, y compris le
retour d'expérience.

Selon la gravité du sinistre et la criticité du système en panne, les mesures de rétablissement seront différentes.

Dans l'hypothèse de la reprise de données, seules des données ont été perdues. L'utilisation des sauvegardes est nécessaire et
la méthode consiste à réimplanter le dernier jeu de sauvegardes. Cela peut se faire dans un laps de temps relativement court, si
l'on a bien identifié les données à reprendre et si les méthodes et outils de réimplantation sont accessibles et connus.

A un seuil de panne, plus important, une ou des applications sont indisponibles. L'utilisation d'un site de secours est
envisageable, le temps de rendre disponible l'application en cause.

Le redémarrage des machines :

* provisoire : utilisation des sites de secours
* définitif : après dépannage de la machine d'exploitation habituelle, y rebasculer les utilisateurs, en s'assurant de ne pas
perdre de données et si possible de ne pas déconnecter les utilisateurs.

Le plan de reprise contient les informations suivantes :

* La composition et le rôle des équipes de pilotage du plan de reprise. Ces équipes se situent au niveau stratégique :
    1. les dirigeants qui ont autorité pour engager des dépenses ;
    2. le porte-parole responsable des contacts avec les tiers ;
    3. au niveau tactique, les responsables qui coordonnent les actions ;
    4. au niveau opérationnel, les hommes de terrain qui travaillent sur le site sinistré et sur le site de remplacement.

La composition de ces équipes doit être connue et à jour, ainsi que les personnes de remplacement et les moyens de les prévenir.
Les membres des équipes doivent recevoir une formation.

* Les procédures qui mettent la stratégie en oeuvre. Ceci inclut les procédures d'intervention immédiate ;
* Les procédures pour rétablir les services essentiels, y compris le rôle des prestataires externes ;
* Les procédures doivent être accessibles aux membres des équipes de pilotage, même en cas d'indisponibilité des bâtiments.

Le plan doit être régulièrement essayé au cours d'exercices. Un exercice peut être une simple revue des procédures,
éventuellement un jeu de rôles entre les équipes de pilotage. Un exercice peut aussi être mené en grandeur réelle, mais peut se
limiter à la reprise d'une ressource (par exemple un serveur principal), ou à une seule fonction du plan (par exemple la
procédure d'intervention immédiate). Le but de l'exercice est multiple :

* Vérifier que les procédures permettent d'assurer la continuité d'activité
* Vérifier que le plan est complet et réalisable
* Maintenir un niveau de compétence suffisant parmi les équipes de pilotage
* Évaluer la résistance au stress des équipes de pilotage

Un plan doit aussi être revu et mis à jour régulièrement pour tenir compte de l'évolution de la technologie et des objectifs de
l'entreprise. La seule façon efficace de mettre à jour le PCA est d'en sous traiter la maintenance aux métiers afin qu'il soit
réactualisé à chaque réunion mensuelle de service.

### Plan de reprise d'activité

Un plan de reprise d'activité (PRA) constitue l'ensemble des procédures documentées lui permettant de rétablir et de reprendre
ses activités en s'appuyant sur des mesures temporaires adoptées pour répondre aux exigences métier habituelles après un
incident.

Le plan de reprise d'activité comprend les tâche suivantes :

* Identification des activités critiques ;
* Identification des ressources ;
* Identification des solutions pour le maintien des activités critiques.
