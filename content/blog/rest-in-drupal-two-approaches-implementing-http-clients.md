---
title: "REST in Drupal: two approaches implementing HTTP Clients"
date: 2021-04-12
draft: true

# post thumb
image: "images/post/davidjguru_drupal_8_9_lm_module_rest_services_main.jpg"

# meta description
description: "REST in Drupal: two approaches implementing HTTP Clients."

# taxonomies
categories:
  - "Development"
tags:
  - "Contrib Modules"
  - "Drupal Development"
  - "Backend"
  - "PHP"

# post type
type: "post"
---
The Representational State Transfer, or REST, is an old friend of inter-system communications that was designed by Roy Fielding circa 2000. As we already know, It's commonly used to create interactive applications using Web Services and called it RESTful. This combine a stateless client/server protocol (HTTP), a well-defined set of operations for all the resources (GET; POST; PUT, DELETED), and a universal syntax for resources(a resource is addressable only through its URI). But in this article I would like write about REST from a Drupal point of view, sharing a pair of models in order to implement new HTTP Client when we're developing our custom modules, related with Web Services.

--------------------------------------------------------------------------------------
**Picture from Unsplash, user [Krzysztof Niewolny, @epan5](https://unsplash.com/@epan5).**


---------------------------------------------------------------------------------
