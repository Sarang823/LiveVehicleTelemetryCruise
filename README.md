# LiveVehicleTelemetryCruise
In this project, we focus on the streaming analysis of vehicle telemetry data using the CDC (Change Data Capture) approach with the Debezium Kafka Connector.
Specifically, we retrieve vital information from the RTD-Denver bus transport API, including vehicle ID, timestamp, latitude, longitude, and bearing. Our primary objective is to derive real-time insights from this data, such as bus speed and various derived variables like average speed and distance traveled.
## Challenges Addressed:

#### Data Retrieval: 
The RTD-Denver website periodically publishes vehicle data in the form of protobuf files (.pb), with updates occurring every 30 seconds. Our task involves deserializing this data and efficiently storing it in our local relational database.

#### Real-Time Speed Calculation: 
To continuously calculate the real-time speed of the vehicles, we employ a change data capture (CDC) pipeline utilizing the Debezium Kafka Connector. Whenever the vehicle speed surpasses a predefined threshold, alerts are triggered, with notifications sent via Telegram.
This project aims to harness the power of vehicle telemetry data to monitor and respond to critical events in real time.
The project-architecture diagram looks like :

![vehicle_telemetry_demo](https://github.com/Sarang823/LiveVehicleTelemetryCruise/assets/133379507/9739ea12-3581-4135-a92d-c2590874fc44)



## Step-by-Step Approach:

### A] Initial Setup:

Connect the RTD-Denver API to PostgreSQL. And via the Debezium Connector and Kafka-Conect subsequently, route data to Kafka cluster, and then to the destinations like: MongoDB, and Telegram Alerts, as well as S3.
**Note** : To effectively use the api-key first sign up to the **https://notifications.rtd-denver.com/s/login/SelfRegister** and use those credentials in the code. for the code refer : **source_data_from_api.ipynb**

Begin by setting up the Debezium Kafka Connector and Kafka Connect within a Docker environment. Refer to the **"docker-compose.yml"** file for the necessary configuration.
Note: Inside the Docker container, **Kafka uses port 9092, while outside the Docker, it employs port 29092.**
To initiate all containers, use the command: **"docker-compose up -d."**


![kafka_with_docker](https://github.com/Sarang823/LiveVehicleTelemetryCruise/assets/133379507/d9e0a954-2d90-4184-af01-54e3085c1a8b)



Configure the connector settings. You can utilize various tools such as Postman or Visual Studio Code (VSCode). Here, we use the Thunderclient extension within VSCode.
Make an API call to the connector's API endpoint **"https://localhost:8083/connectors."** To set up a new connector, send a POST request with the connector definition payload in the request body. Refer to the "source-productcategory-connector.json" file for the code. Make sure to update the database IP in the code to match your **device's IP address**. For the above process refer to **source-productcategory-connector.json** file.


After completing the setup, access "localhost:9000" to create the Kafka cluster.
Adjust the PostgreSQL **"wal_level" setting to "logical"** in the postgresql.conf file and restart the PostgreSQL service on the server. Logical replication operates using a publish-subscribe model.

![wal_level_ss](https://github.com/Sarang823/LiveVehicleTelemetryCruise/assets/133379507/427626a9-10d7-4dfb-a15a-dd0d00eb7075)



### B] Steps:

1.To process data received in the **".pb"** file format from the RTD-Denver API and store it in the local PostgreSQL database, refer to the code in **"source_data_from_api.ipnyb"**.

Note: To utilize the **"gtfs_realtime_pb2"** class and its methods, download and place the **"gtfs_realtime_pb2.py"** file in the project location of your IDE or Jupyter Notebook for importing the class. The file is provided with your project resources.

2.For Kafka Connect processing, sending Telegram alerts, and storing data in S3 and MongoDB, consult the **"final_kafka_consumer_to_destinations.ipynb"** file for the relevant code.

3. For telegram alerts, create a telegram bot by visiting *'@botfather'*

This step-by-step approach outlines the setup and execution of the project's data processing and analysis pipeline, connecting various components to achieve the desired outcomes.
