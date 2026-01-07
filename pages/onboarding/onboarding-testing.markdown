---
layout: page
title: Workflow validation of User Stories (US)
permalink: /onboarding-testing/
up: ../onboarding/
position: left
order: 3000
---

## Introduction

Ce workflow garantit que chaque US soit validée par plusieurs reviewers avant de passer automatiquement en "done".

## Étapes détaillées

1. **Passage en Recette**
   - L’US est déplacée dans la colonne/statut "recette" du board GitHub Projects une fois les développements de celle-ci terminés et mergés.
   - Assignation des reviewers (QA, Métier, PO) sur l’issue.

2. **Tests & Validation par reviewers**
   - Chaque reviewer prend connaissance des critères d’acceptance : spécifiques & génériques.
   - Exécution des scénarios de test associés.
   - Documentation des résultats et observations dans l’issue (commentaire ou checklist collaborative).

3. **Checklist collaborative dans l’issue**
   ```markdown
   ### Checklist de validation collaborative

   - [ ] QA – Validation des tests fonctionnels et critères génériques
   - [ ] Métier – Validation conformité métier
   - [ ] PO – Arbitrage fonctionnel/validation finale
   ```
   > Chaque reviewer coche sa case après validation ou ajoute le commentaire « Validation OK [rôle] ».

4. **Bascule en Done**
   - Dès que toutes les cases de la checklist sont cochées/commentaires tous explicites, l’US est déplacée de "recette" à "done" sur le board (manuellement ou automatiquement par action programmée).
   - Un commentaire synthétique final peut être ajouté (« US validée par tous les reviewers, passage à done »).

5. **Archivage & notification**
   - Les résultats sont archivés (trace de validation dans l’issue).
   - Optionnel : notification automatisée ou ping de l’équipe.

---

## Bonnes pratiques

- Toujours mentionner les reviewers dans la checklist ou les assigner sur l’issue.
- Documenter toutes les validations/refus/observations pour traçabilité. Soit par des commentaires dans l'US soit par la création d'une nouvelle issue de type `Bug` ou `Enhancement`.
- Automatiser la bascule en "done" si possible (GitHub Actions sur checklist complète).

---

## Illustration visuelle du workflow

```
╔════════════╦═════════════════════╦════════════════╦═════════════════╦══════════╗
║   US       ║  Assignation        ║  Validation    ║  Checklist      ║   Done   ║
╠════════════╬═════════════════════╬════════════════╬═════════════════╬══════════╣
║ Créée      ║ Assign QA, Métier,  ║ QA teste       ║ [x] QA          ║          ║
║            ║ PO dans l’issue     ║ Métier valide  ║ [x] Métier      ║          ║
║            ║                     ║ PO arbitre     ║ [x] PO          ║   ✓      ║
╚════════════╩═════════════════════╩════════════════╩═════════════════╩══════════╝
                            ↓
        Quand toutes les cases sont cochées (ou validation dans les
        commentaires par tous), la carte passe automatiquement en "done"
```

---

### SCHÉMA VISUEL (Markdown, onboarding)

```markdown
graph TD
    A[US "recette"] --> B[Assignation des reviewers<br>(QA, Métier, PO)]
    B --> C[Exécution des tests d'acceptance]
    C --> D[Chaque reviewer documente et valide dans l’issue<br>(checklist/cocher, commentaire)]
    D --> E[Checklist complète]
    E --> F[Passage auto/manual à "done" sur le board]
    F --> G[Archiver résultat & notifier équipe]
```

---

## À mettre en place
- Utiliser le template de checklist dans chaque issue de recette.
- Àjouter les tests d'acceptances génériques quand ils n'ont pas été listés dans les anciennes US
```markdown
### Critères d'acceptance

#### critères génériques
- [ ] Accessibilité : Si applicable, vérification du respect des standards RGAA ou WCAG + test clavier et voix.
- [ ] Performance : chargement raisonnable, fonctionnement sans lenteur.
- [ ] Ergonomie/Mobile : rendu responsive, fonctionnel sur desktop, tablette et mobile.
- [ ] Tests/Sécurité basique : pas de crash, pas d’erreur inattendue affichée (test d'insertion de valeurs non conformes).
- [ ] Compatibilité navigateurs : tests sur les principaux navigateurs (Chromium, Google Chrome, Firefox, Edge, Safari) sur des versions qui ont moins de 6 mois, spécifiques desktop et mobile.

#### critères spécifiques
<!-- Lister les critères d'acceptance ci après -->
- [ ] Critère 1: Describe the first acceptance criterion here.
...
```
- Automatiser le changement de statut « Recette » → « Done » via GitHub Actions si possible.

---

*Publié le 2026-01-07 !*