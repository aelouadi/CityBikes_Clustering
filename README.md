
# CityBikes Clustering

Clustering of citybikes stations based on the location. 

## Build status

Source code is available under ./home/marksus/Téléchargements/TestClustering, it's tuned to run under local spark node, please modify file build.sbt by addind ' % "provided" to each line of required component 

## Requirements

* [Java 8](https://www.java.com/fr/download/faq/java8.xml) or higher, you can also execure apt-get directely on terminal in a unix environnement (sudo apt-get install jdk-default), please make sure to set Environnement varaiable JAVA_HOME by typing "echo JAVA_HOME" or "java version"
* [Scala 2.12.8](https://www.scala-lang.org/download/2.12.8.html)
* [SBT 1.0.3](http://www.scala-sbt.org/download.html) or higer
* [Spark 2.4.0](https://www.apache.org/dyn/closer.lua/spark/spark-2.4.0/spark-2.4.0-bin-hadoop2.7.tgz)



## Configuration

Before executing the code, you have to complete the configuration file available in **./src/resources/application.properties**.

    input.data=
    output.dir=
    model.dir=

* **input.data** Path to the file Brisbane_CityBike.json
* **output.dir** The directory that will be used to store clustered data
* **model.dir** Directory that will be used to store the model generate in case you will need to reuse it

In Our case, those directories are included within compressed delivery directory 

## Running without installing Spark

The project could be run locally without installing Spark. This will use an "emulated" Spark wish is included in the dependencies of the project.

To do this, you should change the content of the file **build.sbt** like following :

    name := "TestClustering"

version := "0.1"

scalaVersion := "2.12.8"

val sparkVersion = "2.4.0"

libraryDependencies ++= Seq(
  "org.apache.spark" %% "spark-core" % sparkVersion,
  "org.apache.spark" %% "spark-sql" % sparkVersion,
  "org.apache.spark" %% "spark-mllib" % sparkVersion,
)

What we did here, is removing the "provided" option. This will force sbt to include Spark dependencies when compiling the project. 

You can then compile files : 

    sbt clean compile
    
Then run the job :

    sbt run

## Running on Spark

To build the project, run : 

    sbt clean assembly
    
This will produce **/home/marksus/Téléchargements/TestClustering/target/scala-2.12/testclustering_2.12-0.1.jar** which contains the compiled project.

Then you can submit the job using **spark-submit** :

  ./bin/spark-submit --class "com.citybikes.clustering.Run" --master local "Your Path"/TestClustering/target/scala-2.12/testclustering_2.12-0.1.jar

_Make sure that you put the correct path to the JAR file._

## Results

It seems to me that the most appropriate number of clusters for this use case is 5 (0 to 4).

