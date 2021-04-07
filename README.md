# Vue Outline

## Introduction

The Vue Outline project is a Vuetify based single page app that uses a Drupal instance running the Drupal Outline module as a backend.  Vue Outline provides an improved interface for Drupal content editors, using the Material Design Component library, drag and drop functionality, workflow integration, etc.  The Drupal GraphQL module combined with a node.js server is used for communication between the Vue app and the Drupal back end. 

Primary capabilities supported:

- A better interface for Drupal content editing.
- A way to publish a "book" of content.
- An administrative interface for Drupal.

A Drupal Outline is conceptually similar to a Drupal Book, except that an outline may contain any entity type, not just nodes.  Vue Outline will support editing of Drupal entities using Pupetteer for screen scraping of Drupal node editing pages, which means that a custom GraphQL configuration is not required for editing of custom content types.  The screen scaping technology is also usefull for providing administrative functionality, for example the screen scraper can execute the Drupal Admin UI Site Status page periodically and cache the output so that the current status information is instantly available. 

## Project setup

These must be installed globally prior to installation: git, node, npm, vue, composer, lando/docker.
Before installation, make sure you have drupal account set up with git access, SSH Key, and your global git user/email set.

### Step 1: Clone and install the Vue project

```bash
git clone git@github.com:captaindav/vue-outline
cd vue-outline
npm i
```

### Step 2: Initialize Drupal

Mac OSX:```bash npm run init:drupal:mac```
(requires composer.phar in /usr/local/bin) 

Windows:```bash npm run init:drupal:windows```

### Step 3: Configure Drupal GraphQL Server and VS Code

#### Drupal GraphQL Server

- Login at [http://drupal-outline.lndo.site/user](http://drupal-outline.lndo.site/user), username=admin password=admin
- Navigate to [http://drupal-outline.lndo.site/admin/config/graphql](http://drupal-outline.lndo.site/admin/config/graphql) and create a GraphQL server with these settings:
-- Label: Outline
-- Schema: Outline Schema
-- Endpoint: /outline-graphql
-- Check 'Allow query batching' and 'Enable caching'
- Navigate to: [http://drupal-outline.lndo.site/admin/people/permissions](http://drupal-outline.lndo.site/admin/people/permissions) and give the Anonymous role the GraphQL permissions:
-- Outline: Execute arbitrary requests
-- Outline: Execute persisted requests

### Step 4: VS Code Configuraton

- In the top level vue-outline directory:

```bash
cp -R scripts/vscode .vscode
```

## Runing the App

First start Drupal by going to the Drupal directory (../vue-outline/packages/drupal) and running:
```bash
cd <Drupal directory>
lando start
```

Check Drupal status:
```bash
lando drush status
```

Run the Vue app:
```bash
cd <top level directory>
npm run serve
```

## Rebuild or Reinitialize Lando / Reinstall Drupal

If Lando/Drupal are not working / broken in some way try a Lando rebuild:

```bash
lando rebuild
lando site-setup
```

To reinitialize Lando and reinstall Drupal:

```bash
lando destroy
lando start
lando site-setup
```

## Packages

- api
  - Simple express api server.
  - Can use puppeteer.
- drupal
  - Main dev/drupal environment
  - Only generated when following the Initialize Drupal development server instructions above
- outline
  - Main vue-outline client/application
  - Requires drupal server to function

## General repository commands

### root level

- `build <package>`: runs build on all packages. Accepts `<package>` parameter to target specific packages
- `clean`: Cleans node_modules from all packages
- `init:drupal`: Initializes Drupal development server
- `lint`: Runs lint on all packages
- `lint:fix`: Runs `lint --fix` on all packages
- `serve <package>`: Runs `serve` on all packages. Accepts `<package>` parameter to target specific packages
- `start`: Starts drupal development server. Requires lando to function

### api package

- `serve`: Starts express api server

### outline package

- `build`: Compiles and minifies outline client for production
- `lint`: Lints and fixes outline client files
- `serve`: Compiles and hot-reloads outline client for development

## Roadmap

### Planned Features
Pupeteer
Dynamic Loading of Children

### Long Term
Undo/Redo
Forward/Back
Taxonomy Editing
Admin UI functionality
Migrate module support for migrating Books to Outline
