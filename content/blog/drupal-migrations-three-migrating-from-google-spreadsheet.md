---
title: "Drupal Migrations (III): Migrating from Google Spreadsheet"
date: 2020-05-04
draft: true

# post thumb
image: "images/post/davidjguru_drupal_migrations_debugging_main.jpg"

# meta description
description: "Drupal Migrations (III): Migrating from Google Spreadsheet"

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

**Picture from Unsplash, user [Pan Xiaozhen, @zhenhappy](https://unsplash.com/@zhenhappy)**

  
---------------------------------------------------------------------------------
**Table of Contents**

<!-- TOC -->
[1- Introduction](#1--introduction)  
[2- Basic Resources - Core Modules](#2--basic-resources---core-modules)  
[3- Other Basic Resources - Contrib Modules](#3--other-basic-resources---contrib-modules)  
[4- Extra Resources - Contrib Modules for Plugins](#4--extra-resources---contrib-modules-for-plugins)  
[5- Migration Runners - Contrib Modules Drush-Related](#5--migrations-runners---contrib-modules-drush-related)  
[6- Authors you should know](#6--authors-you-should-know)  
[7- :wq!](#7--wq)
<!-- /TOC -->

-----------------------------------------------------------------------------------------

**This article is part of a series of posts about Drupal Migrations**

<!-- TOC -->
[1- Drupal Migrations (I): Basic Resources](https://www.therussianlullaby.com/blog/drupal-migrations-one-basic-resources/)

[2- Drupal Migrations (II): Examples](https://www.therussianlullaby.com/blog/drupal-migrations-two-examples/)

<!-- /TOC -->

---------------------------------------------------------------------------------

## 1- Introduction (and remember ETL processes)
Well, I would like to start by posing a scenario: imagine that we must migrate a list of taxonomy terms to our Drupal Installation. The source of the data is important, since it defines what type of Plugins we will have to use (source Plugins for the extract, but it can also have influence for the processing and for the final load in destination).

The imagined scenario of this article will be the following: we have "inherited" a migration task already initiated by a previous partner. This migration consists of populating an existing vocabulary in our Drupal installation with taxonomy terms.  
To practice with other Plugins and other migration models, I thought we can take a Google spreadsheet as a source of data. Let's not forget the frame we're in: 

![ETL Scheme and Drupal Migrate API overview](../../images/post/davidjguru_drupal_migrations_ETL_scheme.png)  

## 2- Exposing data through Google Spreadsheet

So now, to perform source data extraction, we'll need a spreadsheet with exposed values. I've created a Google spreadsheet [here at this address](https://docs.google.com/spreadsheets/d/1bKGbPbgeuXaBfcKetaDqoDimmYcerQY_hT1rqzw4TbM/edit?usp=sharing). This Google Spreadsheet contains some columns with values related with fields of a taxonomy term, ready for migration.  

In order to processing the data source we'll use the [Migrate Google Sheets contrib module from Drupal.org](https://www.drupal.org/project/migrate_google_sheets), so you can run your Composer to download the resource. Just launch:  
```
composer require drupal/migrate_google_sheets
drush en migrate_google_sheet
```
This contrib module will treat the resource as a JSON file, though for that we have to do some tasks previously. For example, we have to expose the Google Spreadsheet like a JSON datasource, using the tools provided by Google.  

* First, we need extracting the **workbook-id** of our Spreadsheet: docs.google.com/spreadsheets/d/1bKGbPbgeuXaBfcKetaDqoDimmYcerQY_hT1rqzw4TbM/ -> workbook-id: 1bKGbPbgeuXaBfcKetaDqoDimmYcerQY_hT1rqzw4TbM.  

* Second, we need the worksheet-index too. This is only the index of the tab with data from the Spreadsheet. In this case -> worksheet-index: 1.  

* Third, Building the JSON exposed URL using the pattern: spreadsheets.google.com/feeds/list/[workbook-id]/[worksheet-index]/public/values?alt=json, for us: [http://spreadsheets.google.com/feeds/list/1bKGbPbgeuXaBfcKetaDqoDimmYcerQY_hT1rqzw4TbM/1/public/values?alt=json](http://spreadsheets.google.com/feeds/list/1bKGbPbgeuXaBfcKetaDqoDimmYcerQY_hT1rqzw4TbM/1/public/values?alt=json ).  

**That's all!** now we got an exposed Google Spreadsheet as a JSON file and now we can get the values from the selected Source Plugins of the Drupal Migrate API.  

![Google Spreadsheet JSON dataformat](../../images/post/davidjguru_drupal_migrations_Google_Spreadsheet_source.png)  

## 3- Special Properties from the JSON transformation

Now we're seeing our Json datasource from our browser (maybe better you use some kind of extension for your browser in order to get a well-formed view of the Json data, like Firefox is doing by default):

```
      {
        "gsx$id": {
          "$t": "1"
        },
        "gsx$name": {
          "$t": "Term 1"
        },
        "gsx$description": {
          "$t": "Descrip 1"
        },
        "gsx$url": {
          "$t": "/t1"
        }
      }
```
As we can see, some items has been changed from the original format. Look there: 

![Comparing data formats](../../images/post/davidjguru_drupal_migration_google_spreadsheet_json.png)  

An important observation about this transformation is that the move to JSON format includes some changes over the original spreadsheet. For example, the field identification is modified by Google by adding certain prefixes to the name: 

* gsx$ for field names.
* $t for values stored in fields.

And the headers has been changed: 

1. 'Id' is now 'gsx$id'
1. 'Name' is now 'gsx$name'
1. 'Description' is now 'gsx$description'
1. 'Url' is now 'gsx$url'
1. 'Published' is now 'gsx$published'

But since these changes are stable, the Plugin takes care of their processing: 

```
// Class GoogleSheets.php
// Module migrate_google_sheets
// @see https://git.drupalcode.org/project/migrate_google_sheets/

// Actual values are stored in gsx$<field>['$t'].
$this->currentItem[$field_name] = $current['gsx$' . $selector]['$t'];
```

This allows us use simply the names of the data in their exposed 'flat' transformations: all will be in lowercase and with spaces or special characters removed. In addition, the values are stored in an XPath feed/entry path, which the module will also take over:  

```
// For Google Sheets, the actual row data lives under feed->entry.
if (isset($array['feed']) && isset($array['feed']['entry'])) {
 $array = $array['feed']['entry'];
}
```

## 4- Characteristics of the Migration
The first observation we will make is that the Google Spreadsheet Plugin is actually an extension of the Json Processing Plugin, as we can see in the class: 

```
use Drupal\Core\Plugin\ContainerFactoryPluginInterface;
use Drupal\migrate\MigrateException;
use Drupal\migrate_plus\Plugin\migrate_plus\data_parser\Json;
use GuzzleHttp\Exception\RequestException;

/**
 * Obtain Google Sheet data for migration.
 *
 * @DataParser(
 *   id = "google_sheets",
 *   title = @Translation("Google Sheets")
 * )
 */
class GoogleSheets extends Json implements ContainerFactoryPluginInterface {
...
}
```



## 5- Custom Module for Migration 



# 6- :wq! 

##### Recommended song

{{< youtube je0lFe0MHjU >}}
