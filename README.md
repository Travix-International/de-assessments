The goal of this assessment is for you to show us your data engineering experience, from designing to implementing a solution.

This exercise consists of multiple phases. Ideally the end result covers all phases, however when you feel you're unable to complete a certain step you can pass and try to continue on the next.

## Assessment

We'd like to analyze flight information for a set of transactions which are shared as data input files. Because we prefer sustainable applications and data pipelines we encourage you to think about scalability and maintainability. The input files are relatively small, but imagine we receive a tenfold of this data per day when designing a solution. 

Your mission is to design and implement one or multiple applications or data pipelines to consume these files, store the data to an analytical store and deliver (some of) the requested insights. 

The obvious and most simple solution would be to load data into a spreadsheet or data platform of your choice manually, however we would be more impressed with a solution which includes technologies like Kubernetes, Docker, message queue or message broker (i.e. RMQ, Kafka; *we use Google Pub/Sub*), data processing (i.e. Spark; *we use Google DataFlow/Apache Beam*), orchestration (*we use Apache Airflow*), storage layer (i.e. HDFS, Redshift; *we use Google BigQuery*)

We've described a possible approach in the phases below

### Phase 1
Write a small applications which reads the data from all input files and published the data to a message broker like Kafka or Pub/Sub. Use a docker container to run the application.

### Phase 2
Write a small data processing pipeline to consume data from the message broker and store/ingest data to a storage layer of choice, i.e. BigQuery. Add any additional metadata you think is useful and add transformations or (de)normalization steps.

### Phase 3
Write the necessary SQL statements to gain the required insights.

## Requirements

* A simple design/diagram explaining your solution
* A link to your personal github repo or a zip-file containing (pseudo-) code files in your language of choice (we prefer Python/Java), SQL scripts, Dockerfiles, Jupyter notebooks etc..
* At least the following insights, preferably visualized by plotting charts:
  * From which Country are most transactions originating? How many transactions is this?
  * What's the split between domestic vs international transactions? 
  * What's the distribution of number of segments included in transactions?
  * ...

## Input Files
All input files are in JSON format and gzipped, the schema's for them are described below.

The transactions table has the following (nested) structure:

| Field name | Type |
--- | --- | ---
| UniqueId | STRING |
| TransactionDateUTC | STRING |
| Itinerary | STRING | 
| OriginAirportCode | STRING |
| DestinationAirportCode | STRING |
| OneWayOrReturn | STRING |
| Segment | RECORD |
| Segment.DepartureAirportCode | STRING |
| Segment.ArrivalAirportCode | STRING |
| Segment.SegmentNumber | STRING |
| Segment.LegNumber | STRING |
| Segment.NumberOfPassengers | STRING |

The various AirportCode fields join to the location table.

---

The locations table has the following structure:

| Field name | Type |
--- | --- | ---
| AirportCode | STRING |
| CountryName | STRING |
| Region | STRING |


## Useful resources
The following tools and resources can be of use while creating and implementing a solution

* Apache Airflow, this repo has a nice starting point: https://github.com/puckel/docker-airflow

    Since the tasks are simple, you can use LocalExecutor:
    > docker-compose -f docker-compose-LocalExecutor.yml up -d

## Bonus

* When your code is deployable or runs locally
* Any form of unit/E2E testing
* Additional metadata or data lineage information
* Any form of automation
* Impress us!