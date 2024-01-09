<!-- TOC start (generated with https://github.com/derlin/bitdowntoc) -->

- [Architecture](#architecture)
- [Data collection](#data-collection)
- [Processing](#processing)
- [Presentation](#presentation)
- [Support functions](#support-functions)
- [Links](#links)

<!-- TOC end -->
# ChaZ3 App

Chaz3 is a car availability tracker application that I have configured for my own use to track BMW Z3 availability (hence the name). It collects data from classifieds and processes them to make it useful to answer analytical questions, such as:
 * How many cars were advertized each week?
 * How has the average price changed over the years?
 * ... (Other graphs and maps can be created based on the data collected over the years.)

 The results can be filtered considering color, engine, manufacturing date and other options.
 Basically I did not want to rely on old wifes' tales on when is the good time to buy a roadster, in season, before season, or after season. (When is the season anyway???) Also, I see it for myself when the 'prices will only go up from now'.

This is **my hobby project** that I develop as a practical excercise accompanying my **DevOps** studies. It is very useful to test and demonstrate concepts and practices. For these reasons the architecture and other solutions are over-complicated in some cases. The **DevOps strategy is decribed in [this other document](devops_strategy.md)**.

## Deployment
The application can be deployed in various ways. This repository is going to host the description to some of the options.

## Architecture
ChaZ3 is built from microservices. For easier understanding, the services can be assigned to the following functional layers:

 * Data collection
 * Processing
 * Presentation
 * Support functions


The following diagram gives an overview of the services:  
<img src="images/architecture.drawio.svg" />
 
## Data collection
Data collection is managed by the following services:
 * car-ads
 * car-db-loader

### car-ads
Car-ads can be configured to collect data from car adveretisement sites. The collected data can be output in many ways, including files and Kafka topics to which this service broadcasts.  
It is typically enough to run this app once a day.

### car-db-loader
Car-db-loader reads the collected data from a file or a Kafka topic. It loads the data in a SQL database and also performs related updates.

## Processing
Processing tasks are handled by the ad-aggregator service.

### ad-aggregator
Ad-aggregator querys the SQL database to aggregate data that is efficiently presentable. It writes the aggragated data in a NoSQL database.

## Presentation
The presentation layer is handled by the chaz3-gui service.

### chaz3-gui
Chaz3-gui is a web-based app that allows the user to interact with the collected data. It generates graphs to analyze tendencies in the availability of cars with filtering options.

## Support functions
Microservices supporting the functional services:
 * Kafka
 * alarm-handler
 * SQL database
 * NoSQL DB

### Kafka
Kafka is the mesaging service used for microservice communication in ChaZ3. It publishes collected car data and logging messages. Third party services, such as [cloudkarafka](https://www.cloudkarafka.com/) can be a cost saving choice instead of self deployment.

### MySQL database

### Mongo DB

## Links

The GUI is not published yet.

Services can be accessed as listed in the table below.

Service | Language | Source code | Reviews | Docker | Helm
-|-|-|-|-|-
**car-ads** | java | [git](https://github.com/szegheomarci/car-ads) | [gerrit](https://review.gerrithub.io/q/project:szegheomarci/car-ads) | docker link | helm link
**car-db-loader** | python |  [git](https://github.com/szegheomarci/db-loader) | gerrit link | docker link | helm link
**ad-aggregator** | python | gitlink | gerrit link | docker link | helm link
**chaz3-gui** | java+Spring+Thymeleaf | [git](https://github.com/szegheomarci/chaz3-gui) | gerrit link | docker link | helm link
  
