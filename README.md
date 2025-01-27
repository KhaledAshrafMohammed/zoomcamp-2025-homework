# zoomcamp-2025-homework
# Homework 1: Docker, SQL, and Terraform

## Question 1: Docker First Run
To check the pip version in the python:3.12.8 image:
docker run -it --entrypoint bash python:3.12.8
pip --version

## Question 2: Docker Networking
The pgadmin service should connect to the Postgres database using:
Hostname: db
Port: 5432

## Question 3: Trip Segmentation Count
SQL query to count trips in specific distance ranges for October 2019:
SELECT
    COUNT(CASE WHEN trip_distance <= 1 THEN 1 END) AS up_to_1_mile,
    COUNT(CASE WHEN trip_distance > 1 AND trip_distance <= 3 THEN 1 END) AS between_1_and_3_miles,
    COUNT(CASE WHEN trip_distance > 3 AND trip_distance <= 7 THEN 1 END) AS between_3_and_7_miles,
    COUNT(CASE WHEN trip_distance > 7 AND trip_distance <= 10 THEN 1 END) AS between_7_and_10_miles,
    COUNT(CASE WHEN trip_distance > 10 THEN 1 END) AS over_10_miles
FROM green_tripdata_2019-10
WHERE lpep_pickup_datetime >= '2019-10-01' AND lpep_pickup_datetime < '2019-11-01';
Answer:
The correct counts are: 104,802; 198,924; 109,603; 27,678; 35,189.

## Question 4: Longest Trip for Each Day
SQL query to find the pickup day with the longest trip distance:
SELECT DATE(lpep_pickup_datetime) AS pickup_day, MAX(trip_distance) AS max_distance
FROM green_tripdata_2019-10
GROUP BY pickup_day
ORDER BY max_distance DESC
LIMIT 1;
Answer:
The pickup day with the longest trip distance is 2019-10-26.

## Question 5: Three Biggest Pickup Zones
SQL query to find the top 3 pickup zones with over 13,000 in total_amount for 2019-10-18:
SELECT z."Zone", SUM(g."total_amount") AS total_amount
FROM green_tripdata_2019-10 g
JOIN taxi_zone_lookup z ON g."PULocationID" = z."LocationID"
WHERE DATE(g."lpep_pickup_datetime") = '2019-10-18'
GROUP BY z."Zone"
HAVING SUM(g."total_amount") > 13000
ORDER BY total_amount DESC
LIMIT 3;
Answer:
The top pickup zones are East Harlem North, Morningside Heights.

## Question 6: Largest Tip
SQL query to find the drop-off zone with the largest tip for passengers picked up in "East Harlem North" in October 2019:
SELECT z2."Zone" AS dropoff_zone, MAX(g."tip_amount") AS max_tip
FROM green_tripdata_2019-10 g
JOIN taxi_zone_lookup z1 ON g."PULocationID" = z1."LocationID"
JOIN taxi_zone_lookup z2 ON g."DOLocationID" = z2."LocationID"
WHERE z1."Zone" = 'East Harlem North'
AND DATE(g."lpep_pickup_datetime") >= '2019-10-01'
AND DATE(g."lpep_pickup_datetime") < '2019-11-01'
GROUP BY dropoff_zone
ORDER BY max_tip DESC
LIMIT 1;
Answer:
The drop-off zone with the largest tip is JFK Airport.

## Question 7: Terraform Workflow
The correct sequence of Terraform commands is:
Download provider plugins and set up backend: terraform init
Generate proposed changes and auto-execute the plan: terraform apply -auto-approve
Remove all resources managed by Terraform: terraform destroy