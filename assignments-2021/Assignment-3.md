# CS6650 Fall 2021  Assignment 3

## Adding Persistence
This assignment builds on assignment 2. The main aim is to persist the tuples generated by the your client.

It's likely that the AWS restrictions on the number of instances you can start will mean you can't use the load balancer. Feel free to increase capacity of servers - just report what you used for gathering result sand watch your $$s!

## Step 1 - Adding a Skier Microservice
In assignment 2, your consumer created an in-memory hash map to skier and lift ride information. 

Your first task is to modify the consumer to persist the results to a database. 

You are free to choose any database you like. AWS RDS instances like MySQL, DynamoDB, MongoDB, Redis - all good candidates. 

The data model you design in the database should enable queries like:

* "For skier N, how many days have they skied this season?"
* "For skier N, what are the vertical totals for each ski day?"
* "For skier N, show me the lifts they rode on each ski day"

Whatever database you choose, the challenge is to write to the database ideally as fast as you can consume messages from RabbitMQ. This may be challenging based on the EC2 resources you choose, so experiment required. Run some experiments. 

Test this configuration by reporting the same results as assignment 2 for 128, 256 clients. Feel free to empty the database between tests. It might help ;)

## Step 2 - Adding  a Resorts Microservice

Next, add a new consumer and database for storing data pertinent to the ski resort. The consumer should ingest the same data as the Skier Microservice. The data model should be designed to answer questions like:

- "How many unique skiers visited resort X on day N?"
- "How many rides on lift N happened on day N?"
- "On day N, show me how many lift rides took place in each hour of the ski day"

Again, test to see if you can write to the database as quickly as the data is published to RMQ.

Test this configuration **(without skier microservice)** by reporting the same results as assignment 2 for 128, 256 clients

## Step 3 - Experiments

First, run tests for 128 and 256 with both microservices and see what happens. 

It's likely you will see significant backlogs in your queues, servlet and maybe even consumers.

If you do, you have two choices:

1. Increase capacity - this means deploying more than free tier instances. Watch the $$s. 
2. Introduce throttling - you could do this e.g. in the client by introducing a throughput-based circuit breaker with exponential backoffs in client POSTs and/or RMQ posts/configuration

You don't have to do both. But your aim is to try and deliver more stable throughput and eliminate client errors that may occur when no mitigation measures were used.



## Submission Requirements
Submit your work to Canvas Assignment 3 as a pdf document. The document should contain:

1. The URL for your git repo. Create a new folder for your Assignment 3 server code
1. A short description of your database designs and deployment topologies on AWS
1. Test runs (command windows, RMQ management windows showing queue size, send/receive rates)  128, 256 client threads for Step 1 nd 2.
1. Test run for 128, 256 clients (command windows, RMQ management windows showing queue size, send/receive rates)  with both microservices consuming and writing to the database. 
1. A brief explanation of your mitigation startegy and results with 128. 256 clients to show their effects. Hopefully positive but negative is fine with good analysts!

## Grading:
1. Server and database implementations working (5 points)
1. Database design description (5 points) 
1. Step 1 Results (10 points) - are queue lengths short? 
1. Step 2 Results (10 points) - are queue lengths short? 
1.  Step 3 - (20) Comparison of results with no mitigation startegy and results with mitigation strategy in place. Analyze the difference.

# Deadline: 11/23 11.59pm PST 

https://docs.aws.amazon.com/prescriptive-guidance/latest/modernization-data-persistence/cqrs-pattern.html)

