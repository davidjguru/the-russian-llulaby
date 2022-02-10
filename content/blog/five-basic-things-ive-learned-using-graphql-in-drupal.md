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

In this guide you will learn basic concepts of JavaScript, the terminology used in Drupal, functions, methods and common mechanics to enrich your projects by make them run with executable code on the client side. And all through a combination of theory and practice. It includes some exercises that I have integrated.

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

Some time ago (around December 2019, but it seems a century has passed ) I started writing what I thought would be a simple guide to integration between JavaScript and Drupal. A couple of months later, in February 2020, I had a tutorial of more than eleven thousand words written in Castillian (Spanish from Spain) that I published in my Medium profile.

What was initially going to be brief has become a kind of reference guide on JavaScript and Drupal and (as far as I know) is now part of the training resources shared in many companies in Spain and other Latin American countries. Here you can reach the original publication in Medium, the so called: [JavaScript & Drupal 101 TUTORIAL HANDBOOK TOTAL MAX POWER 2000](https://medium.com/@davidjguru/javascript-drupal-101-tutorial-handbook-total-max-power-2000-a137326fea6a) (I can swear I had a lot of fun thinking about the title).

Well, the fact is that since the publication, I received three basic types of feedback:

  * "Hey, this is wrong, you have to check it"
  * "We have people in the company from other countries, do you have it translated into English?"
  * "Thank you for not putting it behind the Medium payment wall"

So although my first intention was to move all this content to an open book format like git-book or something like that, I've actually grouped the first two together and I'm going to publish a review of the original post translated into English. As always, I hope it can be useful to someone.

In a complementary way, you can download all the code from the exercises grouped as a single Drupal custom module, available here: [gitlab.com/davidjguru/javascript_custom_module](https://gitlab.com/davidjguru/javascript_custom_module). This works in Drupal 8 and Drupal 9.

__DISCLAIMER:__ This guide is actually a manual for the integration of JavaScript code in Drupal-based projects, but only in the context of implementing Drupal modules. This is basically a backend issue. This guide does not contain information related to JavaScript frameworks (React, Angular, Vue) or about the use of Drupal headless as decoupled. Neither does it deal with Drupal Theming issues and its approach to them is only tangential. This tutorial is only for people related to the Drupal backend.


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
