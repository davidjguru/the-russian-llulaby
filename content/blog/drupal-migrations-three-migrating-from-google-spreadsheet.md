---
title: "Drupal Migrations (III): Migrating from Google Spreadsheet"
date: 2020-05-27
draft: false

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
[1- Introduction and remember ETL processes](#1--introduction-and-remember-etl-processes)  
[2- Exposing data through Google Spreadsheet](#2--exposing-data-through-google-spreadsheet)  
[3- Special Properties from the JSON transformation](#3--special-properties-from-the-json-transformation)  
[4- Characteristics of the Migration](#4--characteristics-of-the-migration)  
[5- Custom Module for Migration](#5--custom-module-for-migration)  
[6- :wq!](#6--wq)  
<!-- /TOC -->

-----------------------------------------------------------------------------------------

**This article is part of a series of posts about Drupal Migrations**

<!-- TOC -->
[1- Drupal Migrations (I): Basic Resources](https://www.therussianlullaby.com/blog/drupal-migrations-one-basic-resources/)  

[2- Drupal Migrations (II): Examples](https://www.therussianlullaby.com/blog/drupal-migrations-two-examples/)  

[3- Drupal Migrations (III): Migrating from Google Spreadsheet](https://www.therussianlullaby.com/blog/drupal-migrations-three-migrating-from-google-spreadsheet/)  

[4- Drupal Migrations (IV): Debugging Migrations First Part](https://www.therussianlullaby.com/blog/drupal-migrations-four-debugging-migrations-i/)  

<!-- /TOC -->

---------------------------------------------------------------------------------

## 1- Introduction (and remember ETL processes)
Well, I would like to start by posing a scenario: imagine that we must migrate a list of taxonomy terms to our Drupal Installation. The source of the data is important, since it defines what type of Plugins we will have to use (source Plugins for the extract, but it can also have influence for the processing and for the final load in destination).

The imagined scenario of this article will be the following: we have "inherited" a migration task already initiated by a previous partner. This migration consists of populating an existing vocabulary in our Drupal installation with taxonomy terms.  
To practice with other Plugins and other migration models, I thought we can take a Google spreadsheet as a source of data. Let's not forget the frame we're in: 

![ETL Scheme and Drupal Migrate API overview](../../images/post/davidjguru_drupal_migrations_ETL_scheme.png)  

Today our goal will be to fill the fields of a taxonomy term using the data contained in a external Google Spreadsheet. So we have to complete these fields: 

![Taxonomy term basic fields](../../images/post/davidjguru_drupal_migration_taxonomy_term_fields.png)  

And we're going to do this describing a Migrate Process and executing it as Configuration. Do you know the differences between migrations as code and as configuration? You can learn some keys about this topic [here, in the previous article about Migrations](https://www.therussianlullaby.com/blog/drupal-migrations-two-examples/#migration-as-code-or-as-configuration).

## 2- Exposing data through Google Spreadsheet

So now, to perform source data extraction, we'll need a spreadsheet with exposed values. I've created a Google spreadsheet [here at this address](https://docs.google.com/spreadsheets/d/1bKGbPbgeuXaBfcKetaDqoDimmYcerQY_hT1rqzw4TbM/edit?usp=sharing). This Google Spreadsheet contains some columns with values related with fields of a taxonomy term, ready for migration.  

In order to processing the data source we'll use the [Migrate Google Sheets contrib module from Drupal.org](https://www.drupal.org/project/migrate_google_sheets), so you can run your Composer to download the resource. Just launch:  
```
composer require drupal/migrate_plus #If applicable
composer require drupal/migrate_google_sheets
drush en migrate_plus migrate_google_sheet -y
```
This contrib module will treat the resource as a JSON file, though for that we have to do some tasks previously. For example, we have to expose the Google Spreadsheet like a JSON datasource, using the tools provided by Google.  

* First, we need extracting the **workbook-id** of our Spreadsheet: docs.google.com/spreadsheets/d/1bKGbPbgeuXaBfcKetaDqoDimmYcerQY_hT1rqzw4TbM/ **-> workbook-id: 1bKGbPbgeuXaBfcKetaDqoDimmYcerQY_hT1rqzw4TbM**.  

* Second, we need the **worksheet-index** too. This is only the index of the tab with data from the Spreadsheet. In this case **-> worksheet-index: 1**.  

* Third, Building the JSON exposed URL using the pattern: **spreadsheets.google.com/feeds/list/[workbook-id]/[worksheet-index]/public/values?alt=json**, for us: [http://spreadsheets.google.com/feeds/list/1bKGbPbgeuXaBfcKetaDqoDimmYcerQY_hT1rqzw4TbM/1/public/values?alt=json](http://spreadsheets.google.com/feeds/list/1bKGbPbgeuXaBfcKetaDqoDimmYcerQY_hT1rqzw4TbM/1/public/values?alt=json ).  

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
        "gsx$published": {
          "$t": "1"
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

Also we can see that the GoogleSheet Plugins is marked as a Data Parser in its block annotations, sowe'll need some more resources: a base source Plugin and a Data Fetcher (then the Google Spreadsheet Plugin will be the third part in the process).

Ok, What Source Plugins are available in my Drupal installation? Let's see. Launching:

```
drupal debug:plugin migrate.source
```

We'll get by prompt:
```
drupal@migrations-web:/var/www/html$ drupal debug:plugin migrate.source  
 --------------- --------------------------------------------------------- 
  Plugin ID       Plugin class                                             
 --------------- --------------------------------------------------------- 
  embedded_data   Drupal\migrate\Plugin\migrate\source\EmbeddedDataSource  
  empty           Drupal\migrate\Plugin\migrate\source\EmptySource         
  url             Drupal\migrate_plus\Plugin\migrate\source\Url            
 --------------- --------------------------------------------------------- 

drupal@migrations-web:/var/www/html$ 
```

Ok, as a Source Plugin we can use the class Url.php (our file is exposed by URL). In the [Url.php class](https://git.drupalcode.org/project/migrate_plus/-/blob/8.x-5.x/src/Plugin/migrate/source/Url.php), we see that we need some king of data parser (The Google Spread Sheet class).
```
  /**
   * The data parser plugin.
   *
   * @var \Drupal\migrate_plus\DataParserPluginInterface
   */
  protected $dataParserPlugin;
```

And looking for a fetcher / handler, we can find out a data fetcher for http processing, [the class Http.php](https://git.drupalcode.org/project/migrate_plus/-/blob/8.x-5.x/src/Plugin/migrate_plus/data_fetcher/Http.php) ready to work with a Url Plugin as source: 
```
/**
 * Retrieve data over an HTTP connection for migration.
 *
 * Example:
 *
 * @code
 * source:
 *   plugin: url
 *   data_fetcher_plugin: http
 *   headers:
 *     Accept: application/json
 *     User-Agent: Internet Explorer 6
 *     Authorization-Key: secret
 *     Arbitrary-Header: foobarbaz
 * @endcode
 *
 * @DataFetcher(
 *   id = "http",
 *   title = @Translation("HTTP")
 * )
 */
class Http extends DataFetcherPluginBase implements ContainerFactoryPluginInterface {
```
From another side, seems that the Url.php class requires url directions from its constructor method. We can use single URL directions or a set:  

```
  /**
   * {@inheritdoc}
   */
  public function __construct(array $configuration, $plugin_id, $plugin_definition, MigrationInterface $migration) {
    if (!is_array($configuration['urls'])) {
      $configuration['urls'] = [$configuration['urls']];
    }
    parent::__construct($configuration, $plugin_id, $plugin_definition, $migration);

    $this->sourceUrls = $configuration['urls'];
  }
```
So by putting together the different parts we're looking at, it looks like we'll have to give a structured form to the elements, according to their order and position. In short, our first section for the Source would look like this: 

```
source:
  plugin: url
  data_fetcher_plugin: http
  data_parser_plugin: google_sheets
  urls: 'http://spreadsheets.google.com/feeds/list/1bKGbPbgeuXaBfcKetaDqoDimmYcerQY_hT1rqzw4TbM/1/public/values?alt=json'
```

## 5- Custom Module for Migration 
We'll create a new custom module called ```migration_google_sheet``` and with structure:

```
/project/web/modules/custom/  
                     \__migration_google_sheet/  
                         \__migration_google_sheet.info.yml
                           \__config/
                                \__install/
                                     \__migrate_plus.migration.migration.taxonomy_google_sheet.yml
```

Migration Description File: ```migrate_plus.migration.taxonomy_google_sheet.yml```

```
langcode: en
status: true
dependencies:
  enforced:
    module:
      - migration_google_sheet
id: taxonomy_google_sheet
label: 'Migrating Taxonomy'
source:
  plugin: url
  data_fetcher_plugin: http
  data_parser_plugin: google_sheets
  urls: 'http://spreadsheets.google.com/feeds/list/1bKGbPbgeuXaBfcKetaDqoDimmYcerQY_hT1rqzw4TbM/1/public/values?alt=json'
  fields:
    - name: id
      label: 'Id'
      selector: id
    - name: name
      label: 'Name'
      selector: name
    - name: description
      label: 'Description'
      selector: description
    - name: url
      label: 'Url'
      selector: url
    - name: published
      label: 'Published'
      selector: published
  ids:
    id:
      type: integer
process:
  name: name
  description: description
  path: url
  status: published
destination:
  plugin: 'entity:taxonomy_term'
  default_bundle: tags
 ```

So now, executing the migration from prompt: 

```
drush en migration_google_sheet -y 
drush migrate:import taxonomy_google_sheet

[notice] Processed 30 items (30 created, 0 updated, 0 failed, 0 ignored) - done with 'taxonomy_google_sheet'
```
Well done! We just migrated thirty taxonomy terms from a Google spreadsheet:

![Taxonomy Terms Just migrated](../../images/post/davidjguru_drupal_migration_migrating_taxonomy_terms.png)  

You can download or clone the custom Migration module [from my gitlab repository](https://gitlab.com/davidjguru/drupal-custom-modules-examples/-/tree/master/Migrations). 

## 6- :wq! 

##### Recommended song

{{< youtube je0lFe0MHjU >}}
