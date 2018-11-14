# Notes

## Introduction

## Contexte

### Plateforme

- Monolithique et code fortement lié, un seul code source sur lequel travaillent beaucoup de dévelopeurs
- Ancienne technologie: « ASP Classic »: procédural, variables globales, évolution organique
- Déploiment sensibles: frot contrôlés, une seule unité de déploiement, défaillances couteuses, peur du changement

### Migration

- Migration de toute la plateforme en .NET en une fois (big-bang)
  - Projet trop ambitieux
- Migration progressive
  - Plus réaliste, mais nécessite une interraction avec l'ancienne plateforme
  - Choix de modules candidats pour la migration: le module détection de fraude
    - Le module de fraud avait auparavant été migré en .NET pour la migration "big-bang" mais n'avais jamais été utilisé

## La migration

### Point de départ

Le code du module de détection de fraud se trouve dans le code ASP. 
But de la migration: une migration sans douleur!

## Préparation

Réorganisation du code ASP, préparation pour le "strangler pattern"
Phase risquée du project - bonne nouvelle, si celà ne marche pas, on peut arrêter là. 

## Comparaison

L'ASP apelle un service externe (en HTTP) qui possède un pipeline de déploiement à part, bien plus rapide que celui de l'ASP.

## Arrivée

## Problèmes encontrés

### Latence SQL

- 

## Références

- The DevOps Handbook 