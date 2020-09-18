---
layout: post
title:  '"SQL and Data Modeling for the Web" - Comments on the Udacity Course'
date:   2020-09-17 00:00:00 -0700
categories: 
---
* This course is part of Udacity's nanodegree program for Full Stack Web Developer. I went through almost all of the course material earlier this year.

* The course was really good for someone in my situation, which is intermediate Python developer wanting to expand their awareness and understanding of the larger Python ecosystem. The material is almost entirely focused on the backend implementation.

* However, there is significant client-side JavaScript to work on if you want to push on it. This turned out to be quite a time sink in figuring out exactly the interactions between the frontend and the backend in order to implement apps that are fully functioning, e.g. for the ToDo app exercise early on. The course glossed over much of the frontend stuff and felt incomplete in that regard, but it was worthwhile to put some effort into the entire stack, albeit it was frustrating at times.

* Video lessons are very short, well organized, and well presented.

* You need to know at least some Python, including exceptions and some of understanding of object-oriented programming. You also need to have basic background in SQL and the relational database model. PostgreSQL is used. This is not a course for total beginners.

* Flask, SQLAlchemy, Flask-Migrations/Alembic are introduced and covered. These are all Python modules.

* SQLAlchemy appears to be a fairly thick ORM (object-relational mapping) layer between the controllers written in Python and the underlying database. For example, relationships and backrefs are specific to  SQLAlchemy and do not affect database schemas. They are used to define Python properties on the classes that serve as database models and that reflect the database tables.

* In SQLAlchemy, when using the `relationship()` method, the `backref` argument is the name of a property that is added to the class whose name is the first argument. I don't think this was explicitly described, which made it ambiguous and confusing.

* I managed to badly break Postgres while playing around with starting and stopping the Postgres service on my Ubuntu / Linux system. I am not sure, but I think it was related to which user I was logged in as while messing with the service. I ended up having to completely uninstall Postgres and all of its dependent packages, then re-install all of them. 

* PostgreSQL is new to me. As such, I wasn't aware that  Postgres automatically folds identifiers (including table names) to lowercase unless the identifier is in double quote marks, which preserves the case. This particular idiosyncrasy caused a couple of hours of puzzlement and frustration working in the `psql` utility while doing database migrations.

* In at least one case, JavaScript access to retrieve the value of a DOM element was different from what the lesson showed. I had to use Chrome developer tools to dig through the DOM tree to figure out how to get to that element. So it's really helpful to have a browser with such tools, including console output and the ability to inspect DOM elements. I used Chrome, but Firefox would probably be fine too.

* When learning on my own time and my own dime like this, I will often pursue side roads that are not essential to completing the course but add to my understanding. For example, I was not familiar with JavaScript _promises_, so I wrote code to experiment with what those are and to acquire a good understanding. It took me quite a bit longer to finish the course than its suggested number of hours. 

* There were definitely some speed bumps, a few of which I've mentioned above. Overall, the course was worthwhile for my particular focus and interests.


