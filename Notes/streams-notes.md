

ksql-datagen quickstart=users format=avro topic=krish_users maxInterval=5000
ksql-datagen quickstart=pageviews format=avro topic=krish_pageviews maxInterval=5000



CREATE STREAM krish_users_stream (userid varchar, regionid varchar, gender varchar) WITH \
(kafka_topic='krish_users', value_format='AVRO');


CREATE STREAM krish_pageviews_stream (userid varchar, pageid varchar) WITH \
(kafka_topic='krish_pageviews', value_format='AVRO');


SELECT userid, regionid, gender from krish_users_stream;
SELECT userid, pageid from krish_pageviews_stream;



CREATE STREAM krish_pageviews_enriched_stream AS \
SELECT krish_users_stream.userid AS userid, pageid, regionid, gender \
FROM krish_pageviews_stream \
LEFT JOIN krish_users_stream \
  WITHIN 1 HOURS \
  ON krish_pageviews_stream.userid = krish_users_stream.userid;

SELECT userid, regionid, gender, pageid from krish_pageviews_enriched_stream;


CREATE TABLE krish_pageviews_region_table \
        WITH (VALUE_FORMAT='avro') AS \
        SELECT gender, regionid, COUNT(*) AS numusers \
        FROM krish_pageviews_enriched_stream \
        WINDOW TUMBLING (size 30 second) \
        GROUP BY gender, regionid \
        HAVING COUNT(*) >= 1;
