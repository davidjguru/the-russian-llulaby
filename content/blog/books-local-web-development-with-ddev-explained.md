---
title: "Books/ Local Web development with DDEV"
date: 2019-12-30
draft: false

# post thumb
image: "images/post/davidjguru_drupal_8_books_local_web_development_with_ddev_explained.png"

# meta description
description: "Book Review about Web development with DDEV explained"

# taxonomies
categories: 
  - "Reading"
tags:
  - "Docker"
  - "DDEV"
  - "Books"
  - "Containers"
  - "Drupal Development"

# post type
type: "post"
---

Lately, I'm interested in improving the tools of the project's stack, thanks to the advice of a friend I approached DDEV. I already knew Docker and everything related to its ecosystem (Docker, Docker-Compose, Docker Engine, Docker Swarm...) although it is true that not in a very advanced way in general, only at the level of the daily needs or small test games...so I recently acquired the book "Local web development with DDEV explained" written by Mike Anello  and I'd like to share my thoughts on that here.
So, Do you develop projects based on Drupal? do you know the DDEV tool for building environments? these may be important motivations, but there are more reasons why Mike Anello's book could be interesting for you...

--------------------------------------------------------------------------------------
**This article was originally published  in [https://davidjguru.github.io](https://davidjguru.github.io/blog/books-local-web-development-with-ddev-explained)**    
**Picture from Unsplash, user [Janko Ferlič, @itfeelslikefilm](https://unsplash.com/@itfeelslikefilm)**

  
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

As I have already mentioned on other occasions, I was lucky enough to know about DDEV [(https://www.ddev.com)](https://www.ddev.com/) from the advice of a friend, Pedro Cambra [(@pcambra)](https://twitter.com/pcambra). He told me about DDEV and he also told me about Randy Fay [(@randyfay)](https://twitter.com/randyfay), a developer of the staff behind the tool. With the first thing (using the tool) I tried a lot and with the second I worked by delegated criteria: if a friend spoke so well about Randy Fay, then I had to pay attention. That's how I started to test DDEV in my environments and projects and I followed Randy. 

**This is important:** Ain't no good technology without good people operating behind it and DDEV is a very, very rich project in its community and development. That's why (I guess) the tool and the Human team (here) is very related. Follow them.

Since I started using DDEV I have had time to learn some things, writing some articles:  
 * [Creating Development Environments for Drupal with DDEV](https://davidjguru.github.io/blog/creating-development-environments-for-drupal-with-ddev)
 * [Docker, Docker-Compose and DDEV - Cheatsheet](https://davidjguru.github.io/blog/containers-docker-docker-compose-ddev-cheatsheet)  
 
 ...And even attend some sessions about DDEV, like this one at Drupal Camp Spain 2019 by Diego Marrufo [(@dimaro_)](https://twitter.com/dimaro_) -in Spanish-:
 * [https://2019.drupalcamp.es/sessions/docker-para-todos-los-publicos-ddev.html](https://2019.drupalcamp.es/sessions/docker-para-todos-los-publicos-ddev.html)  
 
 After all this time, I can tell you that using DDEV has made me happy and being able to convince teams to align environments using DDEV has made me even happier. Now I am writing a long article about the tool in Spanish (my primary language) and I have been lucky enough to be able to read the book that Michael Anello [(@ultimike)](https://twitter.com/ultimike) has written about the subject.

As you can see, I have some relationship with the topic and now I'm writing these lines to present and evaluate the book.


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
