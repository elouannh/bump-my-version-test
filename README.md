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

## 1. Mise à jour du parser

Pour ajouter un support de pre-release, il faut mettre à jour le parser dans le fichier de configuration en ajoutant
un regular expression plus poussé qui extrait les parties supplémentaires à notre version :

```toml
parse = """(?x)
    (?P<major>0|[1-9]\\d*)\\.
    (?P<minor>0|[1-9]\\d*)\\.
    (?P<patch>0|[1-9]\\d*)
    (?:
        -                             # dash seperator for pre-release section
        (?P<pre_l>[a-zA-Z-]+)         # pre-release label
        (?P<pre_n>0|[1-9]\\d*)        # pre-release version number
    )?                                # pre-release section is optional
"""
```

## 2. Mise à jour du sérialiseur

Ensuite, on modifie la façon dont c'est sérialisé en ajoutant la possible existence d'un mot-clé de pre-release à notre
liste :

```toml
serialize = [
    "{major}.{minor}.{patch}-{pre_l}{pre_n}",
    "{major}.{minor}.{patch}",
]
```

## 3. Ajout d'une configuration avec les mots-clés possibles

On ajoute une configuration dans notre `.bumpversion.toml` permettant d'ajouter un des mots-clés définis :

```toml
[tool.bumpversion.parts.pre_l]
values = ["dev", "rc", "final"]
optional_value = "final"
```

## 4. Versionner avec le pre-release en ajoutant un mot clé

Il suffit de faire la commande :

```bash
bump-my-version show-bump
```

Pour voir la liste des "résultats" en fonction du mot-clé associé. Par exemple : 

```
$ bump-my-version show-bump
0.2.0 ── bump ─┬─ major ─ 1.0.0
               ├─ minor ─ 0.3.0
               ├─ patch ─ 0.2.1
               ├─ pre_l ─ 0.2.0-rc0
               ╰─ pre_n ─ 0.2.0-dev1
```

Ensuite, on récupère la valeur d'argument associé à la version souhaitée (par exemple, major pour passer en 1.0.0).
Par exemple, si on fait `bump-my-version bump pre_n`, la version sera 0.2.0-dev1.

# Bravo, lecture terminée

Normalement, tout devrait marcher (s'il manque des trucs, je rajouterai).

Voir le fichier README.md pour avoir un exemple complet.