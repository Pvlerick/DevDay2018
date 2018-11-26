# Retour d’expérience sur une migration sauce DevOps

Maxime Degallaix
maxime.degallaix@ingenico.com

Philippe Vlérick
philippe.vlerick@ingenico.com

## Historique

### Plateforme

La plateforme de paiement de Ingenico ePayments à Bruxelles (ex-Ogone) est une plateforme écrite en « ASP Classic » et en Delphi.
Nous nous intéresserons à la partie écrite en ASP, qui constitue le front-end.
La plateforme ASP souffre des travers classiques des anciennes plateformes issue d'une évolution organiques:

- technologie ancienne: langage VB Script procédural et très permissif (variables globales)
- monolithique: code fortement lié, une seule source de code partagée par plusieurs équipes
- déploiement sensible: beaucoup de control, un seul bloc de déploiement (tout ou rien) et une certaine peur du changement due au coûts des défaillances

Ce sont des critiques techniques que nous espérons objectives et établies avec le recul que nous avons presque 20 ans après, car malgré ces problèmes on ne peut remettre en cause les résultats de cette plateforme qui génère encore énormément d'argent.
Il est cependant évident pour tout le monde que la plateforme doit être modernisée.

### Migration

Il y a plusieurs années, il a été décidé de migrer la plateforme en .NET. La migration devait se faire "as is", la nouvelle plateforme devant fournir les mêmes fonctionnalités que l'ancienne.
Bien que l'ampleur de la migration ait été limité, il s'agissait néanmoins d'une migration "Big Bang" trop ambitieuse qui s'étala en longueur et tarda à produire des résultats.

Quelques années après, une seconde initiative démarra afin progressivement scinder la plateforme en micro services (ou en services tout courts, certaines fonctionnalités n'était pas "micro"...) qui devraient replacer certaines parties de la plateforme existante.
Bien que cette approche ait été plus réaliste, elle nécessitait la coexistence et l'interraction entre l'ancienne plateforme et les nouveaux services, souvent au niveau de la base de données, ce qui peut s'avérer plus complexe.
Ce nouvel effort a donné lieu à l'adoption des certaines techniques DevOps dans l'entreprise.

## Premier pilote

...

## Voyons plus grand!

### Détection de la fraude

Lors d'un brainstorming dans le cadre du "Hack Time", initiative selon laquelle tous les dévelopeurs peuvent utiliser 10% de leur temps à leur guise, un dévelopeur émit l'idée d'appeler le nouveau module de fraude depuis la plateforme ASP.
En effet, le module de détection de la fraud avait été en grande partie migré lors de la migration "Big Bang", et ce nouveau module pouvais facilement être transformé en service indépendant appelable comme le service GDPR l'était.

### Objectif principal

Ne rien faire planter et minimiser la latence ajoutée par l'appel du nouveau service.

### Objectifs secondaires

- Valider la viabilité du projet au plus tôt: certifier que l'ASP peut être modifié pour appeler le nouveau module et tenir compte ses ses résultats
- Evaluer que le temps de réponse du nouveau module est accteptable - dans des conditions réeles
- Valider que le nouveau module produit des résultats identiques à l'ancien module
- Se mettre en confiance progressivement que cette migration est possible, détecter au plus tôt si cette migration ne l'est pas

### Plan d'attaque

Le "Strangler Pattern" est devenu un classique pour les migrations (https://docs.microsoft.com/en-us/azure/architecture/patterns/strangler). Il a pour objectif d'aider à la transition d'un ancien système vers un nouveau.
L'idée est de construire une façade devant l'ancien système, de rediriger tous le code appelant vers cette façade pour pouvoir ensuire modifier le système qui a été ainsi isolé.

Le code de la plateforme ASP ne permettant pas l'introduction d'une façade tel qu'il était, la première étapte fut de faire du nettoyage afin de rassembler le code "détection de la fraude" au même endroit.
Une fois le code bien "rassemblé", il peut être modifié pour faire un appel au nouveau service externe.
Quand la validation que le nouveau module remplit bien tous les objectifs (résultats, performances...), le code ASP pourra être modifer pour y supprimer le code de la détection de la fraude qui sera dorénavant un appel au nouveau service.

## Problèmes

...

## Conclusions

### Priorités

Il est nécessaire de se donner les moyens d'essayer et de réussir ce type de projet.
Il faut s'y investir sérieusement et y consacrer un maximum de temps, y passer une demi-journée par semaine ne sera pas suffisant pour obtenir l'élan nécessaire. L'idéal est d'arriver à planifier le projet de sorte que les difficultés majeures soient au plus tôt dans le planning, ce qui augmente les chances de support pour votre projet.

### En marge du reste de l'organisation

Un projet de ce type dévie forcément des techniques habituelles de l'organisation. C'est une étape nécessaire pour le changement. Il faut donc avoir les moyens de prendre un autre chemin sur lequel il y a moins d'embûches et de processus de contrôle (ce qui ne signifie pas qu'il n'y a aucun contrôle du tout).

### Micro services

Les micro services sont très à la mode ces jours-ci, mais le but de cette présentation n'est pas de les vendre comme une solution miracle.
Par leur taille réduite (cadre limité donnant lieu théoriquement à peu de code) et leur découplage physique (ils sont déployés comme des processus séparés), ils permettent palier aux problèmes organisationnels classiques issus des plateforme monolithiques. Ils ajoutent cependant des problèmes techniques auxquels il faut se préparer: beaucoup plus de déploiements, latence due aux appels hors processus, problèmes réseaux...

### « Hack Time » & Innovation

Le projet d'intégration du nouveau  module de détection de fraude avec l'ASP n'aurait jamais vu le jour sans que l'équipe qui y travaille ait eu du temps à y consacrer, en dehors des priorités "classiques" de l'entreprise.
En permettant au développeurs d'essayer, un projet de migration très sérieux - qui avait été jugé trop complexe il y a quelques années - a vu le jour. Un opportunité que personne n'avait vue!

### Découverte de problèmes sur la plateforme existante

En plus d'être un projet qui a maintenant beaucoup de chances d'aboutir, le processus a permis de mettre le doigt sur des latences anormales dans la plateforme existante. La plateforme existante ne produisant pas assez d'éléments pour les révéler, ou le monitoring n'en était pas capable.
Cette migration a donc eu une valeur ajoutée imédiate pour la plateforme existante: nousy avons détecté des insuffisance et des problèmes qui peuvent être résolus.