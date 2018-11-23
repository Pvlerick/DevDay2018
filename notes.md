# Retour d’expérience sur une migration sauce DevOps

Maxime Degallaix
maxime.degallaix@ingenico.com

Philippe Vlérick
philippe.vlerick@ingenico.com

## Historique

### Plateforme

- Front-end « ASP Classic »: ancienne technologie: : procédural, variables globales, évolution organique
- Monolithique: code fortement lié, un seul code source sur lequel travaillent beaucoup de dévelopeurs
- Déploiment sensibles: fort contrôlés, un seul bloc de déploiement, défaillances couteuses, peur du changement

### Migration

- Tentative de « Big Bang » en .NET
  - Toute la plateforme en .NET en une fois
  - Beaucoup trop ambitieux
    - Très coûteux
    - Migration en couche
      - Très long avant d'obtenir des résultats
- Micro services & DevOps
  - Division de la plateforme en services et migration progressive
    - Plus réaliste, mais plus difficile car nécessité d'interraction avec l'ancienne plateform
  - Déploiements plus petits

## Premier pilote

## Voyons plus grand!