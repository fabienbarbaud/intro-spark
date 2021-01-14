---
marp: true
---

<!-- page_number: true -->
<!-- footer: Introduction à Apache Spark (PySpark) -->

Introduction à Apache Spark (PySpark)
===

![](images/spark-logo-trademark.png)

##### Principes de bases

###### par [Fabien Barbaud](fabien.barbaud@timeonegroup.com) - [@BarbaudFabien](https://twitter.com/BarbaudFabien)

---

# Apache Spark

## En résumé

**Spark** (ou **Apache Spark**) est un **framework** open source de **calcul distribué**. Il s'agit d'un ensemble d'outils et de composants logiciels structurés selon une architecture définie. Développé à **l'université de Californie à Berkeley** par AMPLab3, Spark est aujourd'hui un projet de la fondation **Apache**. Ce produit est un cadre applicatif de traitements **big data** pour effectuer des **analyses complexes à grande échelle**.
[Wikipedia](https://fr.wikipedia.org/wiki/Apache_Spark)

---

# Apache Spark

## Rapidité

###### Régression logique sur Hadoop VS Spark
![](images/logistic-regression.png)

100x plus rapide

---

# Apache Spark

## Simple

###### Python

```python
df = spark.read.json("logs.json")
df.where("age > 21")
  .select("name.first").show()
```

Pouvoir rapidement et simplement déployer une application parallélisée de traitement dans les langages Scala, Python, R.

---

# Apache Spark

## Généraliste

###### Stack Spark
![](images/spark-stack.png)

Combiner du SQL, du Streaming, du machine learning, ... en une seule application avec Spark

---

# Apache Spark

## Resilient Distributed Datasets (RDDs)

le RDD est une **collection** d'éléments **partitionnés** et **répartis** entre les nœuds du cluster et **accessible uniquement** en **lecture-seule**.

[RDD](https://spark.apache.org/docs/latest/rdd-programming-guide.html#resilient-distributed-datasets-rdds)

---

# Apache Spark

## Resilient Distributed Datasets (RDDs)

### Paralléliser une collection

```python
data = [1, 2, 3, 4, 5]
distData = sc.parallelize(data)
```

[RDD](https://spark.apache.org/docs/latest/rdd-programming-guide.html#resilient-distributed-datasets-rdds)

---

# Apache Spark

## Resilient Distributed Datasets (RDDs)

### Charger de la donnée externe

```python
distFile = sc.textFile("data.txt")
```

Le fichier peut être en local mais Spark support les systèmes de fichier distribué : *hdfs://*, *s3a://*, ...

[RDD](https://spark.apache.org/docs/latest/rdd-programming-guide.html#resilient-distributed-datasets-rdds)

---

# Apache Spark

## Resilient Distributed Datasets (RDDs)

### Sauvegarder un RDD

```python
sc.saveAsTextFile("data.txt")
```

[RDD](https://spark.apache.org/docs/latest/rdd-programming-guide.html#resilient-distributed-datasets-rdds)

---

# Apache Spark

## Resilient Distributed Datasets (RDDs)

### Exemple simple

```python
lines = sc.textFile("data.txt")
lineLengths = lines.map(lambda s: len(s))
totalLength = lineLengths.reduce(lambda a, b: a + b)
```

[RDD](https://spark.apache.org/docs/latest/rdd-programming-guide.html#resilient-distributed-datasets-rdds)

---

# Apache Spark

## Resilient Distributed Datasets (RDDs)

### Transformations

* map
* filter
* groupByKey
* reduceByKey
* join
* ...

[RDD](https://spark.apache.org/docs/latest/rdd-programming-guide.html#resilient-distributed-datasets-rdds)

---

# Apache Spark

## Resilient Distributed Datasets (RDDs)

### Actions

* reduce
* collect
* count
* take
* ...

[RDD](https://spark.apache.org/docs/latest/rdd-programming-guide.html#resilient-distributed-datasets-rdds)

---

# Apache Spark

## Resilient Distributed Datasets (RDDs)

### Exemple Wordcount

```python
words = sc.textFile("/zeppelin/files/wordcount/conseil-tenu-par-les-rats.txt").flatMap(lambda line: line.split(" "))
wordCounts = words.map(lambda word: (word, 1)).reduceByKey(lambda a,b:a +b)
```

[RDD](https://spark.apache.org/docs/latest/rdd-programming-guide.html#resilient-distributed-datasets-rdds)

---

# Apache Spark

## Resilient Distributed Datasets (RDDs)

### Tester avec Zeppelin

```
$ git clone https://github.com/fabienbarbaud/apache-zeppelin.git
$ cd apache-zeppelin
$ docker-compose up -d
```

http://localhost:8080/#/notebook/2FUS99C8T

---

# Apache Spark

## DataFrames

Un DataFrame est une **collection distribuée** de données organisées en **colonnes nommées**. Il est conceptuellement équivalent à une **table dans une base de données relationnelle**.

[DataFrames](https://spark.apache.org/docs/latest/sql-getting-started.html#creating-dataframes)

---

# Apache Spark

## DataFrames

### Exemple simple

```python
df = spark.read.json("/zeppelin/files/dataframe/MOCK_DATA.json")
df.filter(df['gender'] == "Female")
```

[DataFrames](https://spark.apache.org/docs/latest/sql-getting-started.html#creating-dataframes)

---

# Apache Spark

## DataFrames

### Aussi en CSV

```python
df = spark.read.option("header",True).csv("/zeppelin/files/dataframe/MOCK_DATA.csv")
df.filter(df['gender'] == "Male")
```

[DataFrames](https://spark.apache.org/docs/latest/sql-getting-started.html#creating-dataframes)

---

# Apache Spark

## DataFrames

### Avec un groupement

```python
df.groupBy("gender").count()
```

[DataFrames](https://spark.apache.org/docs/latest/sql-getting-started.html#creating-dataframes)

---

# Apache Spark

## DataFrames

### Mais aussi en SQL

```python
df = spark.read.option("header",True).csv("/zeppelin/files/dataframe/MOCK_DATA.csv")
df.createOrReplaceTempView("people")
sqlDF = spark.sql("SELECT * FROM people")
```

[DataFrames](https://spark.apache.org/docs/latest/sql-getting-started.html#creating-dataframes)

---

# Apache Spark

## DataFrames

### Tester avec Zeppelin

http://localhost:8080/#/notebook/2FUAYT7SC

---

# Apache Spark

## DataFrames

### Exemple Wordcount

```python
from pyspark.sql.functions import split, explode

df = spark.read.text("/zeppelin/files/wordcount/conseil-tenu-par-les-rats.txt")
df.select(explode(split(df.value, '\s+')).alias('word')).groupBy("word").count()
```

http://localhost:8080/#/notebook/2FUZMQVNB

---

# Apache Spark

## Apache Zeppelin

![](images/notebook.png)