# Airflow 1.10 timezone chnage bug replication setup

We are running Airflow 1.10, we have scheduled ~50 DAGs. We are using local timezone, last night when during timezone change (DST change, Sunday 3AM->2AM) scheduler stopped scheduling the dags. Scheduler processes were up, but no jobs were scheduler.
Restart didn't help. We had to set up Airflow on clean database. 

## Steps to reproduce

change system clock to 28 Oct 1.45 CEST (UTC +02.00)

```
docker-compose up -d
```

Go to

```
localhost:8080
```

and turn on the dag (there is only one)

Dag should start to be invoked every 5 minutes. After ~1 hour when time is changed automatically to 28 Oct 2.00 CET (UTC +01.00) there will be no any further executions.  

One can log into container and check the status, logs, etc:

```
docker exec -i -t airflow /bin/bash
```