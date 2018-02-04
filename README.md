# btechproject
steps that we followed:

1. Installing cloudera
2. Added flume service
3. Created twitter application
4. Added flume-sources-1.0-SNAPSHOT.jar to lib of flume
5. Edited flume.conf file to extract twitter data by adding somw twitter api keys.
6. Added flume class path
7. Used following command to extract twitter data to HDFS
	./flume-ng agent -n TwitterAgent -c conf -f /opt/cloudera/parcels/CDH-5.13.1-1.cdh5.13.1.p0.2/lib/flume-ng/conf/flume.conf
8. Finally we got Twitter data in form of JSON.
9. Added Hive Service
10. Added Following Jar to hive command prompt by using command :

         ADD JAR /opt/cloudera/parcels/CDH-5.13.1-1.cdh5.13.1.p0.2/lib/hive/lib/hive-serdes-1.0-SNAPSHOT.jar
11. Created External table :

         CREATE EXTERNAL TABLE tweets (id BIGINT,text STRING) ROW FORMAT SERDE 'com.cloudera.hive.serde.JSONSerDe' LOCATION '/user/flume/tweets';

	*** External table is created where tweets are stored in HDFS.
12. Now For sentiment analysis we used java function ,then created its jar and added to hive

		   ADD JAR /opt/cloudera/parcels/CDH-5.13.1-1.cdh5.13.1.p0.2/lib/hive/lib/hive-sentiment_analysis

13. Created Function using hive query:
	
	CREATE TEMPORARY FUNCTION sentiment AS 'com.orienit.sentimentanalysis.hive.udf.SentimentUdf';

14.  now applied this function on table created in step 11:

	SELECT text, sentiment(text) FROM kalyan.tweets;

	*** Now we got output on command line as (-1 for negative sentiment,0 for neutral and 1 for posituve sentiment).

15. Now for storing these output as text and its sentiment we have created a table :

	CREATE TABLE IF NOT EXISTS sentimenttweets (text string, sentiment int) LOCATION '/user/hive/output';

16. Inserted data in it using sentiment function : '

	INSERT OVERWRITE TABLE sentimenttweets SELECT text, sentiment(text) FROM project.tweets;      (project name of our database)



*** In this way we have got command line output, we are also attaching its screenshot .***



