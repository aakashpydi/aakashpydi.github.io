---
published: true
layout: post
title: Understanding C-Store. Columnar Databases and Big Data Analytics
---
From what I understand, the first prototype for a columnar database was introduced in a 2005 paper from MIT’s school of computer science and artificial intelligence, called [C-Store: A Column Oriented Database. (Stonebraker et al). MIT CSAIL](http://db.csail.mit.edu/projects/cstore/vldb.pdf).

The key idea from this paper seems to be that, in columnar databases we store each column’s values sequentially in a database, as opposed to storing each row’s values sequentially. Let’s try to visualize this idea.

Consider the following examples given on [Amazon Redshift’s Database Developer Guide](https://docs.aws.amazon.com/redshift/latest/dg/c_columnar_storage_disk_mem_mgmnt.html).

![]({{site.baseurl}}/images/columnar_dbs_images/example_1.png)
Some points to note regarding the example above:

1. Notice that each block has values of different types (string, integer, etc).
1. What happens if the row size is greater than a [block](https://docs.oracle.com/cd/B19306_01/server.102/b14220/logical.htm)? We will need to read more than a block to read a single row.
1. Similarly, what happens if the row size is smaller than a [block](https://docs.oracle.com/cd/B19306_01/server.102/b14220/logical.htm)? In this case we will have inefficient use of disk space.
1. Note that a data block is the smallest unit of data used by a database. See this link for more information: [Introduction to Data Blocks. Oracle](https://docs.oracle.com/cd/B19306_01/server.102/b14220/logical.htm).
1. This is a ‘write-optimized’ database as it has a very low cost to write a new record to disk. The DB can easily add a new block with the record’s data.

![]({{site.baseurl}}/images/columnar_dbs_images/example_2.png)
Some points to note regarding the example above:

1. Notice that each block has the same data type. This turns out to be a significant advantage as the DB can use [compression techniques](http://db.csail.mit.edu/projects/cstore/abadisigmod06.pdf) to significantly reduce the disk usage and I/O.
1. This is a ‘read optimized’ database, particularly in a big data analytics context (which is what we use the large datasets in our columnar database for). Consider how most queries only require a few of the column values. So when we have a large dataset, with many rows and columns, only retrieving the columns that we need to execute our analytics query is substantially more efficient than retrieving all the records in the database.

The original CSAIL paper notes that write optimized database systems are naturally ideal write heavy applications (such as [Online Transaction Processing](https://docs.microsoft.com/en-us/azure/architecture/data-guide/relational-data/online-transaction-processing) applications) whereas read optimized database systems are naturally ideal for read heavy applications (such as [Data Warehouses](https://aws.amazon.com/data-warehouse/)).

It should be evident why columnar databases are well suited for big data analytics architectures. They offer (i) significant data compression, and (ii) highly optimized read operations that are typically used for analytics jobs. My Director, Dan Woicke wrote about the value of using a columnar database to our team's work here: [Cerner Advances Big Data Analytic Capabilities. Dan Woicke. CIO Review](https://hp.cioreview.com/cxoinsight/cerner-advances-big-data-analytic-capabilities-nid-11200-cid-59.html). There are more features that columnar databases offer which you can explore in the additional reading section below.

Some additional reading,

1. [C-Store: A Columnar Database. Ameya. Medium](https://medium.com/@ameya_s/c-store-a-columnar-database-1fe7e84d7247).
1. [Rowwise vs Columnar Database? Theory and in Practice. Modi. Medium](https://medium.com/@mangatmodi/rowise-vs-columnar-database-theory-and-in-practice-53f54c8f6505).
1. [The beauty of column oriented data. Zaks, Maxim. Towards Data Science](https://towardsdatascience.com/the-beauty-of-column-oriented-data-2945c0c9f560).
1. [Column Oriented Database Systems. Stavros Harizopoulous. UC Davis Presentation](https://web.cs.ucdavis.edu/~green/courses/ecs165b-s10/Column_Store_Tutorial_VLDB09.pdf).
1. [Integrating Compression and Execution in Column-Oriented Database Systems. Abadi et Al. MIT](http://www.cs.yale.edu/homes/dna/papers/abadisigmod06.pdf).
1. [The Vertica Analytic Database: C-Store 7 Years Later. (Lamb et al). Vertica](http://vldb.org/pvldb/vol5/p1790_andrewlamb_vldb2012.pdf).
1. [Why All Column Stores Are Not The Same: Twelve Low Level Features That Offer High Value To Analysts. Vertica](https://www.vertica.com/wp-content/uploads/2018/05/why_all_column_stores_are_not_the_same_wp.pdf).
