---
title: "Five basic things I've learned using GraphQL in Drupal"
date: 2021-02-14
draft: true

# post thumb
image: "images/post/davidjguru_drupal_8_9_graphql_introduction_main.png"

# meta description
description: "Introduction article with some useful tips about working with GraphQL and Drupal."

# taxonomies
categories:
  - "Development"
tags:
  - "GraphQL"
  - "Drupal Development"
  - "Backend"
  - "PHP"

# post type
type: "post"
---
You've probably heard of "decoupled Drupal" or "headless", a way of working with Drupal that consists of separating the frontend from the backend, implementing communication between both parts as separate projects, distinct repositories, etc. Usually, this way of working necessarily involves "breaking the monolith" i.e. splitting into several parts of different technology, leaving Drupal only for the backend side of the project. 

--------------------------------------------------------------------------------------
**Picture from Unsplash, user [Masjid Pogung Dalangan, @@masjidmpd](https://unsplash.com/@masjidmpd).**


---------------------------------------------------------------------------------

**Index of sections**
<!-- TOC -->
[Introduction](#introduction)  
[Fast Rewind](#fast-rewind)  
[1- Align file naming and resources](#1--align-file-naming-and-resources)  
[2- Respect the order of parameters](#2--respect-the-order-of-parameters)  
[3- Enable debugging mode](#3-enable-debugging-mode)  
[4- Maintain your code clean and do refactoring](#4--maintain-your-code-clean-and-do-refactoring)  
[5- GraphQL may not be your best option](#5--graphql-may-not-be-your-best-option)  
[:wq!](#wq)  

---------------------------------------------------------------------------------


## Introduction

I have been working on openly decoupled Drupal projects for some time now, 


**There we go!**

## Fast Rewind 

### Installing GraphQL in Drupal 

In former versions of Drupal (Drupal 8 basically) you could execute:  
```bash 
$ composer require drupal/graphql
```
And everything goes well, but from Drupal 9 you will get (by now): 


```bash
$ composer require drupal/graphql

Your requirements could not be resolved to an installable set of packages.

  Problem 1
    - Root composer.json requires drupal/graphql ^4.2 -> satisfiable by drupal/graphql[4.2.0].
    - drupal/graphql 4.2.0 requires drupal/typed_data * -> found drupal/typed_data[dev-1.x, 1.0.0-alpha1, ..., 1.x-dev (alias of dev-1.x)] but it does not match your minimum-stability.
```

So first you have to install the required dependency in alpha versions and then GraphQL. I wrote some notes about this [here in my guide for Drupal upgrading, troubleshooting section](https://davidjguru.github.io/blog/drupal-techniques-how-to-upgrade-drupal#6--troubleshooting).   

Just:  
```bash
$ composer require drupal/typed_data:@alpha
[...]
$ composer require drupal/graphql
```
And now everything goes well. When you finish, you have to enable the new module by drush:  

```bash
$ drush en -y graphql
```
For learning purposes, please enable the secondary modules (with available examples) too:  
```bash
$ drush en -y graphql graphql_composable graphql_examples
```

After login, you can navigate to `/admin/config/graphql` and create a new server. You can use the "Example schema" that comes with the `graphql_examples` module (as we can see in the former step the examples comes with the graphql module but needs to be enabled separately).   

After creating the server click on dropdown button at right, click in "Explorer" option and this should bring you to [the GraphiQL explorer](https://github.com/graphql/graphiql/tree/main/packages/graphiql#readme), or you can go directly to `/admin/config/graphql/servers/manage/initial_example/explorer` and there you will see the visual explorer GraphiQL for visualizing queries within your Drupal installation. You can use some plugins for IDEs too, like [the graphiql-explorer for VSCode](https://marketplace.visualstudio.com/items?itemName=GabrielNordeborn.vscode-graphiql-explorer). 



## 1- Align file naming and resources

![GraphQL example of parameters in queries](../../images/post/davidjguru_drupal_8_9_graphql_introduction_2.png)


## 2- Respect the order of parameters


![GraphQL example of parameters in queries](../../images/post/davidjguru_drupal_8_9_graphql_introduction_2.png)



## 3-Enable debugging mode  

![GraphQL example of parameters in queries](../../images/post/davidjguru_drupal_8_9_graphql_introduction_2.png)

## 4- Maintain your code clean and do refactoring  


![GraphQL example of parameters in queries](../../images/post/davidjguru_drupal_8_9_graphql_introduction_2.png)


## 5- GraphQL may not be your best option  


## :wq!

If you have managed to reach the end of this guide linearly, congratulations! Thanks for your patience and I really hope it has been useful to you.

This guide has been published without -direct- profit, but my personal interest is that it spreads and helps my communication. If it has been useful to you, share it using the "share" of this site, putting a simple tweet. It will be important for me. Thank you.

##### Recommended song: The Troublemakers - Get Misunderstood

{{< youtube vCMGyiDF2zI >}}
