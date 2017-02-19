##Bryan Nguyen
###Duc Thanh Tran
CS157B Notes Homework 2

---

Using Yelp Dataset, go through the following steps and include detailed description (what have you done, what are the results) in your notes:

* [ ] extract: extract as much data as you can and need to face Big Data challenges (remember: volume, variety, veracity | we can discuss how to 'simulate' the fourth dimension of velocity using Yelp data in class)

#Extract

  Some of the Big Data challenges that I went through when extracting the data from the Yelp data set through MySQL began with attempting to download all of the data that was needed. The sheer volume of the data itself was very hard to manage for me because of a lack of computational power that I had. Each table in itself was serveral MBs to download and costed me 3 to 5 hours to completely download. After completion of downloading the all of the data, I attempted to just open the data in Excel, however, it crashed while attempting to view the data. In conversion to this experience, I tried to extract the JSON data thinking that the data would be easier to manipulate. Downloading all the data was quick, however, in JSON it was still barely readable. I copied the data in notepad and tried to paste the data into <http://jsonformatter.org/>. JSON formatter is a very popular tool that many developers use. I pasted the data in there only to have the site crash on me since it could not handle the volume of data it had to process. 

  csvcut -c business_id,city,review_count,name,state yelp_business | csvlook

  csvcut -c business_id,city,review_count,name,state ./yelp_business.csv | csvgrep -c state -m AZ  > ./yelp_business_extract.csv

  csvsql -e "utf8" --db mysql://root:password@localhost/yelp?charset=utf8 --insert ./yelp_business_extracted.csv

  mysql --local-infile -uroot -ppassword yelp

  CREATE TABLE yelp_business_final (neighborhood VARCHAR(40), business_id VARCHAR(40), hours VARCHAR(40), is_open INT, address VARCHAR(40), attributes VARCHAR(40), categories VARCHAR(40), city VARCHAR(40), review_count INT, name VARCHAR(40), longitude VARCHAR(40), state VARCHAR(2), stars INT, latitude VARCHAR(40), postal_code VARCHAR(10), type VARCHAR(20));


  csvsql --no-inference --blanks -e "utf8" --db mysql://root:password@localhost/yelp?charset=utf8 --insert ./yelp_business_extract.csv


  LOAD DATA LOCAL INFILE "/Users/bryannguyen/Documents/School/Spring2017/CS157B/Data/yelp_business"
INTO TABLE yelp_business_final
FIELDS TERMINATED BY ','
LINES TERMINATED BY '\n'
IGNORE 1 LINES
(neighborhood, business_id, hours, is_open, address, attributes, categories, city, review_count, name, longitude, state, stars, latitude, postal_code, type);

csvformat -D "|" ./yelp_academic_dataset_business.csv | cat > yelp_business_formatted.csv


LOAD DATA LOCAL INFILE "/Users/bryannguyen/Documents/School/Spring2017/CS157B/Data/yelp_business_final.csv"
INTO TABLE yelp_business_final
FIELDS TERMINATED BY '|' ESCAPED BY '"'
LINES TERMINATED BY '\n'
IGNORE 1 LINES
(neighborhood, business_id, hours, is_open, address, attributes, categories, city, review_count, name, longitude, state, stars, latitude, postal_code, type);

ALTER TABLE yelp_business
ADD COLUMN state VARCHAR(2) AFTER name;

UPDATE yelp_business
SET business_id = replace(business_id,',','');
---

* [ ] transform: convert extracted data to the format / schema suitable for loading into the database schema

#Transform

  The first step I did was I did this command: <br>_head -n 100 yelp_academic_dataset_business.csv | cat > ./yelp_business.csv in my terminal._

  In order to get the data into MySQL, I decided to go down the JSON route. First, I pulled all the information off the Google Drive because it seemed like JSON data would be much easier to import compared to some how get the very large CSV files into MySQL.

  _csvcut -c business_id,city,review_count,name,state ./yelp_business.csv | csvlook_

---

* [ ] load: note prior to loading this steps requires some efforts in schema design (tables, column datatypes, constraints)

#Load

  Once I got the JSON data, I realized that I had to tell MySQL what data types each column was. I skimmed through the JSON object to find all the keys in order to put them in each column and manually insert what kind of data type each object was. However, using MySQL in the terminal was very slow and visibility was not good, so I had to install MySQL workbench to generate my schema.

---

* [ ] query: 2 queries, one 'simple' OLTP-like and one 2 OLAP-like analytical queries

#Query

---

* [ ] update: one simple update (single row, possibly over multiple tables) and one batch update (multiple rows)

#Update

---

* [ ] optimize quality: illustrate how to capture and maintain data quality via integrity constraints, normalization and transactions

#Optimize Quality

---

#Optimize Performance

* [ ] optimize performance: improve performance via de-normalization (vertical / horizontal partitioning), indexes, views

---
