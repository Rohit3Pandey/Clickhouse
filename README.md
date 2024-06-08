# Rohit-DataZip-Assignment

### Prerequisites
1. Helm should be available locally.
2. Minikube/Kind kuberenetes Cluster should be up and running.

### Helm command to create chart
```bash
helm create clickhouse
```
Delete everything and clone this repo using git clone command.

before running the below mentioned command make sure, 
-kuberentes cluster is up and runnimg
-naviagte to parent directory to not to encounter any error in running helm install command.

### Helm command to install clickhouse 
```bash
helm install clickhouse clickhouse/
```
This will run clickhouse-server and clickhouse-client on your kubernetes cluster.
 
### use kubectl to verify if clickhouse-server and clickhouse-client pods are up and running
```bash
kubectl get pods
```
this will return two pods

### exec into client-pod of clickhouse
```bash
kubectl exec -it clickhouse-client -- bash
```
this will open an bash interactive terminal for you. 

```bash
clickhouse-client --host clickhouse-clickhouse --port 9000
```
Congrats you have established connection with clickhouse-server. 

### SQL COMMANDS
```SQL
CREATE DATABASE MY_CLICK_DB;
USE MY_CLICK_DB;
SELECT currentDatabase();
```
![SQL](https://github.com/Rohit3Pandey/Images/blob/main/Screenshot%20(106).png)
Here, you have created and going to use a different db than default. 

### Now let's create a table and insert data based on assignment
```SQL
CREATE TABLE dz_test
(
    `B` Int64,
    `T` String,
    `D` Date
)
ENGINE = MergeTree
PARTITION BY D
ORDER BY B;
```
![SQL](https://github.com/Rohit3Pandey/Images/blob/main/Screenshot%20(107).png)
```SQL
INSERT INTO dz_test
SELECT number, CAST(number AS String), '2024-05-05' AS D
FROM numbers(1000000000); -- 1e9 is equal to 1 billion, use a smaller value for testing
```
![SQL](https://github.com/Rohit3Pandey/Images/blob/main/Screenshot%20(108).png)
P.S this data is of 8 GB.

### Below command will return 10 rows from 1 billion rows it has inserted in the db
```SQL
SELECT * FROM dz_test LIMIT 10;
```
![SQL](https://github.com/Rohit3Pandey/Images/blob/main/Screenshot%20(109).png)
### DO NOT FORGET TO CLEAR EVERYTHING 
```SQL
DROP DATABASE MY_CLICK_DB;
```
### To check whether everything is clear or not 
```SQL
SELECT formatReadableSize(sum(data_compressed_bytes)) AS total_size
FROM system.parts
WHERE table = 'dz_test';
```
### At last use helm uninstall
```bash
helm uninstall clickhouse
```
This will terminate the pods!!!

### Use below mentioned links to understand how to setup hot and cold volumes with move factor. 
https://altinity.com/blog/2019-11-27-amplifying-clickhouse-capacity-with-multi-volume-storage-part-1

https://altinity.com/blog/2019-11-29-amplifying-clickhouse-capacity-with-multi-volume-storage-part-2

