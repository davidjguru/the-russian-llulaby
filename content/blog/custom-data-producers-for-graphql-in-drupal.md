---
title: "Custom data producers for GraphQL in Drupal"
date: 2022-01-29
draft: true

# post thumb
image: "images/post/davidjguru_drupal_8_9_custom_data_producers_for_graphql_in_drupal_main.jpg"

# meta description
description: "How to implement custom data producers based in GraphQL for Drupal."

# taxonomies
categories:
  - "Development"
tags:
  - "Drupal Plugins"
  - "Drupal Development"
  - "Backend"
  - "GraphQL"

# post type
type: "post"
---
Imagine that you have to integrate JavaScript code into your Drupal project... Where do you start? How do you do it? You're looking for information but you don't find anything "holistic", something that goes from 0 to 100 and that puts in context how the relationships between Drupal and JavaScript are structured. Well, this article was made for you (Or for other people in your team that you want to introduce to this topic).

In this guide you will learn basic concepts of JavaScript, the terminology used in Drupal, functions, methods and common mechanics to enrich your projects by make them run with executable code on the client side. And all through a combination of theory and practice. It includes some exercises that I have integrated.

--------------------------------------------------------------------------------------
**Picture from Unsplash, user [Minoru Nakajima, @architraeveeee](https://unsplash.com/@architraeveeee).**


---------------------------------------------------------------------------------

**Index of sections**
<!-- TOC -->
[1- Introduction](#1--introduction)  
[2- Very fast review of GraphQL](#2--very-fast-review-of-graphql)  
[10- :wq!](#10--wq)


---------------------------------------------------------------------------------


## 1- Introduction

[GraphQL](https://graphql.org/) has been a new standard for connections between backend and frontend from recent years, allowing Drupal being decoupled from its own frontend and letting us taken one or another framework for building interfaces (Vue, React, Angular, etc).   
## 2- (Very fast) Review of GraphQL 

GraphQl is a query language for API, alternative to REST. Was developed by Facebook and now is open-source. As an API, can define how a client can load data from a server, executing queries by using a typed system you define for your data.  

**What's the difference?**   
- REST enable multiple endpoints that return fixed data structures.   
- GraphQL server only exposes a single endpoint and responds dynamically with requested data.   

So, basically GraphQL try to surpass REST avoiding overfetching (downloading superfluous data) and underfetching (additional requests to fetch everything you need, n+1).    

_GraphQL is a query language for APIs_ and you can use from the framework you select (not only React from Facebook). We're going to review some basic concepts in GraphQL.  

#### Schema Definition Language  

GraphQL has its own syntax, [the Schema Definition Language (SDL)](https://www.prisma.io/blog/graphql-sdl-schema-definition-language-6755bcb9ce51). From this syntax we can modeling all our types, for instance (e.g):    

```graphql 
type Person {
  name: String!
  age: Int!
}

type Car {
  model: String!
  year: Int!
  colour: String!
}
```

This type `Car` has three fields: model, year and colour and are respectively of type String and Int. The ! following the type means that this field is required.  

### Schema

Basically, a schema is a set of type and data descriptions describing the set of possible data you can query on the GraphQL service that will be used to compare incoming requests by validating and executing them if they are allowed by your system. Schemas delimit - very concretely - the limits of what is possible (and what is not) in your GraphQL server.  

### Queries 

### Mutations 

In GraphQL, we need cover the C-U-D fields of the CRUD concept (Create, Updating, Delete). With queries we have the R (Read) but we need the others. Ok, in GraphQL they are called "Mutations" like a way to execute changes using GraphQL.  

```graphql
mutation {
  createCar(model: "Ford", year: 1997, colour: "red") {
    model
    year
    colour
  }
}
```

### Subscriptions 

GraphQL offers the subscription, a way for receiving updates from a specific event.  
Subscriptions are written using the same syntax as queries and mutations. Here’s an example where we subscribe on events happening on the Person type:

```graphql
subscription {
  newCar {
    model
    year
    colour
  }
}
```

And so, when a client sent this subscription to a server, a connection is opened between them. Then, whenever a new mutation is performed creating a new Car, the server sends the information about this car to the client:

{
  "newPerson": {
    "name": "Jane",
    "age": 23
  }
}

### Services 

A GraphQL service is created by defining types and fields on those types, and then generating functions for each field on each type.  


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


### Creating dummy content for testing in Drupal

We can create on the fly new content in our Drupal installation with only three commands in our console:  

```bash
$ composer require drupal/devel
$ drush en -y devel_generate
$ drush genc --bundles=article 10
```

![Executing queries from GraphQL explorer in Drupal](../../images/post/davidjguru_drupal_8_9_custom_data_producers_for_graphql_in_drupal_2.png)

## 9- Read More 

* [Valuebound: GraphQL a Beginners Guide (2018)](https://www.valuebound.com/resources/blog/graphql-beginners-guide)  
* [Specbee: GraphQL with Drupal 8: All you need to know (2019)](https://www.specbee.com/blogs/graphQL-with-drupal-8-what-is-graphql-Advantages-need-to-know-Guide)  
* [OpenSense Labs: Why is GraphQL an Importante Player in Decoupled Drupal? (2020)](https://opensenselabs.com/blog/articles/graphql-important-player-decoupled-drupal)  
* []()  
## 10 - :wq!

##### Recommended song: The Troublemakers - Get Misunderstood  


{{< youtube vCMGyiDF2zI >}}
