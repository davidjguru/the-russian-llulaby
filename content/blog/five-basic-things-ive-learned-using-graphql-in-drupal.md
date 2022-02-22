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
You've probably heard of "decoupled Drupal" or "headless", a way of working with Drupal that consists of separating the frontend from the backend, implementing communication between both parts as separate projects, distinct repositories, etc. Usually, this way of working necessarily involves "breaking the monolith" i.e. splitting into several parts of different technology, leaving Drupal only for the backend side of the project. In this context, you will use some kind of specification or API connection to make backend and frontend work well and understand each other. You can use REST, JSON:API or GraphQL as well. This article is oriented to this last opinion, the possibility of using GraphQL in Drupal, and is edited and published as the first post of a series about GraphQL. As an introduction, I would like to share some initial thoughts based only on my observations. 

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
[6- Read More](#6--read-more)  
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
In fact, I'm trying to install the last available version for [drupal/graphql, 4.3.0 (released on 15/02/2022)](https://www.drupal.org/project/graphql/releases/8.x-4.3) in Drupal 9.3.6 and I'm facing the same problems related with the stability of the drupal/typed_data version:  

![Getting errors in drupal/typed_data](../../images/post/davidjguru_drupal_8_9_graphql_introduction_0.png)

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

![Align naming in files](../../images/post/davidjguru_drupal_8_9_graphql_introduction_1.png)


## 2- Respect the order of parameters


![GraphQL example of parameters in queries](../../images/post/davidjguru_drupal_8_9_graphql_introduction_2.png)



## 3-Enable debugging mode  

![GraphQL example of parameters in queries](../../images/post/davidjguru_drupal_8_9_graphql_introduction_2.png)

## 4- Maintain your code clean and do refactoring  


![GraphQL example of parameters in queries](../../images/post/davidjguru_drupal_8_9_graphql_introduction_2.png)


## 5- GraphQL may not be your best option  

1. **Documentation is scarce and may not be very up to date.** You have few resources available to learn. You can go to [www.drupal.org/docs/graphql](https://www.drupal.org/docs/contributed-modules/graphql) and then you will see that the information may be [very little](https://www.drupal.org/docs/8/modules/graphql/fetching-data), [outdated](https://www.drupal.org/docs/8/modules/graphql/query-maps) or [non-existent](https://www.drupal.org/docs/contributed-modules/graphql/graphql-twig). This is not very motivating and you will have to build a curated reading list on your own. Of course, the Amazee Labs content list related to GraphQL is fundamental. It is arguably the company that has written the most about the Drupal & GraphQL partnership. You have the links in the last section, [Read More](#6--read-more).  

## 6- Read More


* [Amazee Labs: GraphQL Introduction](https://www.amazeelabs.com/en/blog/graphql-introduction)  
* [Amazee Labs: Drupal and GraphQL with React and Apollo](https://www.amazeelabs.com/en/blog/drupal-graphql-react-apollo)  
* [Amazee Labs: Drupal and GraphQL - Batteries included](https://www.amazeelabs.com/en/blog/drupal-graphql-batteries-included)  
* [Amazee Labs: Extending GraphQL Part 1 - Fields](https://www.amazeelabs.com/en/blog/extending-graphql-part1-fields)  
* [Amazee Labs: Extending GraphQL Part 2 - Types and Interfaces](https://www.amazeelabs.com/en/blog/extending-graphql-part-2)  
* [Amazee Labs: Extending GraphQL Part 3 - Mutations](https://www.amazeelabs.com/en/journal/extending-graphql-part-3-mutations)  
* [Amazee Labs: GraphQL for Drupalers Part 1 - The Basics](https://www.amazeelabs.com/en/journal/graphql-drupalers-part-1-basics)  
* [Amazee Labs: GraphQL for Drupalers Part 2 - The Queries](https://www.amazeelabs.com/en/journal/graphql-drupalers-part-2-queries)  
* [Amazee Labs: GraphQL for Drupalers Part 3 - The Fields](https://www.amazeelabs.com/en/journal/graphql-drupalers-part-3-fields)  
* [Amazee Labs: GraphQL for Drupalers Part 4 - Fetching the entities](https://www.amazeelabs.com/en/journal/graphql-drupalers-part-4-fetching-entities)  
* [Amazee Labs: Don't push it - Using GraphQL in Twig](https://www.amazeelabs.com/en/blog/dont-push-it-using-graphql-twig)  
  
**Others**

* [Valuebound: GraphQL a Beginners Guide (2018)](https://www.valuebound.com/resources/blog/graphql-beginners-guide)  
* [Specbee: GraphQL with Drupal 8: All you need to know (2019)](https://www.specbee.com/blogs/graphQL-with-drupal-8-what-is-graphql-Advantages-need-to-know-Guide)  
* [OpenSense Labs: Why is GraphQL an Importante Player in Decoupled Drupal? (2020)](https://opensenselabs.com/blog/articles/graphql-important-player-decoupled-drupal)  
* [Mediacurrent: A Recipe for a Graphql Server in Drupal Using graphql-php (2020)](https://www.mediacurrent.com/blog/recipe-graphql-server-drupal-using-graphql-php)  

## :wq!

If you have managed to reach the end of this guide linearly, congratulations! Thanks for your patience and I really hope it has been useful to you.

This guide has been published without -direct- profit, but my personal interest is that it spreads and helps my communication. If it has been useful to you, share it using the "share" of this site, putting a simple tweet. It will be important for me. Thank you.

##### Recommended song: The Troublemakers - Get Misunderstood

{{< youtube vCMGyiDF2zI >}}
