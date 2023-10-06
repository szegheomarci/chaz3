# ChaZ3 App

Chaz3 is a car availability tracker application that I have configured for my own use to track BMW Z3 availability. It collects data from classifieds and processes them to make it useful to answer analytical questions. (How many cars are available, what is there average price considering customizeable filtering.)

## Architecture
ChaZ3 is built from microservices. It serves as a proof of concept for DevOps activities. For this reason the architecture is over complicated in some cases. Otherwise it does what most other applications do:  
 * Collect data
 * Process data
 * Present data
 * Support functions.

These services can be deployed in various ways (standalone docker images, AKS, etc) and some of the services are interchangeable.
The following diagram gives an overview of the services:

 
## Data collection
Data collection is managed by the following services:
 * car-ads
 * car-db-loader
 * Kafka

### car-ads
Car-ads can be configured to collect data from car adveretisement sites. The collected data can be output in many ways, ChaZ3 uses Kafka topic to which this service broadcasts.  
Stage | Link
-|-
Source code: | gitlink
Reviews: | gerrit link
Docker: | docker link
Helm: | helm link

### car-db-loader
Car-db-loader subscribes to 

## Data processing

## Data presentation

