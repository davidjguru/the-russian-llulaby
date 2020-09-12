---
title: "Books/ Drupal 9 Module Development"
date: 2020-09-12
draft: true

# post thumb
image: "images/post/davidjguru_books_drupal_module_development_main.png"

# meta description
description: "Review of the book Drupal 9 Module Development by Daniel Sipos"

# taxonomies
categories: 
  - "Reading"
tags:
  - "Books"
  - "Drupal Development"
  - "Backend"
  - "PHP"
  - "Drupal Plugins"

# post type
type: "post"
---


For a long time I was thinking about write or not a review of the Drupal 8 Module Development (the former edition of the current). For me there were two very important keys: on the one hand, it was a very ambitious book in terms of scope and content (so it was an important challenge to make a synthesis on its simple review). On the other hand, Drupal 9 was already on its way and it was possible that it would be deprecated quickly. Luckily, Drupal 9 has already arrived and the transition from 8 to 9 has not only not been at all traumatic (there are still many people trapped in the crack between Drupal 7 and Drupal 8), but it has also been an easy road. There won't be substantial or profound differences in how to approach the creation of custom modules in Drupal 9 respect to Drupal 8 and with this new edition adapted to Drupal 9, everything is ready to review this ambitious book. There we go!  

--------------------------------------------------------------------------------------
**Picture from Unsplash, user [Erol Ahmed, @erol](https://unsplash.com/@erol)**

  
---------------------------------------------------------------------------------

**Table of Contents**  
<!-- TOC -->  
[1- Introduction](#1--introduction)  
[2- The Book](#2--the-book)  
[3- Recommendations](#3--recommendations)  
[4- Book Information](#4--book-information)  
[5- Fast Review](#5--fast-review)  
[6- Ratings](#6--ratings)  
[7- :wq!](#7--wq)  
<!-- /TOC -->

-------------------------------------------------------------------------------

## 1- Introduction

Daniel Sipos (or just Danny Sipos, or [Upchuk in Drupal.org](https://www.drupal.org/u/upchuk) or [@drupalexp in twitter](https://twitter.com/drupalexp)) is a developer related to Drupal for many years. He is also a regular speaker at Drupal events and is also the person behind the website called Web Omelette ([https://www.webomelette.com](https://www.webomelette.com/)), such a popular website that you should get to know it if you're a Drupal developer (put the URL in your favorites if you don't know).  




## 2- The Book
![Image of DDEV explained](../../images/post/davidjguru_drupal_8_books_ddev_explained_one.png)

The fact that this book has a lot of experience behind it is something you can see, and a lot. Not in vain it is written by Mike Anello, one of the thinking heads behind Drupal Easy, a company with a lot of experience in training processes in general and Drupal in particular. But it is also edited by OSTraining, another organization strongly focused on training, and this is a quite important plus.

I can say -_without fear of being wrong_-, that this is the most consistent handbook on technology I have read recently, and by this I mean that any technological proposal should consist of an intuitive sequence:

1. **Problem** / Need that comes to solve.
2. **Analysis** of the situation.
3. **Proposal** (technical solution).

Well, this is the sequence that we normally do not find in the _anti-teaching_ of technology. And this is where the construction of this book shines. Here and in the background, and the training background is evident in the fact that this scheme is perfectly fulfilled in this book: we obtain, before anything else, a reference model on the configuration of environments with which we can compare the existence of a functional GAP in our day-to-day life: so we can assess in what moment we are and what sense would have the adoption of the proposed technical solution.   

We're tired of running after new libraries, versions, etc. Increasing the tooling of our projects, simply chasing the hype, without having clear ideas about what problem we are trying to solve, and this is a very important thing (in fact, the only important thing).  

It is very gratifying to see that the book devotes an entire chapter to the presentation of the initial problem as a premise (_Chapter 2: Introducing Our web development problem_), and then continues with the ideal process model (_Chapter 3: Professional Development Workflows Explained_). After these initial chapters, we will move on to more specific DDEV issues such as:  

* Basics of DDEV  
* Installing DDEV (Windows, Mac, Linux)  
* Installing new sites with DDEV (Drupal, WordPress)  
* Cloning existing sites with DDEV (Drupal, WordPress)  

And these are only the "_essential_" chapters about the tool...in addition to these, we have other chapters dedicated to equally important issues, such as: DDEV commands, Tips & Tricks, an specific example of Apache Solr-Drupal integration using DDEV containers, sharing the DDEV project with ngrok, or how to configure Xdebug - PHP Storm in order to work with your DDEV projects.   

At the end, bringing all pieces together, you will get a complete map of how to approach the project development process (workflow) using DDEV from start to finish, from local to live.  
If you have doubts or you only know some sections but you don't know the rest, instead of reading N:M articles about the subject, you can have this whole blueprint reading this book. 


## 3- Recommendations
Mainly, this book is indicated for anyone who wants to get introduced to the construction of working environments for WordPress or Drupal. 

In principle, there are no key prerequisites to enjoy this book, but due to the special emphasis it places on the ideal characteristics of a development environment/team, its reading makes it especially interesting for those people who, due to work circumstances or management conditions, cannot work in the way they might prefer. If you choose to make your team a more productive environment, then it is your book. 

![Tips from DDEV explained](../../images/post/davidjguru_drupal_8_books_ddev_explained_two.png)

The book has a very specific target...
* **Do you feel the irremediable existential void when you are solving configuration problems in the project-environment relationship for several days?**  
Well, this book is for you.
* **Do you think you can convince your team of the importance of aligning environments around an easy, simple, agile and intuitive solution?**  
This book is also for you.
* **Do you think you need to provide evidence to an _old-school_ manager in order to consider more productive the adoption of this kind of tools?**  
  Don't hesitate, this is your book.

You can take it in hand and hold it up in front of your managers as if it were Mao's little red book. For us, it will be like that. I assure you.

And remember: **"Code flows up, Data flows down"**. Always.


## 4- Book Information

| Field         | Description   |
| ------------- |:-------------:|
| Title         | Local web Development with DDEV Explained. |
| Author      | Michael Anello.      |
| Publisher | OSTraining      |
| Date | July 15, 2019      |
| Pages | 157      |
| Overview | DDEV tool installation and use manual.      |
| Keywords | DDEV, WordPress, Drupal, Docker, Development, Environment.      |
| Price | 9.99$ // 8.95€ (aprox)      |
| Links | [Ostraining](https://www.ostraining.com/books/local/">https://www.ostraining.com/books/local/), [Amazon](https://www.amazon.es/Local-Development-Explained-Step-Step/dp/1731048858)      |


## 5- Fast Review

| Question // Answer         | 
| ------------- |
| 1- Is it progressive, Iterative and Incremental?         |
| Yes, the book has a simple - complex sequencing.         |
| 2- Does it offer specific solutions to particular problems or concrete issues?         |
| Yes, the book is built in a clear problem-solution scheme.         |
| 3- Does it explain well the original problems or needs it aims to solve?       |
| Yes, exactly the book begins by setting out the premises of a good environment and the related problems.        |
| 4- Is it rich in examples?         |
| Yes, it contains many examples and practical demonstrations.          |
| 5- Is it written in plain English, suitable for non-English speakers?          |
| Yes, it’s a very comfortable reading, very simple, very pleasant.         |
| 6- Is it up to date?         |
| Yes, it is updated frequently. This edition I have is from July 2019 and I know there is a later edition from August 2019.          |


## 6- Ratings

![Table of Book Ratings for DDEV](../../images/post/davidjguru_drupal_8_ddev_rating_table_books.png)


## 7- :wq!


##### Recommended song

{{< youtube zqNTltOGh5c >}}
