##Bryan Nguyen
###Duc Thanh Tran
CS157B Notes Homework 2

---

Using Yelp Dataset, go through the following steps and include detailed description (what have you done, what are the results) in your notes:

* [x] extract: extract as much data as you can and need to face Big Data challenges (remember: volume, variety, veracity | we can discuss how to 'simulate' the fourth dimension of velocity using Yelp data in class)

#Extract

  Some of the Big Data challenges that I went through when extracting the data from the Yelp data set through MySQL began with attempting to download all of the data that was needed. The sheer volume of the data itself was very hard to manage for me because of a lack of computational power that I had. Each table in itself was serveral MBs to download and costed me 3 to 5 hours to completely download. After completion of downloading the all of the data, I attempted to just open the data in Excel, however, it crashed while attempting to view the data. In conversion to this experience, I tried to extract the JSON data thinking that the data would be easier to manipulate. Downloading all the data was quick, however, in JSON it was still barely readable. I copied the data in notepad and tried to paste the data into <http://jsonformatter.org/>. JSON formatter is a very popular tool that many developers use. I pasted the data in there only to have the site crash on me since it could not handle the volume of data it had to process. In the end, I was able to successfully view the columns of the data only with the help of csvcut: <br>

  _csvcut -c business_id,city,review_count,name,state business | csvlook_

  I then extracted the columns that were necessary to me:

  _csvcut -c business_id,city,review_count,name,state ./business.csv | csvgrep -c state -m CA  > ./yelp_business_extract.csv_

  Now within yelp_business_extract.csv, I was able to open it in Excel. In order to accurately compare the data with the original before I extracted it, was nearly impossible since I couldn't get the original to open. I had to use the 'head' command and pull certain lines such as the first 200 lines and compare it with my extraction. The result was that it was pretty comparable to the original data therefore using csvcut seemed to do the trick.

---

* [ ] transform: convert extracted data to the format / schema suitable for loading into the database schema

#Transform & Load

---

  ##Transform

  In order to get the data into MySQL, I decided to go down the JSON route. First, I pulled all the information off the Google Drive because it seemed like JSON data would be much easier to import compared to getting the very large CSV files into MySQL. However, this turned out to be more confusing attempting to just view the JSON file in a JSON validating tool was not possible. Instead, I went back to using the provided CSV files.

  In order to prepare to convert CSV file to be imported, I first created an empty CSV file named yelp_business.csv.

  The second step I did was I did this command: <br>_head -n 100 yelp_academic_dataset_business.csv | cat > ./yelp_business.csv in my terminal._

  Now, yelp_business.csv has about the first 100 lines that I want to use in my database. I also only want certain columns that might be relavent to me, so I used the command:

  _csvcut -c neighborhood,business_id,hours,is_open,address,attributes,categories,city,review_count,name,longitude,state,stars,latitude,postal_code,type ./yelp_business.csv | csvlook_


  After this command, I was shown the lines that exist in my file.

  | neighborhood | business_id | hours | is_open | address | attributes | categories | city | review_count | name | longitude | state | stars | latitude | postal_code | type |



##LOAD

After this, I created the schema within the creation of my table.
Once I got the JSON data, I realized that I had to tell MySQL what data types each column was. I skimmed through the JSON object to find all the keys in order to put them in each column and manually insert what kind of data type each object was. However, using MySQL in the terminal was very slow and visibility was not good, so I had to install MySQL workbench to generate my schema.


_CREATE TABLE business (neighborhood text, business_id VARCHAR(45), hours text, is_open text, address text, attributes text, categories text, city text, review_count int(11), name text, longitude double, state text, stars int(11), latitude double, postal_code VARCHAR(45), type text);_

_CREATE TABLE checkin (time text, type text, business_id VARCHAR(45));_

_CREATE TABLE user (yelping_since text, useful int(11), compliment_photos int(11), compliment_list int(11), compliment_funny int(11), compliment_plain int(11), review_count int(11), elite text, fans int(11), type text, compliment_note int(11), funny int(11), compliment_writer int(11), compliment_cute int(11), average_stars double, user_id VARCHAR(45), compliment_more int(11), friends text, compliment_hot int(11), cool int(11), name text, compliment_profile int(11), compliment_cool int(11));_

_CREATE TABLE tip (user_id text, tip text, business_id varchar(45), likes int(11), date text, type text);_

_CREATE TABLE review (funny int(11), user_id varchar(45), review_id varchar(45), review text, business_id varchar(45), stars int(11), date text, useful int(11), type text, cool int(11));_


Now my data was ready for importing. I started my sql and loaded the file in local using --local-infile (had to do this or else mysql would say not in the language).

_mysql --local-infile -uroot -ppassword yelp_

_LOAD DATA LOCAL INFILE "/Users/bryannguyen/Documents/School/Spring2017/CS157B/Data/yelp_academic_dataset_business.csv"
INTO TABLE business
FIELDS TERMINATED BY ',' ENCLOSED BY '"'
escaped by '"'
LINES TERMINATED BY '\n'
IGNORE 1 LINES
(neighborhood, business_id, hours, is_open, address, attributes, categories, city, review_count, name, longitude, state, stars, latitude, postal_code, type);_

_LOAD DATA LOCAL INFILE "/Users/bryannguyen/Documents/School/Spring2017/CS157B/Data/yelp_academic_dataset_checkin.csv"
INTO TABLE checkin
FIELDS TERMINATED BY ',' ENCLOSED BY '"'
escaped by '"'
LINES TERMINATED BY '\n'
IGNORE 1 LINES
(time, type, business_id);_

_LOAD DATA LOCAL INFILE "/Users/bryannguyen/Documents/School/Spring2017/CS157B/Data/yelp_academic_dataset_user.csv"
INTO TABLE user
FIELDS TERMINATED BY ',' ENCLOSED BY '"'
escaped by '"'
LINES TERMINATED BY '\n'
IGNORE 1 LINES
(yelping_since, useful, compliment_photos, compliment_list, compliment_funny, compliment_plain, review_count, elite, fans, type, compliment_note, funny, compliment_writer, compliment_cute, average_stars, user_id, compliment_more, friends, compliment_hot, cool, name, compliment_profile, compliment_cool);_

_LOAD DATA LOCAL INFILE "/Users/bryannguyen/Documents/School/Spring2017/CS157B/yelp_academic_dataset_review-3.csv"
INTO TABLE review
FIELDS TERMINATED BY ',' ENCLOSED BY '"'
escaped by '"'
LINES TERMINATED BY '\n'
IGNORE 1 LINES
(funny, user_id, review_id, review, business_id, stars, date, useful, type, cool);_

_LOAD DATA LOCAL INFILE "/Users/bryannguyen/Documents/School/Spring2017/CS157B/Data/yelp_academic_dataset_tip.csv"
INTO TABLE tip
FIELDS TERMINATED BY ',' ENCLOSED BY '"'
escaped by '"'
LINES TERMINATED BY '\n'
IGNORE 1 LINES
(user_id, tip, business_id, likes, date, type);_

After loading my data, I realized that there was problems with commas and the characters: _u'_ in some of the fields so I ran:

_UPDATE yelp_business_final
SET attributes = replace(attributes,'u"'','');_

_UPDATE yelp_business_final
SET categories = replace(categories,'u''','');_

_UPDATE yelp_business_final
SET business_id = replace(business_id,',','');_

After this, my business table was successfully imported, however some of the data was still inaccurate when I did: _select stars from yelp_business_final_ . I had postal_code in some of the fields for postal_code which was not correct.

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
