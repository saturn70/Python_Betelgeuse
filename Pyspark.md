#### Install
    pip install pyspark

##### Spark SQL
    pip install pyspark[sql]

##### pandas API on Spark
    pip install pyspark[pandas_on_spark] plotly  # to plot your data, you can install plotly together.

##### Spark Connect
    pip install pyspark[connect]

Resilient Distributed Datasets (RDD)

#### SparkSession
    from pyspark.sql import SparkSession
    import pandas as pd
    spark = SparkSession.builder.appName("Sirius_31_Octo").getOrCreate()
    df = pd.read_csv("https://raw.githubusercontent.com/saturn70/Sirius_31_Octo/main/Python/tips.csv")
    print(df.head())

#### Create DataFrame
    Pandas_df = pd.DataFrame({
    'Planet Name' : ['Saturn', 'Earth', 'Jupiter'],
    'Color':['Orange','Blue','Pink'],
    # year,month,date
    'Date': [datetime(2023, 12, 31, 2, 30), datetime(2024, 12, 31, 4, 30), datetime(2025, 12, 31, 6, 30)]
    })

    df = spark.createDataFrame(Pandas_df)
    print(Pandas_df)
    print(" ")
    print(df)
#### Show Dataframe
    df.show()
        +-----------+------+-------------------+
        |Planet Name| Color|               Date|
        +-----------+------+-------------------+
        |     Saturn|Orange|2023-12-31 02:30:00|
        |      Earth|  Blue|2024-12-31 04:30:00|
        |    Jupiter|  Pink|2025-12-31 06:30:00|
        +-----------+------+-------------------+
    
    df.show(2)
    +-----------+------+-------------------+
    |Planet Name| Color|               Date|
    +-----------+------+-------------------+
    |     Saturn|Orange|2023-12-31 02:30:00|
    |      Earth|  Blue|2024-12-31 04:30:00|
    +-----------+------+-------------------+
    only showing top 2 rows

#### When rows are too long to show horizontally.
    df.show(vertical=True)
    
    -RECORD 0--------------------------
     Planet Name | Saturn              
     Color       | Orange              
     Date        | 2023-12-31 02:30:00 
    -RECORD 1--------------------------
     Planet Name | Earth               
     Color       | Blue                
     Date        | 2024-12-31 04:30:00 
    -RECORD 2--------------------------
     Planet Name | Jupiter             
     Color       | Pink                
     Date        | 2025-12-31 06:30:00 

#### Show Columns names
    df.columns
    ['Planet Name', 'Color', 'Date']

#### Show Schema
    df.printSchema()
    root
     |-- Planet Name: string (nullable = true)
     |-- Color: string (nullable = true)
     |-- Date: timestamp (nullable = true)

#### Summary
    df.describe()
    DataFrame[summary: string, Planet Name: string, Color: string]
    
    df.describe().show()
    +-------+-----------+-----+
    |summary|Planet Name|Color|
    +-------+-----------+-----+
    |  count|          3|    3|
    |   mean|       NULL| NULL|
    | stddev|       NULL| NULL|
    |    min|      Earth| Blue|
    |    max|     Saturn| Pink|
    +-------+-----------+-----+
#### Select Column
    df.select("Planet Name").describe().show()
    
    +-------+-----------+
    |summary|Planet Name|
    +-------+-----------+
    |  count|          3|
    |   mean|       NULL|
    | stddev|       NULL|
    |    min|      Earth|
    |    max|     Saturn|
    +-------+-----------+ 
    
### Collect
   
    DataFrame.collect() collects the distributed data to the driver side as the local data in Python. Note that this can throw an out-of-memory error when the dataset is too large to fit in the driver side because it collects all the data from executors to the driver side.

    df.collect()
    [Row(a=1, b=2.0, c='string1', d=datetime.date(2000, 1, 1), e=datetime.datetime(2000, 1, 1, 12, 0)),
     Row(a=2, b=3.0, c='string2', d=datetime.date(2000, 2, 1), e=datetime.datetime(2000, 1, 2, 12, 0)),
     Row(a=3, b=4.0, c='string3', d=datetime.date(2000, 3, 1), e=datetime.datetime(2000, 1, 3, 12, 0))]
    
    In order to avoid throwing an out-of-memory exception, use DataFrame.take() or DataFrame.tail().

#### df.take()
    df.tak(1)
    [Row(Planet Name='Saturn', Color='Orange', Date=datetime.datetime(2023, 12, 31, 2, 30))]
