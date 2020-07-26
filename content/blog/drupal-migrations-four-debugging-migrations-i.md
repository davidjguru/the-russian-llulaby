---
title: "Drupal Migrations (IV): Debugging Migrations-I"
date: 2020-06-29
draft: false

# post thumb
image: "images/post/davidjguru_drupal_migrations_debugging_migrations_main.jpg"

# meta description
description: "Drupal Migrations (IV): Debugging Migrations-I"

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

The Drupal migrations, despite their linearity in terms of definitions, contain a lot of inherited complexity. The reason is very intuitive: although the Migrate API is a supersystem that offers a very simple "interface" of interactions for the user-developer who wants to build migration processes, in reality several subsystems work by interacting with each other throughout a migration process: Entities, Database, Plugins...There are a lot of classes involved in even the simplest migration process. If we add the irrefutable fact that a migration will tend to generate errors in many cases until it has been refined, it's clear then that one of our first needs will be to learn...how to debug migrations.
 
--------------------------------------------------------------------------------------

**Picture from Unsplash, user [Krzysztof Niewolny, @epan5](https://unsplash.com/@epan5)**

  
---------------------------------------------------------------------------------
**Table of Contents**

<!-- TOC -->
[1- Introduction](#1--introduction)  
[2- Basic Debugging: Keep an eye on your file](#2--basic-debugging-keep-an-eye-on-your-files)  
[3- Average Debugging with Migrate Devel](#3--average-debugging-with-migrate-devel)  
[4- :wq!](#4--wq)
<!-- /TOC -->
-----------------------------------------------------------------------------------------

**This article is part of a series of posts about Drupal Migrations:**

<!-- TOC -->
[1- Drupal Migrations (I): Basic Resources](https://www.therussianlullaby.com/blog/drupal-migrations-one-basic-resources/)  
* [Core Modules for Migrations in Drupal](https://www.therussianlullaby.com/blog/drupal-migrations-one-basic-resources/#2--basic-resources---core-modules).  
* [Contributed  Modules for Migrations in Drupal](https://www.therussianlullaby.com/blog/drupal-migrations-one-basic-resources/#3--other-basic-resources---contrib-modules).  
* [Contributed Modules for Plugins in Migrations](https://www.therussianlullaby.com/blog/drupal-migrations-one-basic-resources/#4--extra-resources---contrib-modules-for-plugins).  
* [Contributed Modules Drush - Related](https://www.therussianlullaby.com/blog/drupal-migrations-one-basic-resources/#5--migrations-runners---contrib-modules-drush-related).
* [Authors you should now](https://www.therussianlullaby.com/blog/drupal-migrations-one-basic-resources/#6--authors-you-should-know).  

[2- Drupal Migrations (II): Examples](https://www.therussianlullaby.com/blog/drupal-migrations-two-examples/)  
* [Introduction: Migration as code or as configuration](https://www.therussianlullaby.com/blog/drupal-migrations-two-examples/#1--introduction).
* [Arrangements: Migrating embedded data and CSV file](https://www.therussianlullaby.com/blog/drupal-migrations-two-examples/#2--arrangements).
* [Approaches for the Migrations](https://www.therussianlullaby.com/blog/drupal-migrations-two-examples/#3--approaches).  
* [Executing Migrations](https://www.therussianlullaby.com/blog/drupal-migrations-two-examples/#4--migrations).  
* [Key Concepts for the Migrations](https://www.therussianlullaby.com/blog/drupal-migrations-two-examples/#5--key-concepts).  
* [Resources for the Migrations](https://www.therussianlullaby.com/blog/drupal-migrations-two-examples/#6--resources).  

[3- Drupal Migrations (III): Migrating from Google Spreadsheet](https://www.therussianlullaby.com/blog/drupal-migrations-three-migrating-from-google-spreadsheet/)  
* [Remember ETL processes](https://www.therussianlullaby.com/blog/drupal-migrations-three-migrating-from-google-spreadsheet/#1--introduction-and-remember-etl-processes).  
* [Exposing Data Through Google Spreadsheet](https://www.therussianlullaby.com/blog/drupal-migrations-three-migrating-from-google-spreadsheet/#2--exposing-data-through-google-spreadsheet).  
* [Special Properties from the JSON transformation](https://www.therussianlullaby.com/blog/drupal-migrations-three-migrating-from-google-spreadsheet/#3--special-properties-from-the-json-transformation).
* [Chararcteristics of the Migration](https://www.therussianlullaby.com/blog/drupal-migrations-three-migrating-from-google-spreadsheet/#4--characteristics-of-the-migration).  
* [Custom Module for Migration](https://www.therussianlullaby.com/blog/drupal-migrations-three-migrating-from-google-spreadsheet/#5--custom-module-for-migration).

[4- Drupal Migrations (IV): Debugging Migrations First Part](https://www.therussianlullaby.com/blog/drupal-migrations-four-debugging-migrations-i/)  
* [Basic Debugging of a Drupal Migration: files, database tables and configuration objects](https://www.therussianlullaby.com/blog/drupal-migrations-four-debugging-migrations-i/#2--basic-debugging-keep-an-eye-on-your-files).  
* [Average Debugging with Migrate Devel Module](https://www.therussianlullaby.com/blog/drupal-migrations-four-debugging-migrations-i/#3--average-debugging-with-migrate-devel).  

<!-- /TOC -->

---------------------------------------------------------------------------------

## 1- Introduction
In the wake of the latest articles, I wanted to continue expanding information about migration in Drupal. I was thinking about writing a sub-series of debugging migrations (inside the main series about Drupal Migrations), and I want to publish now the first part, just a set of basic steps in order to get all the available information from a migration process. All the examples in this post were taken of the [migration_google_sheet example, from my Gitlab account](https://gitlab.com/davidjguru/drupal-custom-modules-examples/-/tree/master/Migrations/migration_google_sheet). 


## 2- Basic Debugging (Keep an eye on your files)

First, we will start with a very primary approach to error detection during a migration. To begin with, it is essential to keep the focus on reducing the range of error possibilities as much as possible by approaching the migration in an iterative and incremental manner. In other words: we will go step by step and expand our migrated data. 

### 2.1- Reviewing your Migration description file
First of all we are going to comment on the most intuitive step of all we will take, since sometimes there are errors that occur at first sight and not because they are recurrent but end up being more obvious. 

The first steps in our process of debugging a migration will be a review of two fundamental issues that usually appear in many migrations. So before anything else, we'll do a quick review of: 

**Whitespaces:** Any extra whitespace may be causing us problems at the level of the migration description file: we review all lines of the file in a quick scan in order to detect extra whitespace.

**Errors in the indentation:** The migration description file has a format based on YAML, a language for data serialization based on a key scheme: a value where it is structured by parent - child levels and an indentation of two spaces to the right at each level down in the hierarchy. It is very frequent that some indentation is not the right one and this ends up producing an error in the processing of the file. As a measure, as in the previous case, we will review all the cases of indentations registered in the file. 

You can rely on a YAML syntax review service such as [www.yamllint.com](http://www.yamllint.com), but you will have to monitor the result as well. 

### 2.2- Reviewing registers in database
If you're in a basic Drupal installation (standard profile) we have seventy-three tables, after the activation of the basic modules related to migrations: migrate, migrate_plus, migrate_tools and in this case the custom *migration_google_sheet_wrong* the number of tables in the database is seventy-five. Two more tables have been generated: 

```toml
cache_migrate
cache_discovery_migration
```

But also, later, after executing the migration with ID taxonomy_google_sheet_wrong contained in our custom module, we see in the database that two new tables have been generated related to the executed migration:  

* **migrate_map_taxonomy_google_sheet**  
This table contains the information related to the movements of a row of data (migrations are operated 'row' to 'row'). Migrate API is in charge of storing in this table the source ID, the destination ID and a hash related to the 'row' in this data mapping table. Combinations between the source ID and the hash of the row operation then make it easier to track changes, progressively update information, and cross dependencies when performing a batch migration (see below for how they are articulated).  
 The lookup processes for migrations are supported by this data: for example, to load a taxonomy term you must first lookup its "parent" term to maintain the hierarchy of terms. 
If we go to our database and we do not see recorded results after launching a migration, no data was stored and the migration requires debugging.  

* **migrate_message_taxonomy_google_sheet**  
In this table, messages associated to the executed migration will be stored, structured in the same way as the previous table (based on the processing of a 'row' of the migration), each message with its own identifier and an association to the id_hash of the 'row' of origin of the data:  

![Drupal Migration columns from table Messages](../../images/post/davidjguru_drupal_migrations_debugging_two.png)  

This information can be obtained through Drush, since the content of this table is what is shown on the screen when we execute the instruction:  

```toml
drush migrate:messages id-migration
```

And this can be a useful way to start getting information about the errors that occurred during our migration.  

### 2.4- Reloading Configuration objects

Another issue we'll need to address while debugging our migration is how to make it easier to update the changes made to the configuration object created from the migration description file included in the config/install path.  

As we mentioned earlier, each time the module is installed a configuration object is generated that is available in our Drupal installation. In the middle of debugging, we'll need to modify the file and reload it to check that our changes have been executed. How can we make this easier? Let's take a look at some guidelines.  

On the one hand, we must associate the life cycle of our migration-type configuration object with the installation of our module. For it, as we noted in the section 2.3.2- Migration factors as configuration, we will declare as forced dependency our own custom module of the migration: 

```toml
dependencies:
   enforced:
      module:
          - migration_google_sheet
```

We can use both Drush and Drupal Console to perform specific imports of configuration files, taking advantage of the single import options of both tools: 

**Using Drupal Console**
```toml
drupal config:import:single --directory="/modules/custom/migration_google_sheet/config/install" --file="migrate_plus.migration.taxonomy_google_sheet.yml"

```

**Using Drush**
```toml
drush cim --partial --source=/folder/
```
Similarly, we can also remove active configuration objects using either Drush or Drupal Console: 

```toml
drush config-delete "migrate_plus.migration.taxonomy_google_sheet"
drupal config:delete active "migrate_plus.migration.taxonomy_google_sheet"
```
If we prefer to use the Drupal user interface, there are options such as the contributed Config Delete module [drupal.org/config_delete](https://www.drupal.org/project/config_delete) , which activates extra options to the internal configuration synchronization menu to allow the deletion of configuration items from our Drupal installation. It's enough to download it through Composer and enable it through Drush or Drupal Console:  

```
composer require drupal/config_delete
drush en config_delete -y
```

![Drupal Config Delete actions](../../images/post/davidjguru_drupal_migrations_debugging_eight.png) 


This way we can re-import configuration objects without colliding with existing versions in the database. If you choose to update and compare versions of your configuration, then maybe the Configuration Update Manager contributed module can be a good option [https://www.drupal.org/project/config_update](https://www.drupal.org/project/config_update).  


## 3- Average Debugging with Migrate Devel

Well, we have looked closely at the data as we saw in the previous section and yet our migration of taxonomy terms from a Google Spreadsheet seems not to work. 

We have to resort to intermediate techniques in order to obtain more information about the process. In this phase our allies will be some modules and plugins that can help us to better visualize the migration process.  

### 3.1- Migrate Devel 

Migrate Devel [https://www.drupal.org/project/migrate_devel](https://www.drupal.org/project/migrate_devel) is a contributed module that brings some extra functionality to the migration processes from new options for drush. This module works with *migrate_tools* and *migrate_run*.  

#####  **UPDATE (03/07/2020): Version 8.x-2.0-alpha2**  
Just as I published this article, [Andrew Macpherson](https://twitter.com/MartianWebDev) (new maintainer of the Migrate Devel module and one of the accessibility maintainers for Drupal Core), left a comment that you can see at the bottom of this post with some important news. Well, since I started the first draft of this article, a new version had been published, released on June 28th and it's already compatible with Drush 9 (and I didn't know...) So now you know there's a new version available to download compatible with Drush 9 and which avoids having to install the patch exposed below.

To install and enable the module, we proceed to download it through composer and activate it with drush: [Migrate Devel 8.x-2.0-alpha2](https://www.drupal.org/project/migrate_devel/releases/8.x-2.0-alpha2).  

```
composer require drupal/migrate_devel
# To install the 2.0 branch of the module:
composer require drupal/migrate_devel:^2.0
drush en migrate_devel -y
```


##### **Follow for versions prior to 8.x-2.0-alpha2:**
If you're working with versions prior to 8.x-2.0-alpha2, then you have to know some particularities: The first point is that it's was optimized for a previous version of Drush (8) and it does not seem to have closed its portability to Drush 9 and higher.  

There's a tag 8.x.1.4 from two weeks ago in the 8.x-1.x branch:  [migrate_devel/tree/8.x-1.4](https://git.drupalcode.org/project/migrate_devel/-/tree/8.x-1.4)

There is a necessary patch in its Issues section to be able to use it in versions of Drush > 9 and if we make use of this module this patch [https://www.drupal.org/node/2938677 ](https://www.drupal.org/node/2938677) will be almost mandatory. The patch does not seem to be in its final version either, but at least it allows a controlled and efficient execution of some features of the module. Here will see some of its contributions. 

And to apply the patch we can download it with wget and apply it with git apply:  

```
cd /web/modules/contrib/migrate_devel/
wget https://www.drupal.org/files/issues/2018-10-08/migrate_devel-drush9-2938677-6.patch 
git apply migrate_devel-drush9-2938677-6.patch
```
Or place it directly in the patch area of our composer.json file if we have the patch management extension enabled: [https://github.com/cweagans/composer-patches](https://github.com/cweagans/composer-patches).  

Using: 
```
composer require cweagans/composer-patches 
```
And place the new patch inside the "extra" section of our composer.json file:  

![Drupal Debugging adding the patch](../../images/post/davidjguru_drupal_migrations_debugging_three.png)  


##### **How it works:**

The launch of a migration process with the parameters provided by Migrate Devel will generate an output of values per console that we can easily check, for example using **--migrate-debug**:  

![Drupal Devel Output first part](../../images/post/davidjguru_drupal_migrations_debugging_four.png)  
![Drupal Devel Output second part](../../images/post/davidjguru_drupal_migrations_debugging_five.png)

This is a partial view of the processing of a single row of migrated data, showing the data source, the values associated with this row and the final destination ID, which is the identifier stored in the migration mapping table for process tracking:  

![Drupal Devel Output in database](../../images/post/davidjguru_drupal_migrations_debugging_six.png)

Now we can see in the record that for the value 1 in origin (first array of values), the identifier 117 was assigned for the load in destination. This identifier will also be the internal id of the new entity (in this case taxonomy term) created within Drupal as a result of the migration. This way you can relate the id of the migration with the new entity created and stored. 

**What about event subscribers?**, Migrate Devel creates an event subscriber, a class that implements EventSubscriberInterface and keeps listening to events generated from the event system of the Drupal's Migrate API, present in the migrate module of Drupal's core: 

```
Called from +56 
/var/www/html/web/modules/contrib/migrate_devel/src/EventSubscriber/MigrationEventSubscriber.php
```

The call is made from the class where events are heard and actions from the module's Event classes are read. Many events are defined there [modules/migrate/src/Event](https://git.drupalcode.org/project/drupal/-/tree/9.0.x/core/modules/migrate/src/Event ), but in particular, two that are listened to by Migrate Devel:  

1. [MigratePostRowSaveEvent.php](https://api.drupal.org/api/drupal/core%21modules%21migrate%21src%21Event%21MigratePostRowSaveEvent.php/class/MigratePostRowSaveEvent/8.2.x) 
2. [MigratePreRowSaveEvent.php](https://api.drupal.org/api/drupal/core%21modules%21migrate%21src%21Event%21MigratePreRowSaveEvent.php/8.6.x)

What are the two Drush options offered by Migrate Devel, and in both cases results in a call to the Kint library dump() function provided by the Devel module to print messages. In fact the call to Kint has changed in the last version 8.x-2.0-alpha2, where Kint is replaced by a series of calls to the Dump method of ths Symfony VarDumper. Where we used to do:  

```
  /**
   * Pre Row Save Function for --migrate-debug-pre.
   *
   * @param \Drupal\migrate\Event\MigratePreRowSaveEvent $event
   *    Pre-Row-Save Migrate Event.
   */
  public function debugRowPreSave(MigratePreRowSaveEvent $event) {
    $row = $event->getRow();

    $using_drush = function_exists('drush_get_option');
    if ($using_drush && drush_get_option('migrate-debug-pre')) {
      // Start with capital letter for variables since this is actually a label.
      $Source = $row->getSource();
      $Destination = $row->getDestination();

      // We use kint directly here since we want to support variable naming.
      kint_require();
      \Kint::dump($Source, $Destination);
    }
  }
```
Now we're doing: 

```
  /**
   * Pre Row Save Function for --migrate-debug-pre.
   *
   * @param \Drupal\migrate\Event\MigratePreRowSaveEvent $event
   *    Pre-Row-Save Migrate Event.
   */
  public function debugRowPreSave(MigratePreRowSaveEvent $event) {
    if (PHP_SAPI !== 'cli') {
      return;
    }

    $row = $event->getRow();

    if (in_array('migrate-debug-pre', \Drush\Drush::config()->get('runtime.options'))) {
      // Start with capital letter for variables since this is actually a label.
      $Source = $row->getSource();
      $Destination = $row->getDestination();

      // Uses Symfony VarDumper.
      // @todo Explore advanced usage of CLI dumper class for nicer output.
      // https://www.drupal.org/project/migrate_devel/issues/3151276
      dump(
        '---------------------------------------------------------------------',
        '|                             $Source                               |',
        '---------------------------------------------------------------------',
        $Source,
        '---------------------------------------------------------------------',
        '|                           $Destination                            |',
        '---------------------------------------------------------------------',
        $Destination
      );
    }
  }
```

You can see the update and changes in [migrate_devel/8.x-2.0-alpha2/src/EventSubscriber/MigrationEventSubscriber.php](https://git.drupalcode.org/project/migrate_devel/-/blob/8.x-2.0-alpha2/src/EventSubscriber/MigrationEventSubscriber.php).  

And you can get more information about creating events and event subscribers in Drupal here in The Russian Lullaby: [Building Symfony events for Drupal](https://www.therussianlullaby.com/blog/building-symfony-events-for-drupal/).  



### 3.2- Debug Process Plugin

The contributed module Migrate Devel also brings a new processing plugin called "debug" and defined in the Debug.php class. This PHP class can be found in the module path: /web/modules/contrib/migrate_devel/src/Plugin/migrate/process/Debug.php and we can check its responsibility by reading its annotation section in the class header: 

```toml
/**
 * Debug the process pipeline.
 *
 * Prints the input value, assuming that you are running the migration from the
 * command line, and sends it to the next step in the pipeline unaltered.
 *
 * Available configuration keys:
 * - label: (optional) a string to print before the debug output. Include any
 *   trailing punctuation or space characters.
 * - multiple: (optional) set to TRUE to ask the next step in the process
 *   pipeline to process array values individually, like the multiple_values
 *   plugin from the Migrate Plus module.
```

And it consists directly with the transform() method - inherited from the ProcessPluginBase abstract class, where instead of applying transformation actions during processing, it simply uses PHP's print_r function to display information by console and will print both scalar values and value arrays. 

This plugin can be used autonomously, being included as part of the migration pipeline, so that it prints results throughout the processing of all value rows. 
In this case, we are going to modify the pipeline of the processing section of our taxonomy terms migration, with the idea of reviewing the values being migrated. 

To begin with, we are going to modify the structure. We already know (from previous chapters) that this is actually the way: 

```toml
process:
 name: name
 description: description
 path: url
 status: published
```
It's just an implicit way of using the Get.php Plugin which is equivalent to:  

```toml
process:
 name:
   plugin: get
   source: name
 description:
   plugin: get
   source: description
 path:
   plugin: get
   source: url
 status:
   plugin: get
   source: published
```

Now we add to the pipeline the Debug plugin with an information label for processing: 

```toml
process:
 name:
   plugin: debug
   label: 'Processing name field value: '
   plugin: get
   source: name
 description:
   plugin: debug
   label: 'Processing description field value: '
   plugin: get
   source: description
 path:
   plugin: debug
   label: 'Processing path field value: '
   plugin: get
   source: url
 status:
   plugin: debug
   label: 'Processing status field value:  '
   plugin: get
   source: published
```
After this change we reload the migration configuration object by uninstalling and installing our module (as it is marked as a dependency, when uninstalled the migration configuration will be removed):  

```
drush pmu migration_google_sheet && drush en migration_google_sheet -y 
```
So when we run the migration now we will get on screen information about the values:  

![Drupal Migrate Devel feedback](../../images/post/davidjguru_drupal_migrations_debugging_seven.png)


This way we get more elaborated feedback on the information to be migrated. If we want to complete this information and thinking about more advanced scenarios, we can combine various arguments and options to gather as much information as possible. Let's think about reviewing the information related to only one element of the migration. We can run something like: 

```
 drush migrate-import --migrate-debug taxonomy_google_sheet --limit="1 items"
```
Which will combine the output after storage (unlike its --migrate-debug-pre option), showing in a combined way the output of the Plugin, the values via Kint and the final storage ID of the only processed entity.  

In this case, we only see basic values and with little processing complexity (we only extract from Source and load in Destiny) but in successive migrations we will be doing more complex processing treatments and it will be an information of much more value. Interesting? think about processing treatment for data values that must be adapted (concatenated, cut, added, etc)...if at each step we integrate a feedback, we can better observe the transformation sequence.  

Here you can check the Plugin code: [migrate_devel/src/Plugin/migrate/process/Debug.php](https://git.drupalcode.org/project/migrate_devel/-/blob/8.x-2.x/src/Plugin/migrate/process/Debug.php).  

Here you can review the Drupal.org Issue where the idea of implementing this processing Plugin originated: [https://www.drupal.org/node/3021648](https://www.drupal.org/node/3021648).

Well, with this approach to Migrations debugging we will start the series on debugging...soon more experiences! 
 
## 4- :wq! 


##### Recommended song

{{< youtube Z2tvlp7RnlM >}}