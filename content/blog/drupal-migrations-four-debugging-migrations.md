---
title: "Drupal Migrations (IV): Debugging Migrations"
date: 2020-06-29
draft: true

# post thumb
image: "images/post/davidjguru_drupal_migrations_debugging_migrations_main.jpg"

# meta description
description: "Drupal Migrations (IV): Debugging Migrations"

# taxonomies
categories: 
  - "Migrations"  
tags:
  - "Drupal Migrations"
  - "Migrate API"
  - "Contrib Modules"
  - "Drupal Plugins"
  - "ETL"

# post type
type: "post"
---

The Drupal migrations, despite their linearity in terms of definitions, contain a lot of inherited complexity. The reason is very intuitive: although the Migrate API is a supersystem that offers a very simple "interface" of interactions for the user-developer who wants to build migration processes, in reality several subsystems work by interacting with each other throughout a migration process: Entities, Database, Plugins...
 
--------------------------------------------------------------------------------------

**Picture from Unsplash, user [Krzysztof Niewolny, @epan5](https://unsplash.com/@epan5)**

  
---------------------------------------------------------------------------------
**Table of Contents**

<!-- TOC -->
[1- Introduction and remember ETL processes](#1--introduction-and-remember-etl-processes)  
[2- Exposing data through Google Spreadsheet](#2--exposing-data-through-google-spreadsheet)  
[3- Special Properties from the JSON transformation](#3--special-properties-from-the-json-transformation)  
[4- Characteristics of the Migration](#4--characteristics-of-the-migration)  
[5- Custom Module for Migration](#5--custom-module-for-migration)  
[6- :wq!](#6--wq)  
<!-- /TOC -->
-----------------------------------------------------------------------------------------

**This article is part of a series of posts about Drupal Migrations:**

<!-- TOC -->
[1- Drupal Migrations (I): Basic Resources](https://www.therussianlullaby.com/blog/drupal-migrations-one-basic-resources/)  

[2- Drupal Migrations (II): Examples](https://www.therussianlullaby.com/blog/drupal-migrations-two-examples/)  

[3- Drupal Migrations (III): Migrating from Google Spreadsheet](https://www.therussianlullaby.com/blog/drupal-migrations-three-migrating-from-google-spreadsheet/)  
<!-- /TOC -->

---------------------------------------------------------------------------------

## 1- Introduction
In the wake of the latest articles, I wanted to continue expanding information on migration in Drupal