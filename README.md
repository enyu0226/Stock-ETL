# Stock ETL Pipeline

Implement a Spark ETL pipeline that process the US stock's closing prices over the course of one year. Spark CSV reader is used to extract raw stock data from CSV files with pre-defined schema, and several transformation is applied to the data (such as removing the null row entires, extract only certain column and perform aggregation based on month and take the average on pricing, and to perform count on the number of occurrence closing price is not equaled to adjusted closing price in order to derive possibility for signs of dividend sharing and stock splitting) and the result of the processing is then loaded or stored inside the database that can then be used for further processing or analytic.

Traditional ETL job generally requires heavy and expensive vendor tooling with little support for the needs of specific application. Spark is a general purpose cluster computing solution that can process data in-memory and therefore offers high processing performance compared with batched processing framework such as Hadoop.

## How to run

Start the vagrant vm

`vagrant up`

Get bash shell in vagrant vm

`vagrant ssh`

Set config script permission (you may not need to do this depending on how you execute)

`sudo chmod +x /vagrant/config.sh`

Move to /vagrant directory

`cd /vagrant/config`

Execute config

`./config.sh`

Install Pyspark

`./install_pyspark.sh`

Move to src directory

`cd /vagrant/src`

Execute Spark Application

`spark-submit --driver-class-path /vagrant/lib/postgresql-42.1.4.jar etl.py`

## Data Retrieval

Grabbed data using Python 3.5.2 using scripts in the data_retrieval directory

## Postgres configs (Scripted in config.sh)

You only need these steps if you aren't using the config.sh script to set everything up

- Default username is postgres.
- Default db is postgres

`sudo docker run --name some-postgres -p 5432:5432 -e POSTGRES_PASSWORD=mysecretpassword -d postgres`

`sudo docker exec -it some-postgres bash`

`su postgres`

`psql`

```
CREATE TABLE stock_data (
    symbol text,
    date date,
    open int,
    high int,
    low int,
    close int,
    volume int,
    adj_close int,
    month int,
    year int,
    day int
);
```

```
CREATE TABLE avg_month_close (
    month int,
    average_month_close int
);
```

```
CREATE TABLE adjusted_close_count (
    month int,
    count int
);
```
