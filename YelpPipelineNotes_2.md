##Bryan Nguyen
###Duc Thanh Tran
CS157B Notes Homework 2

---

Using Yelp Dataset, go through the following steps and include detailed description (what have you done, what are the results) in your notes:

* [ ] extract: extract as much data as you can and need to face Big Data challenges (remember: volume, variety, veracity | we can discuss how to 'simulate' the fourth dimension of velocity using Yelp data in class)

#Extract

  Some of the Big Data challenges that I went through when extracting the data from the Yelp data set through MySQL began with attempting to download all of the data that was needed. The sheer volume of the data itself was very hard to manage for me because of a lack of computational power that I had. In order for me to actually handle the data, I had to simplify the data so that I could view the columns and see what kind of data I was working with.

---

* [ ] transform: convert extracted data to the format / schema suitable for loading into the database schema

#Transform

  The first step I did was I did this command: <br>_head -n 100 yelp_academic_dataset_business.csv | cat > ./yelp_business.csv in my terminal._

  In order to get the data into MySQL, I decided to go down the JSON route. First, I pulled all the information off the Google Drive because it seemed like JSON data would be much easier to import compared to some how get the very large CSV files into MySQL. However, once I got the JSON data, I realized that I had to tell MySQL what data types each column was. I skimmed through the JSON object to find all the keys in order to put them in each column and manually insert what kind of data type each object was. However, using MySQL in the terminal was very slow and visibility was not good, so I had to install MySQL workbench to generate my schema.

---

* [ ] load: note prior to loading this steps requires some efforts in schema design (tables, column datatypes, constraints)

#Load

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



<style>
h1 {
  color: blue;
}
</style>
