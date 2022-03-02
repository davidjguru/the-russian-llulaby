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

I have been working on openly decoupled Drupal projects for some time now, and I would like to share some experiences.
I'm not a GraphQL advocate, I think there are already many people and many platforms that do it quite well, so I'm not interested in holy wars. In fact I wouldn't convince you to use GraphQL in your project, but there are situations where someone may be conditioned by circumstances to work with GraphQL.
So I have taken as an excuse the intention of publishing five basic ideas about GraphQL in Drupal. As an introductory article, but taking advantage of some initial notes. 


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

After creating the server click on dropdown button at right, click in "Explorer" option and this should bring you to [the GraphiQL explorer](https://github.com/graphql/graphiql/tree/main/packages/graphiql#readme), or you can go directly to `/admin/config/graphql/servers/manage/initial_example/explorer` and there you will see the visual explorer GraphiQL for visualizing queries within your Drupal installation. This will be your new best friend, the place where you will live from now on:  


![GraphiQL Visual Explorer for GraphQL in Drupal](../../images/post/davidjguru_drupal_8_9_graphql_introduction_05.png)


You can use some plugins for IDEs too, like [the graphiql-explorer for VSCode](https://marketplace.visualstudio.com/items?itemName=GabrielNordeborn.vscode-graphiql-explorer). If you don't have much experience in the GraphQL - Drupal combination, you can review the source code of this contributed module and the folder structure with the resources, within your project' folder or [within its own gitlab repository](https://git.drupalcode.org/project/graphql/-/tree/8.x-4.x/examples).  




## 1- Align file naming and resources

Well, the first small idea to share (small but very useful), is this: always keep the names of the internal resources of each module aligned. This will make your life much easier and will make you happy, for sure, reducing the time to search for errors without results.  
At the beginning it will quite simple but the more your project grows, the more GraphQL custom modules it will contain with more files, with more resources and with different names. It will be easy to start a new GraphQL module by copying and pasting files from a previous module and there... you may forget to change names.  

My advice: before the GraphQL dimension of your project grows, make naming patterns for custom resources. See the next screen caption:  
![Align naming in files](../../images/post/davidjguru_drupal_8_9_graphql_introduction_1.png)

Ok, as you can see when you're working with GraphQL in Drupal you have to create new custom resources, new custom modules containing all the required files for extending your current Schema and defining new types, fields, queries... See the former image: think about your folder structure and get a naming pattern, something like:  

```txt

graphql_custom_name/
  | | | 
  | | |_ _ graphql/
  | |           |
  | |           |_ _ custom_name.base.graphqls
  | |           |_ _ custom_name.extension.graphqls
  | |
  | |_ _ _ src/Plugin/GraphQL/
  |                      |
  |                      |_ _ DataProducer/
  |                      |        \
  |                      |         \_ _ CustomName.php
  |                      |     
  |                      |_ _ SchemaExtension/
  |                              \
  |                               \_ _ CustomNameSchemaExtension.php
  |                            
  |_ _ _ _ graphql_custom_name.info.yml
```

This can be important to delay chaos within the project. Especially if not only you work with GraphQL within the project. Go ahead and propose to your colleagues naming patterns that you can use comfortably. 

## 2- Respect the order of parameters

Another basic aspect but one that can take up a lot of your time... please, just keep the order of the parameters in the same sense that you expect it in your DataProducer, ok? See the following image:  
![GraphQL example of parameters in queries](../../images/post/davidjguru_drupal_8_9_graphql_introduction_2.png)

As you can see in the image above, first you've described a new extension for a Query type, in this example is for querying "News" in our Drupal installation. Then you're adding a new Schema Extension, adding FieldResolvers for the new query and you are using "query_news" as your custom DataProducer. In this declaration to register, you're sending the parameters in the order:  

```txt
collection_id => Identifier of the collection to query.
                 Taken as an incoming argument from the query parameters.
offset => Starting point for the query of items.
          Taken as an incoming argument from the query parameters.
limit => How many items do you want to get.
         Taken as an incoming argument from the query parameters.
language => The language code of the requested items.
            Taken as an incoming argument from the query parameters.
```

All these parameters will later function as filters within our custom DataProducer. If I don't respect the order of the parameters in the resolve method definition itself in my custom DataProducer, then I will get strange results. In the bottom tab of the image above you can see the values when debugging with Xdebug: we were not obtaining the required items, and we were not getting the expected values since what should be the offset (zero value), here is working as a limit (zero items to return). This may cause you a little headache and make you debug more than necessary, just for a matter of order in the parameters passed to the custom data producer class.  

## 3-Enable debugging mode  

The third small tip has to do with a quick action that will undoubtedly offer you great benefits.  

![Enable debugging options in GraphiQL explorer](../../images/post/davidjguru_drupal_8_9_graphql_introduction_3.png)



![Getting feedback from debugging in GraphiQL explorer](../../images/post/davidjguru_drupal_8_9_graphql_introduction_4.png)

## 4- Maintain your code clean and do refactoring  

In the middle of working on a decoupled Drupal project can be common to define needs as you go along, on the fly. For example, from the frontend, it may be necessary to show a new listing.  

* Then frontend asks backend: I need a new list of items. 
* Backend responds by creating a custom data producer (if none fits well).
* Backend prepares a query extracting the required data:  

```
try {
  $node_storage = $this->entityTypeManager->getStorage('node');
  $type = $node_storage->getEntityType();

  // Extracting and Counting main query.
  $query = $node_storage->getQuery()
    ->currentRevision()
    ->accessCheck();

  $query
    ->condition('status', TRUE)
    ->condition('langcode', $language)
    ->condition('field_related_collection_page', $collection_id)
    ->condition('type', ['type_one','type_two', 'type_three'], 'IN')
    ->sort('created', 'DESC');

  // Add cache info.
  $metadata->addCacheTags($type->getListCacheTags());
  $metadata->addCacheContexts($type->getListCacheContexts());

  // Get the array of node ids and process the required info.
  $node_ids = $query->execute();
  $available_nodes = $node_storage->loadMultiple($node_ids);
}
catch (\Exception $e) {
  return [];
}
```

* Backend also will prepare the processing the data to return:  
```
// Processing the resulting array of total nodes.
// First reduce the items cutting by offset and limit.
// Second allows only nodes with moderation_state as published.
$processed_array = array_slice($available_nodes, $offset, $limit, TRUE);
$filtered_array = array_filter($processed_array, function ($node){
  // @see https://www.drupal.org/project/drupal/issues/3025164
  return ( $node->get('moderation_state')->value == 'published');
}, TRUE);
```

And finally:  

```
// Prepare the array of page nodes(News, Local News, Blog Page, Press Page). 
$pages = [];
foreach ($filtered_array as $node) {
    $id = $node->id();
    $final_url = $node->toUrl()->toString(TRUE);
    $url_string = $final_url->getGeneratedUrl();
    $pages[] = [
      'id' => $id,
      'preamble' => $node->get('field_summary')->value,
      'title' => $node->title->value,
      'url' => $url_string,
      'bundle' => $node->bundle(),
    ];
}

return $pages;
}
```

You're adding new items by extending more and more values and sections in your returned array:  
![GraphQL example of parameters in queries](../../images/post/davidjguru_drupal_8_9_graphql_introduction_5.png)


## 5- GraphQL may not be your best option  

1. **Documentation is scarce and may not be very up to date.** You have few resources available to learn. You can go to [www.drupal.org/docs/graphql](https://www.drupal.org/docs/contributed-modules/graphql) and then you will see that the information may be [very little](https://www.drupal.org/docs/8/modules/graphql/fetching-data), [outdated](https://www.drupal.org/docs/8/modules/graphql/query-maps) or [non-existent](https://www.drupal.org/docs/contributed-modules/graphql/graphql-twig). This is not very motivating and you will have to build a curated reading list on your own. Of course, the Amazee Labs content list related to GraphQL is fundamental. It is arguably the company that has written the most about the Drupal & GraphQL partnership. You have the links in the last section, [Read More](#6--read-more).  

2. **You will have to expose everything-everything to your frontend.** This usually includes providing all the navigation: menus, links, sections, paths, content: titles, bodies, taxonomy terms and other available fields. And all the structural and meta-information for SEO: Blocks, sections, layout regions information, metatags, descriptions, etc. This can increase the workload of a project exponentially. It may not be your option if you don't have the right knowledge, the right budget, the time required or if your team is small.  

3. 

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

If you have managed to reach the end of this article, Thank you very much! Thanks for your patience and I really hope it has been useful to you. I wish you good luck working with Drupal and GraphQL so you will learn a lot (I'm doing it now). I leave you with a final song, which you can find [in the Spotify playlist "The Russian Lullaby"](https://open.spotify.com/playlist/47hhvxX1IFhzN4QQ1g1mzx?si=6eae9c5660cc4be6).  

See you next time!  


##### Recommended song: The Troublemakers - Get Misunderstood

{{< youtube vCMGyiDF2zI >}}
