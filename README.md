Using the docker image (https://github.com/puckel/docker-airflow) please complete the following tasks.

## Task 1 - Getting Airflow up and running
Since the tasks are simple, you can use LocalExecutor.
docker-compose -f docker-compose-LocalExecutor.yml up -d

As a test, we expect you to get the docker container up and running.
Once you are able to use it, as a small task, use airflow repository as a source and the same database as a target to create a table in the following structure:
 - table_name (string): table name in the airflow repository
 - row_number (int): number of rows within that table

There are 22 tables in the repository (dag_run, dag, connection, job, sla_miss, etc.).

The deliverables of this task are 
 - a dag file for the pipeline
 - a csv of the data of the final table

| table_name   | row_count |
| ------------ | --------- |
| dag_run      | 0         |
| dag_stats    | 0         |
| import_error | 0         |
| known_event  | 0         |
| ...          |           |


## Task 2 - Cleansing
For this task, your input is the data_cleansing_input.json file. The data is in json format.

This data consists of trips. A trip is identified by a TripId.
Each trip is formed of Legs. A Leg defines an origin and a destination. Each trip has at least one Leg. You can consider this as the travel direction. Each Leg has at least one Segment.
Segments are all parts from the leg, including all intermediate stops until the destination is reached.

The order with the TripId 'a1' has 2 Legs. The origin of the first Leg is MEX and the destination is MNL. The second Leg is the return.
In this example, each Leg consists of 2 segments where CAN is the intermediate stop.

| TripId | Itinerary               | OneWayOrReturn | DepartureAirport | ArrivalAirport | Leg | SegmentOrder | TransactionDateUTC          |
| ------ | ----------------------- | -------------- | ---------------- | -------------- | --- | ------------ | --------------------------- |
| a1     | MEX-CAN-MEX-CAN-MNL-CAN | Return         | MEX              | CAN            | 1   | 1            | 2019-03-14 03:58:37.343 UTC |
| a1     | MEX-CAN-MEX-CAN-MNL-CAN | Return         | CAN              | MNL            | 1   | 2            | 2019-03-14 03:58:37.343 UTC |
| a1     | MEX-CAN-MEX-CAN-MNL-CAN | Return         | MNL              | CAN            | 2   | 1            | 2019-03-14 03:58:37.343 UTC |
| a1     | MEX-CAN-MEX-CAN-MNL-CAN | Return         | CAN              | MEX            | 2   | 2            | 2019-03-14 03:58:37.343 UTC |

The correct order of those flights can be calculated by ordering with respect to Leg and SegmentOrder fields.
As you can see from the sample data above, the itinerary column within the data is produced erroneously. The correct Itinerary for this order should be MEX-CAN-MNL-CAN-MEX.

We expect you to give us a list of trips (TripId) that have erroneous Itinerary information. 
In addition to that, we also want you to check whether OneWayOrReturn field is correct or not. A trip is a "Return" if the initial DepartureAirport is equal to the final ArrivalAirport. Otherwise is should be a "One Way" trip.

You may need to deduplicate the data as well. For this, you can consider TripId, Leg and SegmentOrder to be unique.

We expect you to have a clean, readable code with tests and comments where necessary.

The deliverables of this task are
 - a dag file for the pipeline
 - a csv file of TripId's that has wrong Itinerary or OneWayOrReturn information and a column stating that reason

| TripId | Reason         |
| ------ | -------------- |
| a1     | Itinerary      |
| z5     | Itinerary      |
| a4     | OneWayOrReturn |
| h6     | Itinerary      |
| ...    |                |

Bonus: There can be several processing methods to accomplish this task eg. Beam, Spark, SQL or even basic Python libraries. There are bonus points if you implement a second method.
