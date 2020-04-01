---
title: "Building Symfony events for Drupal"
date: 2020-04-01
draft: false

# post thumb
image: "images/post/davidjguru_drupal_events_based_in_symfony_main.jpg"

# meta description
description: "How to build events and events subscribers in Drupal"

# taxonomies
categories: 
  - "Development"
tags:
  - "PHP"
  - "Symfony"
  - "Backend"
  - "Events"
  - "Drupal Development"

# post type
type: "post"
---

Undoubtedly, in the latest versions of Drupal, certain components of Symfony have not only made their appearance, but also have been gaining importance and surely this will extend further in time, given its elasticity and fully integrated OOP approach. One of these components is the HttpKernel, a very valuable subsystem for building request-response relationships in a technology (Silex, Symfony, Drupal).

--------------------------------------------------------------------------------------
 
**Picture from Unsplash, user [Noiseporn, @noiseporn](https://unsplash.com/@noiseporn)**

  
---------------------------------------------------------------------------------

**Table of Contents**  
<!-- TOC -->  
[1- Introduction](#1--introduction)  
[2- Concepts of Event and Event Subscriber](#2--concepts-of-event-and-event-subscriber)  
[3- Building the Event Dispatcher](#3--building-the-event-dispatcher)  
[4- Building the Event Subscriber](#4--building-the-event-subscriber)  
[5- Read More](#5--read-more)  
[6- :wq!](#6--wq)  
<!-- /TOC -->

-------------------------------------------------------------------------------

## 1- Introduction
As we said, HttpKernel very flexible that allows us to structure relationships requests - response. Let's see what it says about itself in its [Symfony documentation](https://symfony.com/doc/current/components/http_kernel.html): 

> *The HttpKernel component provides a structured process for converting a Request into a Response by making use of the EventDispatcher component. It's flexible enough to create a full-stack framework (Symfony), a micro-framework (Silex) or an advanced CMS system (Drupal).*

In this context where I wanted to practice a little bit with the concepts of "Event" and "Event Subscription". Let's see. 

## 2- Event and Event Subscriber

**Concept of Event:** Event systems are ways to allow extensions in many applications. Generally there are some similar topics and concepts, wich usually include common elements:

1. **Event Registry:** Just a place where event subscribers will be grouped.    
1. **Event Subscribers / Event Listeners:** Callable functions or methods that react to an event.  
1. **Event Dispatcher:** The way the event is triggered.    
1. **Event Context:** Some information shared for the subscribers of an event.     


**Getting information about services and events:**
Services? Events? but...how do I know how many I have available at my Drupal installation? Well you can use Drupal Console with a pair of commands to get the info:

```
drupal debug:container #Get list of available services in a Drupal site.
drupal debug:event #Get a list of available events in your Drupal site.
```


**Events in Drupal:** From Drupal 8, the Symfony Event Dispatcher is one the most important components in a Drupal installation. This component is responsible for launching notifications from your applications. You can be listening these notifications and returning some responses executing your custom code.   
In Drupal 8, events are defined in a class and declared in a YML file, then dispatched using a function call on the core Drupal event_dispatcher service.

**What is an Event Subscriber:** A very common task in the Drupal development it's intercept some request from the browser to your Drupal, getting something interesting on it, and then redirect the user to another location in the site.    

Well, in Drupal 7 we were using a combination of [hook_init()](https://api.drupal.org/api/drupal/modules%21system%21system.api.php/function/hook_init/7.x) with a weird function called [drupal_goto()](https://api.drupal.org/api/drupal/includes%21common.inc/function/drupal_goto/7.x), but now in Drupal >= 8 we're doing this using with the symfony model for subscribing us to the ```kernel.request``` event.   

We can launch a redirect from a Controller, but in this case we want to test the topic of Event-Subscribers in Drupal.   

## 3- Building the Event Dispatcher

**Note:** The example requires that you have previously created a new custom module with its ```custom_module.info.yml``` in your Drupal installation. 

**Note II:** This example is from [Drupal 8 Module Development (second edition) by Daniel Sipos](https://www.packtpub.com/web-development/drupal-8-module-development-second-edition-0), the Holy Bible of the Drupal Development.   
**Pag 65:** *"We will create an event to be dispatched whenever our HelloWorldSalutation::getSalutation() method is called. The purpose is to inform other modules that this happened and potentially allow them to alter the message."* 

### 3.1- Creating an event class
**File:** SalutationEvent.php  
**Location:** /custom_module/  

```
<?php
namespace Drupal\custom_module;

use Symfony\Component\EventDispatcher\Event;

/**
 *Event class to be dispatched from the HelloWorldSalutation service.
 */
class SalutationEvent extends Event {

const EVENT = 'hello_world.salutation_event';

/**
 * The salutation message.
 *
 * @var string
 */
protected $message;

/**
 * @return mixed
 */
public function getValue() {
    return $this->message;
}

/**
 * @param mixed $message 
 */
public function setValue($message) {
    $this->message = $message;
 } 
}

```

### 3.2- Injecting the Event Dispatcher service

```toml
services:
  hello_world.salutation:
    class: 'Drupal\hello_world\HelloWorldSalutation'
    arguments: ['@config.factory', '@event_dispatcher']
    tags:
      - { name: salutation }
```

## 4- Building the Event Subscriber
Given an internal path in Drupal, we're going to define a new event subscriber to catch the request, review certain conditions of the current user of Drupal and if there's a matching, then we'll set a new redirect to another route in your system bypassing the process.     

We know that the system allows subscribers to change data before the business logic uses it for something. So in order to register the new event subscriber, you  have to create a new service related with the former class (that implements the interface) and tagged with the ```event_subscriber``` key.   
 
In this example, when a user request our custom route, we'll check if the current user has assigned a "non_grata" role and if the result is positive, then we'll redirect him to the home page of the web portal, without allowing him to access our route. In order to execute this, we'll need two injected services: the [AccountProxyInterface](https://api.drupal.org/api/drupal/core%21lib%21Drupal%21Core%21Session%21AccountProxyInterface.php/interface/AccountProxyInterface/8.8.x) for get the user related data and [CurrentRouteMatch](https://api.drupal.org/api/drupal/core%21lib%21Drupal%21Core%21Session%21AccountProxyInterface.php/interface/AccountProxyInterface/8.8.x) to test the requested route. 

**Required parts:**

1. A declared internal route (file custom_module.routing.yml in ```custom_module/```)  
2. A new declared service (file custom_module.services.yml in ```custom_module/```)  
3. A new class CustomModuleRedirectSubscriber implementing the EventsSubscriberInterface (file in ```custom_module/src/EventSubscriber/```)  


**Note:** The example requires that you have previously created a new custom module with its custom_module.info.yml to register in your Drupal. 

### 4.1- The route 

```toml
custom_module.hello:
  path: '/hello'
  defaults:
    _controller: '\Drupal\custom_module\Controller\CustomModuleController::helloWorld'
    _title: 'Our Testing Route'
  requirements:
    _permission: 'access content'
```

### 4.2- The Service

```toml
 custom_module.redirect_subscriber:
    class: '\Drupal\custom_module\EventSubscriber\CustomModuleRedirectSubscriber'
    arguments: ['@current_user', '@current_route_match']
    tags:
      - { name: event_subscriber }
```

### 4.3- The Class CustomModuleRedirectSubscriber.php

```
<?php


namespace Drupal\custom_module\EventSubscriber;


use Drupal\Core\Routing\CurrentRouteMatch;
use Drupal\Core\Routing\LocalRedirectResponse;
use Symfony\Component\EventDispatcher\EventSubscriberInterface;
use Drupal\Core\Session\AccountProxyInterface;
use Symfony\Component\HttpKernel\Event\GetResponseEvent;
use Symfony\Component\HttpKernel\KernelEvents;

/**
 * Class CustomModuleRedirectSubscriber Subscribes to the Kernel Request events.
 */
class CustomModuleRedirectSubscriber implements EventSubscriberInterface {

  /**
   * @var \Drupal\Core\Session\AccountProxyInterface
   */
  protected $currentUser;

  /**
   * @var \Drupal\Core\Routing\CurrentRouteMatch
   */
  protected $currentRouteMatch;

  /**
   * HelloWorldRedirectSubscriber constructor.
   *
   * @param \Drupal\Core\Session\AccountProxyInterface $currentUser
   * @param CurrentRouteMatch $currentRouteMatch
   */
  public function __construct(AccountProxyInterface $currentUser, 
                              CurrentRouteMatch $currentRouteMatch) {
    $this->currentUser = $currentUser;
    $this->currentRouteMatch = $currentRouteMatch;
  }



  /**
   * {@inheritdoc}
   */
  public static function getSubscribedEvents() {
    $events[KernelEvents::REQUEST][] = ['onRequest', 0];
    return $events;
    }

  /**
   * Handler for the kernel request event.
   *
   * @param \Symfony\Component\HttpKernel\Event\GetResponseEvent $event
   *
   */
  public function onRequest(GetResponseEvent $event){
    $route_name = $this->currentRouteMatch->getRouteName();
    if ($route_name !== 'custom_module.hello') {
      return;
    }

    $roles = $this->currentUser->getRoles();
    if(in_array('non_grata', $roles)) {
      $url = Url::fromUri('internal:/');
      $event->setResponse(new LocalRedirectResponse($url->toString()));
   }
  }
}
```

## 5- Read More

Here you can find much more information about the concepts previously exposed and consult a multitude of use cases and examples:

* [DOC: Use of events in the Rules module](https://www.drupal.org/docs/8/modules/d8-rules-essentials/for-developers/events).   
* [DOC: Subscribe to and dispatch events (Event Systems Overview)](https://www.drupal.org/docs/8/creating-custom-modules/subscribe-to-and-dispatch-events).    
* [DOC Event Dispatcher examples in the Symfony documentation](https://symfony.com/doc/current/event_dispatcher.html).  
* [DOC: The HttpKernel in Symfony](https://symfony.com/doc/current/components/http_kernel.html).  
* [DOC: The KernelEvents class in Symfony](https://symfony.com/doc/current/reference/events.html#kernel-events).  
* [DOC: The EventSubscriberInterface from Symfony in Drupal](https://api.drupal.org/api/drupal/vendor%21symfony%21event-dispatcher%21EventSubscriberInterface.php/interface/EventSubscriberInterface/8.2.x).   


## 6- :wq!

##### Recommended song

{{< youtube KshrtLXBdl8 >}}
