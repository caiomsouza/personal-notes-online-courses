### BerkeleyX: CS105x Introduction to Apache Spark

Course Overview:<BR>
https://courses.edx.org/courses/course-v1:BerkeleyX+CS105x+1T2016/fbe63aa3c95948e3912fa128aedec27d/<BR>

Welcome Page:<BR>
https://courses.edx.org/courses/course-v1%3ABerkeleyX%2BCS105x%2B1T2016/info

Databricks and UC BerkeleyX<BR>
https://databricks.com/<BR>
https://amplab.cs.berkeley.edu/<BR>

We will use PySpark (the Python API for Apache Spark).<BR>

Python Tutorials to prepare to the Apache Spark Course<BR>
http://ai.berkeley.edu/tutorial.html#PythonBasics<BR>
http://www.mypythonquiz.com/<BR>

We will use Databricks Community Edition to the course exercises: <BR>
https://accounts.cloud.databricks.com/registration.html#signup/community <BR>

The course has a FAQ.<BR>
https://courses.edx.org/courses/course-v1:BerkeleyX+CS105x+1T2016/wiki/BerkeleyX.CS105x.1T2016/<BR>

Videos and Slides Notes:<BR>

Awesome Public Datasets<BR>
https://github.com/caesar0301/awesome-public-datasets<BR>

My account to Databricks CE:<BR>
https://community.cloud.databricks.com/login.html?user=caio%40it4biz.com.br

###RDD vs Data Frame

RDDs are lower-level and based upon the notion that you model incoming data as a collection, and you do functional transformations over that collection, producing new collections. For example:<BR><BR>

new_rdd = rdd.flatMap(lambda text: text.split())<BR><BR>
These kinds of transformations are powerful, but they have issues, such as:<BR><BR><BR><BR>

It's possible to build chains of transformations that are quite inefficient, and Spark cannot fix that for you.
RDD transformations in Python are dramatically slower than in Scala. Spark uses C Python, not Jython. So, when it needs to run a Python lambda, the Executor that needs to run the lambda (which is a JVM process) ships the partition to a Python process, which deserializes (unpickles) the data, runs the Python lambda, reserializes the resulting transformed data, and sends it back to the parent Executor. This is slow.<BR><BR><BR><BR>

DataFrames are different. First, they're higher level. With RDDs, you figure out what you want to do, and you express how to do it in terms of transformations on RDDs. With DataFrames, you figure out what you want to do, you express that in terms of SQL or a SQL-like query API, and Spark figures out the best way to implement it (the "how") in terms of RDDs.<BR><BR>

DataFrames have several advantages:<BR><BR>

They suffer no language-dependent slowdown. No matter what language you use, you're merely putting together a query. The query is optimized and implemented on the JVM, even if you used R or Python to express it.
Spark has a query optimizer (called Catalyst) that can do some pretty fabulous things. Here are some examples:
It consolidates filter (i.e., "where clause") operations, where it makes sense to do so.
It will move a filter in front of a join, if that will speed things up, even if you expressed the filter operation after the join when you built the query. (With RDDs, the operations happen in whatever order you specify them, even if you put things in a really inefficient order.)
It can push some operations down into the data sources. For example, suppose you select only 5 columns of the 1000 columns in a Parquet data source. Parquet is a columnar source, and you can efficiently decide not to read certain columns. In this case, Catalyst will arrange to materialize only those 5 columns in memory. Contrast that with reading a CSV file: To discard 995 columns from a CSV file,  you still have to read the whole row, parse it, and extract the columns you want. With Parquet, those 995 columns you don't want never get read at all. Another example: Suppose you do a filter operation on a DataFrame that happens to be reading from MySQL. Catalyst knows you're reading from an RDBMS, and it will push that filter operation down into MySQL (as a SQL WHERE clause). Thus, the filtering is done in the database, and the data that is filtered out never makes it into memory.<BR><BR>

In general, these days, we always recommend that you write your Spark jobs using DataFrames, not RDDs, unless you can make a really compelling case for using RDDs.<BR><BR>

(Initial answer by Brian Clapper, Databricks)<BR><BR>

###Which version of Spark we would be using for this Xseries? Can we use 2.0?
This Xseries course material has been based on Spark version 1.6.1

###Using Spark along with numpy / scikit algorithms
There was a very interesting talk at one of last years Spark Summits, which may give you some pointers:

https://spark-summit.org/eu-2015/events/3-trillion-app-recommendations-with-less-than-100-lines-of-spark-code-in-less-than-25-minutes/

However, it uses the older api and may be a bit difficult to translate to the newer dataframe api

In addition to the Spark Summit EU presentation, a good webinar that talks about integration numpy and scikit-learn is the webinar: Apache Spark MLlib: From Quick Start to Scikit-Learn.

The webinar includes notebooks that utilize numpy, matplotlib, pandas, ggplot, as well as a separate one on sklearn integration.


###My course tasks:
Test notebooks 1:<BR>
https://databricks-prod-cloudfront.cloud.databricks.com/public/4027ec902e239c93eaaa8714f173bcfc/6073324324262249/4371048590712335/6971386257103033/latest.html<BR><BR>

Test notebooks 2:<BR>
https://databricks-prod-cloudfront.cloud.databricks.com/public/4027ec902e239c93eaaa8714f173bcfc/6073324324262249/4371048590712329/6971386257103033/latest.html<BR>
