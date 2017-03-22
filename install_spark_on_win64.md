# Install Spark on windows (x64)

### Basic information
* Spark 2.1.0
* Hadoop 2.7.1 (only use winutils.exe)
* Scala 2.12.1
* JDK 1.8 x64
* Python 2.7 x64 (for pyspark)

### 1. Install JDK 1.8
> JDK installation is very easy and you can find lots of tutorials in google, so we ignore it.

### 2. Install Scala
* Download and install Scala from offical website  
  [http://www.scala-lang.org/download/](http://www.scala-lang.org/download/)


* Add Scala environment variables
> - Add `%SCALA_HOME%` point to your installation folder path
> - Add `%SCALA_HOME%\bin` to `Path` env 

* Test Scala
>>  Open windows shell (cmd or powershell) and execute "`scala -version`", some information like below should be printed:  
>>>`Scala code runner version 2.12.1 -- Copyright 2002-2016, LAMP/EPFL and Lightbend, Inc.`

### 3. Install Spark