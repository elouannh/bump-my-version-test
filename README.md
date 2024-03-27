# Configuration de base

## 1. Installation du package

```bash
pip install --upgrade bump-my-version
```

## 2. Création d'un fichier de configuration

Crée automatiquement un fichier de configuration avec des valeurs par défaut pour éviter de mettre les arguments
à chacune des commandes.

```bash
bump-my-version sample-config --no-prompt --destination .bumpversion.toml
```

## 3. Publication d'une version

Pour publier une nouvelle version, la commande est la suivante :

> ⚠️ Bien penser à mettre `bump` devant la version, qui n'apparaissait pas sur l'ancien package bumpversion.

```bash
bump-my-version bump <patch|minor|major>
```

# Ajout des mots-clés de pre-release

