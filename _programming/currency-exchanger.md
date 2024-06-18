---
title: "Currency Exchanger"
excerpt: "Django(Python) App with real api usage"
date: "2021-07-10"
header:
  teaser: /assets/images/programming/walutownik/home_page.png
sidebar:
  - title: "Language"
    text: "Python"
  - title: "Additional tags"
    text: "Cypress, Django, Celery, Sentry, Poetry, Kubernates, Docker"
gallery:
---

Currency exchanger was an app created to allow for real-time api based currency exchane, aswell as stock market exchange similarly to an existing app called Revolut.

The app is based on Django and Django Rest Framework written in Python. The schema of database was generated via ORM. The used database was PostgreSQL, while the fronted is Vue.js. The same is described in picture below.

![alt text](../../assets/images/programming/walutownik/app_diagram.png)

We've created basic CRUD operations alongisde signup and login for standard django REST API.

The service allowed for transfers between two users as below aswell as currency conversion.

![alt text](../../assets/images/programming/walutownik/obr3.png)

I've connected external API for currency exchange from Fixer.io allowing Celery with RabbitMQ to probe the data hourly.

The wallets were created via signals when the new user registered and allowed for both currencies and stocks. The wallet allowed for history of operations overview.

![alt text](../../assets/images/programming/walutownik/obr4.png)

The next step was connecting api from Polygon.io that allowed for stock data to be probed. Data looked as below:

![alt text](../../assets/images/programming/walutownik/stocksAPI.png)

After implementing the base functionality we've deployed the app on Google Cloud Platform with Kubernetes. Then we proceeded to test the page via Cypress and Sentry.io. We've tracked the most common errors, response time based on clients and requests and the results are below.

Efficiency of the endpoint via sentry:

![alt text](../../assets/images/programming/walutownik/stocks_sentry.png)

Time for each operation of the endpoint via sentry:

![alt text](../../assets/images/programming/walutownik/stocks_sentry2.png)

Slowest operations of the app:

![alt text](../../assets/images/programming/walutownik/slowest.png)


Most common errors:

![alt text](../../assets/images/programming/walutownik/common_errors.png)


Response time for n amount of clients with m requests each in seconds on x axis:
![alt text](../../assets/images/programming/walutownik/wykres.png)