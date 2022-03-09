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

GraphQL was intended as a way to avoid overfetching in other approaches such as REST APIs: you describe exactly what resources you will expose and how you want to expose them. You then build specific queries that will scrupulously respect these descriptions and fetch the exact data that has been requested. In this article we will take a brief look at GraphQL by way of introduction: installation, features, aspects to take into account, possible limitations and an index of recommended reading.  


**There we go!**

## Fast Rewind 
GraphQL is an open-source data query and manipulation language for APIs developed by Facebook in 2012 and publicly released in 2015. Now the GraphQL project is hosted by the Linux Foundation ([see the projects protected under the Linux Foundation umbrella here](https://www.linuxfoundation.org/projects/)).  

![GraphQL related projects under Linux Foundation](../../images/post/davidjguru_drupal_8_9_graphql_introduction_6.png)

**Common Terms**  
* Schema: A Schema is a description of resources that will be exposed via GraphQL. 
* Server: A server is the URL where a schema will be accessible.  
* graphqls: The file extension for descriptive schema files in GraphQL.  
* Resolvers: Are collections of functions that build a response for a GraphQL input query, acting like a GraphQL query handler.  
* Data Producers: A data producer is function taking arguments and processing these inputs into some other data.  
* GraphiQL Explorer: Visual tool for execute and review results of your query, your blackboard.  


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

All these parameters will function as filters within our custom DataProducer. If I don't respect the order of the parameters in the resolve method definition itself in my custom DataProducer, then I will get strange results. In the bottom tab of the image above you can see the values when debugging with Xdebug: we were not obtaining the required items, and we were not getting the expected values since what should be the offset (zero value), here is working as a limit (zero items to return). This may cause you a little headache and make you debug more than necessary, just for a matter of order in the parameters passed to the custom data producer class.  

## 3-Enable debugging mode  

Do you know the phrase "the future is where you will spend the rest of your life"? Well, in GraphQL's case, debugging mode will be the place for the rest of your life. You will have to define a lot of schemas, declare a lot of types, build a lot of queries and implement some custom  Data Producers...so the third small tip has to do with a quick action that will undoubtedly offer you great benefits.  

![Enable debugging options in GraphiQL explorer](../../images/post/davidjguru_drupal_8_9_graphql_introduction_3.png)

Enabling debug mode for your GraphQL environment will be essential. This, coupled with enabling Xdebug for debugging from the PHP side, will make any implementation that gets stuck along the way much easier to deal with. Enable debug in order to get exception specific messages going to `/admin/config/graphql/servers/manage/` and edit the configuration of your schema server. You will be very happy.  

![Getting feedback from debugging in GraphiQL explorer](../../images/post/davidjguru_drupal_8_9_graphql_introduction_4.png)


Read More about enabling Xdebug for Drupal:  

* [Introduction to Xdebug for DDEV](https://www.therussianlullaby.com/blog/drupal-migrations-five-debugging-migrations-ii/#1--introduction-to-xdebug)  
* [Drupal Techniques: Xdebug, DDEV and Postman](https://davidjguru.github.io/blog/drupal-techniques-xdebug-ddev-and-postman)  
* [Configuration for Debugging Decoupled Drupal using Lando](https://gist.github.com/davidjguru/68323f7f42b5093686c9e0b09a08832c)  

## 4- Maintain your code clean and do refactoring  

In the middle of working on a decoupled Drupal project can be common to define needs as you go along, on the fly. For example, from the frontend, it may be necessary to show a new listing.  

* Then frontend asks backend: I need a new list of items. 
* Backend responds by creating a custom data producer (if none fits well).
* Backend prepares a query extracting the required data:  

```
try {
  $node_storage = $this->entityTypeManager->getStorage('node');
  $type = $node_storage->getEntityType();

  // Extracting main query.
  $query = $node_storage->getQuery()
    ->currentRevision()
    ->accessCheck();

  $query
    ->condition('status', TRUE)
    ->condition('langcode', $language)
    ->condition('field_related', $field_value)
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

* Then, backend will prepare the data filtering:  
```
// Processing the resulting array of total nodes.
// First reduce the items cutting by offset and limit.
// Second allows only nodes with moderation_state as published.
$filtered_array = array_filter($available_nodes, function ($node){
  // @see https://www.drupal.org/project/drupal/issues/3025164
  return ( $node->get('moderation_state')->value == 'published');
}, TRUE);
```

And finally we'll prepare the array of values to return from our custom Data Producer:  

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
But then times goes by, the project progresses, other tickets are solved and the frontend colleagues start asking other things for the same query (or maybe yourself if you're working in full-stack mode). Anyway, it produces a request "Hey, I need to add to the query a value of items returned and the total number of available existing items in database, could you implement it, please?" and it's time to return to the code you left in that class.  

In your frantic life, the request for a calculation of available values will be translated as "I must run a count query." and you will add in a hurry, a new query in code:  
```
$query = $node_storage->getQuery()
    ->currentRevision()
    ->accessCheck();

  $query
    ->condition('status', TRUE)
    ->condition('langcode', $language)
    ->condition('field_related', $field_value)
    ->condition('type', ['type_one','type_two', 'type_three'], 'IN')
    ->sort('created', 'DESC');
    
$existing_items = $query->count()->execute();
```
Ok? Nopes, 'cause you already have a previous query and with some adjustments you can make, you will get the data you need for this new request. With a little refactoring your code can evolve better. If you don't try to think like that, your classes will grow out of control and in a short time they will start to become incomprehensible and out of logic. Instead of adding more logic, try to think in something like this, just after execute the first query:  

```
// Processing the resulting array of total nodes.
// First reduce the items cutting by offset and limit.
// Second allows only nodes with moderation_state as published.
$processed_array = array_slice($available_nodes, $offset, $limit, TRUE);
$filtered_array = array_filter($processed_array, function ($node){
  // @see https://www.drupal.org/project/drupal/issues/3025164
  return ( $node->get('moderation_state')->value == 'published');
}, TRUE);

// Load the values for available and returned items.
$batch = [
  'total' => count($available_nodes),
  'items' => count($filtered_array),
];
```
And then do the data processing from the returned set of nodes:  

```
// Prepare the array of page nodes(Type 1, Type 2, Type 3, Type 4). 
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

// Finally load the batch of nodes.
$batch['pages'] = $pages;
```
Keep your code in good shape. Think about You're adding new data by extending more and more values and sections in your returned array of data:  
![GraphQL example of parameters in queries](../../images/post/davidjguru_drupal_8_9_graphql_introduction_5.png)

**So please, remember:** review and revise the code above. Don't add more code than you need. Use the calculations you have already done and if necessary, change and refactor. Avoid unnecessary growth of your DataProducer classes.

## 5- GraphQL may not be your best option  

Despite the enormous flexibility GraphQL offers and the optimisation involved in building your own schemas and queries, it is true that it is not all advantages and you can have important problems and limitations. Intuitively we can say that GraphQL is not a good option if your team is small or your budget is tight, but in addition, I have gathered three more points to be able to meditate better if it is our solution:  

1. **Documentation is scarce and may not be very up to date.** You have few resources available to learn. You can go to [www.drupal.org/docs/graphql](https://www.drupal.org/docs/contributed-modules/graphql) and then you will see that the information may be [very little](https://www.drupal.org/docs/8/modules/graphql/fetching-data), [outdated](https://www.drupal.org/docs/8/modules/graphql/query-maps) or [non-existent](https://www.drupal.org/docs/contributed-modules/graphql/graphql-twig). This is not very motivating and you will have to build a curated reading list on your own. Of course, the Amazee Labs content list related to GraphQL is fundamental. It is arguably the company that has written the most about the Drupal & GraphQL partnership. You have the links in the last section, [Read More](#6--read-more).  

2. **You will have to expose everything-everything to your frontend.** This usually includes providing all the navigation: menus, links, sections, paths, content: titles, bodies, taxonomy terms and other available fields. Do you use search subsystems? searching-indexes? elastic search? you'll have to expose and connect every resource. And all the structural and meta-information for SEO: Blocks, sections, layout regions information, metatags, descriptions, etc. This can increase the workload of a project exponentially. It may not be your option if you don't have the right knowledge, the right budget, the time required or if your team is small.  

3. **You will have to operate from scratch** As with the implementation of other resources for external connections (REST, JSON API), GraphQL for Drupal also set by default an exposure of all existing resources in the Drupal installation. You could simply install the module and get all entities available via GraphQL. But this has changed between version 3 and version 4, and is no longer available... now users must manually describe schemas for every entity and resource that they need. [You can see the discussion here](https://github.com/drupal-graphql/graphql/issues/1021). The final fact is that you may need (even more) work - time to be able to build all the schemes you need.  


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
