# Workflow de développement assisté par IA : OpenCode + Repomix + OpenSpec

Ce dépôt contient deux commandes personnalisées pour **OpenCode** (`openspec-create` et `openspec-enrich`) qui s’intègrent parfaitement avec **Repomix** et **OpenSpec** pour former un pipeline de développement robuste, basé sur des spécifications claires et un contexte maîtrisé.

## Objectif

Permettre à une équipe de développement de :
- **Spécifier** une fonctionnalité de manière exhaustive et structurée (OpenSpec),
- **Contextualiser** le travail de l’IA avec l’ensemble du code existant (Repomix),
- **Implémenter** de façon autonome mais supervisée (OpenCode).

Le résultat : moins d’ambiguïtés, une traçabilité des décisions, et un gain de temps significatif.

## Les outils

- **[OpenCode](https://github.com/opencode)** : Agent de codage IA capable de planifier, générer et modifier du code dans un projet réel.
- **[Repomix](https://github.com/repomix)** : Utilitaire qui condense l’intégralité d’un dépôt en un seul fichier (XML, Markdown…) pour fournir un contexte complet à l’IA.
- **[OpenSpec](https://github.com/openspec)** : Gestionnaire de spécifications « vivantes » qui archive les décisions et guide l’implémentation via des fichiers Markdown versionnés.

## Workflow typique

1. **Préparer le contexte**  
```bash
npx repomix --output context.xml
````
2. **Lancer OpenCode en mode plan**
```bash
opencode --plan
```
3. **Créer une nouvelle spécification**
Dans la session OpenCode, tapez :
```text
/openspec-create "Description de votre idée"
```
La commande engage un dialogue interactif (questions/réponses) pour collecter les informations nécessaires, puis génère automatiquement l’arborescence OpenSpec dans openspec/changes/[feature-name]/.

4. **Enrichir une spécification existante**
Pour compléter ou affiner une feature déjà documentée :
```text
/openspec-enrich "NomDeLaFeature"
```
L’IA analyse les fichiers OpenSpec déjà présents, identifie les lacunes (exigences manquantes, critères d’acceptation flous…), et pose des questions ciblées pour les combler sans rien supprimer.

5. **Valider et synchroniser**
Une fois la spécification satisfaisante, utilisez la commande native d’OpenSpec :
```text
/openspec:update
```
Cela génère les instructions pour l’implémentation (fichiers tasks.md mis à jour, etc.).

6. Passer à l’implémentation
Basculez en mode dev dans OpenCode et commencez à coder en suivant les tâches définies.

## Les commandes personnalisées
Elles se trouvent dans le dossier .opencode/command/ et sont automatiquement détectées par OpenCode.

### openspec-create.md
- **Rôle** : Générer une spécification complète à partir d’une idée brute.
- **Fonctionnement** : Dialogue Q/R séquentiel (une question à la fois) sur les fonctionnalités, utilisateurs, contraintes, architecture, défis et critères de succès.
- **Livrables** : proposal.md, specs/overview.md, specs/requirements.md, specs/user-stories.md, tasks.md.

### openspec-enrich.md
- **Rôle** : Améliorer une spécification existante sans créer de doublons.
- **Fonctionnement** : Lecture des fichiers OpenSpec déjà présents, identification des manques, puis questions ciblées pour les compléter.
- **Règles** : Ne jamais supprimer, toujours ajouter en marquant les ajouts par [ADDED].

### Pourquoi adopter ce workflow ?
- **Clarté** : Les spécifications sont centralisées, versionnées et suivent un format standard.
- **Efficacité** : L’IA dispose de tout le contexte nécessaire (code + specs) pour produire un code cohérent.
- **Évolutivité** : Les décisions architecturales sont documentées et réutilisables pour les futures features.
- **Maîtrise** : L’humain garde le contrôle à chaque étape (validation des spécifications, revue du code).

Prêt à booster votre productivité ?
Placez ces scripts dans .opencode/command/, lancez opencode --plan et commencez par /openspec-create "Ma prochaine killer feature".
