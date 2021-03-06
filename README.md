# Experience Data Model (XDM) Schema

[![CircleCI](https://circleci.com/gh/adobe/xdm.svg?style=svg)](https://circleci.com/gh/adobe/xdm)

The [Experience Data Model](https://www.adobe.io/open/standards/xdm) (XDM) is the language of digital experiences. XDM is an open standard, driven by Adobe to improve the interoperability, expressivness, and power of digital experiences. This repository contains the source code for the formal specification of the XDM model, using the JSON Schema language.

## Project Layout

The project is laid out in the following way:

- `docs`: Markdown files that contain front-matter, introduction, and non-normative parts of XDM
- `schemas`: JSON schema files constituting the normative part of XDM
  - `*.schema.json` is the schema file, e.g. `event-envelope.schema.json` – we pick the `.json` file extension for easy syntax highlighting in editors
  - `*.example.*.json` is an example file that will be validated against the `*.schema.json` file, and if successful, merged into the `*.schema.json` file as an `"example"` field at the root of the schema before it is converted into Markdown or published
  - `*.invalid.*.json` is an invalid example file that will be validated against the `*.schema.json` file. If the `invalid` file validates (false positive), the schema is too lax and the overall build will fail. `invalid` files will not be merged into documentation.
  - `*.description.md` is a description file that will be merged into the `"description"` field at the root of the schema, allowing the authoring of longer descriptions. The merge happens before publishing the schema file or creating a joint Markdown file
  - `schemas/assets` – for schemas related to the asset core concept, including image, video, document
  - `schemas/audiences` – for schemas describing audiences, segments, and groups of consumers
  - `schemas/campaigns` – for schemas describing orchestrated experience campaign
  - `schemas/content` – for schemas related to content, including visual, audible, and virtual reality content
  - `schemas/context` – for schemas related to the context of a digital experience, including profile, person, environment
  - `schemas/data` – for schemas describing collected data, metric, etc.
  - `schemas/external/*` – for JSON Schema representation of externally defined standards
  - `schemas/locations` – for schemas describing experience channels and locations therein
  - `schemas/common` – for basic concepts like extensibility that are not covered in `external` schemas
- `*.ttl`: legacy schema files in Turtle Syntax that have not yet been converted into JSON Schema
- `package.json`: a build file for `npm` that allows the generation of Markdown from the JSON Schema source files and (later) the generation of an AEM package to be deployed on the Adobe I/O Website
- `README.md`: this file
- `CONTRIBUTING.md`: guidelines for contributors, covering process, conventions and design guidelines

## Tooling

The tooling project (e.g. for generating Markdown documentation, example validation, JSON Schema documentation inlining) is located in the [adobe/jsonschema2md](https://github.com/adobe/jsonschema2md) git repository. 

### Dependencies

You need:

1. Node.js and `npm`
2. `curl`

### AEM Package Generation

When all dependencies are installed, simply run

```bash
$ npm install
$ npm run package
```

This will fetch all dependencies, then generate Markdown in the `docs/reference` directory and then generate an AEM package under `xdm-docs.zip`. The package can be uploaded using the command

```bash
$ npm run upload
```

`upload` will upload to the Adobe I/O Staging instance. To upload to production, use `upload:prod` instead.

The AEM password is not saved in this repository (of course), but you can set it using this command:

```bash
$ npm config set xdm-models:aem_password $AEM_PASSWORD
```

This is assuming `$AEM_PASSWORD` has been provided to your Continuous Integration system or set on the command line beforehand.

Running `upload` will only make the documentation visible on the "author" instance, this means visitors to the site cannot see any of the documentation. To publish it, run the `npm run activate` or `npm run activate:prod` commands.

A full process would then look like this:

```bash
$ npm config set xdm-models:aem_password $AEM_PASSWORD
$ npm install
$ npm test
$ npm run package
$ npm run upload
$ npm run activate
```
This will generate documentation, create an AEM package, install it on (stage) author, and activate it from there, so that visitors can read the documentation.

#### A Note on Dependencies

The `package.json` script will fetch a released version of the AEM Markdown Importer JAR using curl.

### Validation

This project contains a minimal validation script that depends on [NPM](https://www.npmjs.com). You can use it to validate the JSON example files against the JSON Schema files after making changes to either.

```bash
$ npm install
$ npm test

> xdm-models@0.0.1 test /Users/lars/Code/models
> sh validate.sh

Validating schemas/common/eventenvelope.example.1.json against schemas/common/eventenvelope.schema.json
schemas/common/eventenvelope.example.1.json valid
Validating schemas/common/eventenvelope.example.2.json against schemas/common/eventenvelope.schema.json
schemas/common/eventenvelope.example.2.json valid
Validating schemas/common/eventenvelope.example.3.json against schemas/common/eventenvelope.schema.json
schemas/common/eventenvelope.example.3.json valid
Validating schemas/common/eventenvelope.example.4.json against schemas/common/eventenvelope.schema.json
schemas/common/eventenvelope.example.4.json valid
Validating schemas/common/eventenvelope.example.5.json against schemas/common/eventenvelope.schema.json
schemas/common/eventenvelope.example.5.json valid

```

If you see warnings or error messages (or an non-zero exit code), fix them before making a pull request.

## Naming Conventions

* see the [Contributor's guide](CONTRIBUTING.md)

## Links

### Specifications

* [JSON Schema](http://json-schema.org/)
* [JSON-LD 1.0](https://www.w3.org/TR/json-ld/)
* [XMP SPECIFICATION PART 1 DATA MODEL, SERIALIZATION, AND CORE PROPERTIES](http://wwwimages.adobe.com/content/dam/Adobe/en/devnet/xmp/pdfs/XMP%20SDK%20Release%20cc-2014-12/XMPSpecificationPart1.pdf)

### Vocabularies / Ontologies

* [schema.org](http://schema.org)
* [Dublin Core](http://dublincore.org/)
* [OWL](http://www.w3.org/TR/2009/REC-owl2-overview-20091027/)
* [SKOS Core](http://www.w3.org/TR/2009/REC-skos-reference-20090818/)

## License/Copyright

Copyright 2017 Adobe Systems Incorporated. All rights reserved.
This file is licensed to you under the Apache License, Version 2.0 (the "License");
you may not use this file except in compliance with the License. You may obtain a copy
of the License at http://www.apache.org/licenses/LICENSE-2.0
