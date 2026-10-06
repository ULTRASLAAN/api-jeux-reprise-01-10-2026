# 1. Fusionner les PR par squash

- **Date** : 2026-10-06
- **Statut** : Proposée

## Contexte
Pendant le développement, les membres de l'équipe créent de nombreux commits intermédiaires (`wip`, retours de relecture). Nous devons définir une stratégie de fusion uniforme pour préserver un historique `main` clair et exploitable.

## Options envisagées

1. **Merge commit**
   - *Pour* : Conserve tout l'historique et la structure de l'arbre Git.
   - *Contre* : Pollue `main` avec des commits de travail non significatifs.

2. **Rebase and merge**
   - *Pour* : Historique parfaitement linéaire.
   - *Contre* : Exige que chaque commit individuel de la branche soit déjà propre.

3. **Squash and merge**
   - *Pour* : Regroupe tous les commits de la PR en un seul commit propre sur `main`.
   - *Contre* : Perte du détail des commits intermédiaires de la branche.

## Décision
Nous retenons l'option **Squash and merge**. Le titre de la PR sert de message au commit final sur `main`.

## Conséquences
- **Point positif** : Historique `main` très lisible (1 commit = 1 PR relue).
- **Point négatif** : Impossibilité de revenir à un commit intermédiaire précis au sein d'une PR après fusion.
- **Condition de révision** : Nous reverrons ce choix si un besoin strict d'audit commit par commit survient.