---
published: true
layout: post
title: Understanding C-Store: Columnar Databases and Big Data Analytics
---

On my first day of joining a DevOps+Data Engineering team at [Cerner](https://www.cerner.com/) a couple of weeks ago, my manager and team-mentor gave me a high level overview of our team’s data architecture. The technologies associated with our team’s data pipeline were all extremely fascinating to me. I was particularly curious about our primary database which stores several petabytes (1 petabyte = 10¹⁵ bytes) of Ops data.

I was told that our primary database was highly optimized for read operations because it used columnar storage. I am a fresh grad and I wasn’t taught about columnar databases in school, so I had no clue what they were talking about. So I set out to do some research.

Columnar Databases

From what I understand, the first prototype for a columnar database was introduced in a 2005 paper from MIT’s school of computer science and artificial intelligence, called [C-Store: A Column Oriented Database. (Stonebraker et al). MIT CSAIL](http://db.csail.mit.edu/projects/cstore/vldb.pdf).

The key idea from this paper seems to be that, in columnar databases we store each column’s values sequentially in a database, as opposed to storing each row’s values sequentially. Let’s try to visualize this idea.

Consider the following examples given on [Amazon Redshift’s Database Developer Guide](https://docs.aws.amazon.com/redshift/latest/dg/c_columnar_storage_disk_mem_mgmnt.html).

![]({{site.baseurl}}/images/columnar_dbs_images/example_1.png)

Some points to note regarding the example above:

1. Notice that each block has values of different types (string, integer, etc).
1. What happens if the row size is greater than a block? We will need to read more than a block to read a single row.
1. Similarly, what happens if the row size is smaller than a block? In this case we will have inefficient use of disk space.
1. Note that a data block is the smallest unit of data used by a database. See this link for more information: [Introduction to Data Blocks. Oracle](https://docs.oracle.com/cd/B19306_01/server.102/b14220/logical.htm).
1. This is a ‘write-optimized’ database as it has a very low cost to write a new record to disk. The DB can easily add a new block with the record’s data.

---

![]({{site.baseurl}}/images/columnar_dbs_images/example_2.png)

Some points to note regarding the example above:

1. Notice that each block has the same data type. This turns out to be a significant advantage as the DB can use [compression techniques](http://db.csail.mit.edu/projects/cstore/abadisigmod06.pdf) to significantly reduce the disk usage and I/O.
1. This is a ‘read optimized’ database, particularly in a big data analytics context (which is what we use the large datasets in our columnar database for). Consider how most queries only require a few of the column values. So when we have a large dataset, with many rows and columns, only retrieving the columns that we need to execute our analytics query is substantially more efficient than retrieving all the records in the database.

The original CSAIL paper notes that write optimized database systems are naturally ideal write heavy applications (such as [Online Transaction Processing](https://docs.microsoft.com/en-us/azure/architecture/data-guide/relational-data/online-transaction-processing) applications) whereas read optimized database systems are naturally ideal for read heavy applications (such as [Data Warehouses](https://aws.amazon.com/data-warehouse/)). Our team uses the [Vertica](https://www.vertica.com/) columnar database system to ingest lots and lots Ops data, and also carry out data analytics on the ingested data. Investigating columnar databases has helped me appreciate this design choice.

---

Some additional reading,

(1) [The beauty of column oriented data. Zaks, Maxim. Towards Data Science](https://towardsdatascience.com/the-beauty-of-column-oriented-data-2945c0c9f560).

(2) [Column Oriented Database Systems. Stavros Harizopoulous. UC Davis Presentation](https://web.cs.ucdavis.edu/~green/courses/ecs165b-s10/Column_Store_Tutorial_VLDB09.pdf).

(3) [Integrating Compression and Execution in Column-Oriented Database Systems. Abadi et Al. MIT](http://www.cs.yale.edu/homes/dna/papers/abadisigmod06.pdf).

(4) [The Vertica Analytic Database: C-Store 7 Years Later. (Lamb et al). Vertica](http://vldb.org/pvldb/vol5/p1790_andrewlamb_vldb2012.pdf).

(5) [Why All Column Stores Are Not The Same: Twelve Low Level Features That Offer High Value To Analysts. Vertica](https://www.vertica.com/wp-content/uploads/2018/05/why_all_column_stores_are_not_the_same_wp.pdf).
