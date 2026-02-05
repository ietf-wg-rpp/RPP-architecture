# RPP Architecture
This repository contains the initial draft of the RPP (RESTful Provisioning Protocol) architecture. 

## Overview
The Architecture outlines the core components, design considerations, and requirements for RPP

## Editor's copy
The actual editor's copy of the document is published [here](https://ietf-wg-rpp.github.io/RPP-architecture/). This version includes all pull requests merged so far.

## Generating IETF I-D
`draft-ietf-rpp-architecture-XX.adoc` file is the source file to generate I-D.

In order to generate I-D documents the following script [generate.sh](./generate.sh) is provided.

Required tools:
- xml2rfc
- metanorma

The [.devcontainer](./.devcontainer) folder contains configuration for [VSCode Dev Container extension](https://code.visualstudio.com/docs/devcontainers/containers) with corresponding [Dockerfile](./.devcontainer/Dockerfile) including all necessary tools.

## Contributing
This draft is open for contributions and comments. We welcome feedback and suggestions to improve the architecture. To contribute, please submit a pull request or open an issue in this repository.

Please branch from and target PR to the current editor's copy branch `dev/draft-ietf-rpp-architecture-01`.