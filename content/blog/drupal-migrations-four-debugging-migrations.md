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

The systems and subsystems related to Drupal's migration API are certainly exciting. In the previous articles in this series, I wanted to draw as complete a map as possible (part one) of the vast amount of resources, possibilities and referenced experts. In the second part I wanted to expose some basic mechanics of the migration processes in Drupal and knowing that this opens the door to thousands of options, possibilities and techniques....I didn't want to let a third article go by without sharing some experiences migrating data from a common format as a Google Spreadsheet, just an usual way in wich sometimes the data are sent. 
 
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