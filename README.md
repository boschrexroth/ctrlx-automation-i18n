# ctrlx-automation-i18n

The Repository ctrlx-automation-i18n aggregates translation files of the ctrlX OS system that can be used to translate ctrlX OS into different languages. The repository contains the currently available languages English (en) and German (de) for apps provided by Bosch Rexroth.

## Intended use

The language files in this repository can be used as base to translate ctrlX OS into different languages. Bosch Rexroth is not liable for any harm or damage caused by language packs created and published by Third-Parties.

## Repository Layout

The translation files are organized in the folder `languages`. For each language exists exactly one folder following the ISO 639-1 two-letter code.
In this folder you will find all language files in the json format.
The folder contains three types of language files:

- **Diagnostic Messages** of the Runtime and Apps provided by Bosch Rexroth AG. They follow the file naming schema `{<app>}.diagnostics.{en-US|de-DE}.json`
- **UI Elements** of the Runtime and Apps provided by Bosch Rexroth AG. They are organized in the subfolder `webapp` and follow the file naming schema `{<component>}.{en|de}.json`
- **App Meta Data** of the Runtime and Apps provided by Bosch Rexroth AG. They follow the file naming schema `{<app>}.package-manifest.{en|de}.json`

### Versioning

The content of these folders might change for each release, including the key names, e.g. because the UI changed or elements get removed or added. Therefore it is important to notice that the language files are pinned to a specific release milestone version.
Diagnostic messages are versioned by themselves, see schema. Each version update to this repository will be reflected by a specific git tag.

### Format

All files are in json format but with a different internal schema. While the `Diagnostic Messages` follow the json-schema documented in [diagnostics.v1.schema.json](https://json-schema.boschrexroth.com/ctrlx-automation/ctrlx-os/diagnostics/diagnostics.v1.schema.json), the `App Meta Data` uses a simple key-value JSON file with namespaces in key (Flat JSON) and the `UI Elements` uses either a JSON file with namespaces in key (Flat JSON) or nested namespaces (Namespaced JSON).

#### JSON with nested namespaces (Namespaced JSON)

```json
{
    "app": {
        "buttons": {
            "apply": "Apply",
            "cancel": "Cancel"
        }
    }
}
```

#### JSON with namespaces in key (Flat JSON)

```json
{
    "app.buttons.apply": "Apply",
    "app.buttons.cancel": "Cancel"
}
```

## Usage

### Usage as Language Pack

This repository can be used to create language packs for ctrlX OS. A **language pack** is basically a ctrlX OS App that provides an additional language for the system. To make this work, a snap with the following structure needs to be created:

```
package-assets/
├── example-language-pack-fr/
│   └── i18n/
│       ├── webapp
│       │   ├── <component>.fr.json
│       │   └── ...
│       ├── <app>.package-manifest.fr.json
│       ├── ...
│       ├── <app>.diagnostics.fr-FR.json
│       └── ...
└── meta/
    └── snap.yaml
```

As you can see, the internal structure of the files is basically the content of the language folder in this repository. To make the system aware of the content in the snap, you will find the example `snapcraft.yaml` used above here:

```yaml
name: example-language-pack-fr # The name of the app, a single language pack app should only contain one language
base: bare # the base snap is the execution environment for this snap
version: "2.6.0" # The version of the app
title: Example language pack FR # A meaningful title of the app
summary: Provides language files in french for ctrlX OS # A short description of the app
description: |
  Provides French text resources for ctrlX OS user interface and diagnostic messages.
architectures:
  - build-on: [amd64]
    run-on: [all]
grade: stable
confinement: strict

parts:
  package-assets: # In this example the part "package-assets" copies the files from "source" into the snap location defined in organize
    plugin: dump # Simple dump plugin from snapcraft
    source: "./deps/language/public/languages/fr" # Location of the language files as defined in this repository
    source-type: local # Dump from local file system
    organize: # Organize the files inside of the snap to the correct location ${SNAPCRAFT_PROJECT_NAME} will be replaced by the snap name
      "*": "package-assets/${SNAPCRAFT_PROJECT_NAME}/i18n/"

slots: 
  package-assets: # The package assets slot shares the content defined in source with the ctrlX OS Device Admin App
    interface: content # Content interface for file sharing between snaps
    content: package-assets # Name of the content interface as expected by ctrlX OS
    source: # Files to share
      read: # Read-only
      - $SNAP/package-assets/${SNAPCRAFT_PROJECT_NAME}
```

## Important directions for use

### Areas of use and application

The content (e.g. source code and related documents) of this repository is intended to be used for configuration, parameterization, programming or diagnostics in combination with selected Bosch Rexroth ctrlX AUTOMATION devices.
Additionally, the specifications given in the "Areas of Use and Application" for ctrlX AUTOMATION devices used with the content of this repository do also apply.

### Unintended use

Any use of the source code and related documents of this repository in applications other than those specified above or under operating conditions other than those described in the documentation and the technical specifications is considered as "unintended". Furthermore, this software must not be used in any application areas not expressly approved by Bosch Rexroth.

## About

SPDX-FileCopyrightText: Copyright (c) 2024 Bosch Rexroth AG

<https://www.boschrexroth.com/en/dc/imprint/>

## Licenses

SPDX-License-Identifier: XOS
