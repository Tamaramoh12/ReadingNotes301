# Introduction to Database Normalization
Database normalization is a process used to organize a database into tables and columns.  The main idea with this is that a table should be about a specific topic and only supporting topics included.

# Reasons for Database Normalization
1- minimize duplicate data.

2- minimize or avoid data modification issues.

3- simplify queries.

**Example for non-normalized table**

![](Capture25.PNG)

The first thing to notice is this table serves many purposes including:

1- Identifying the organization’s salespeople.

2- Listing the sales offices and phone numbers.

3- Associating a salesperson with an sales office.

4- Showing each salesperson’s customers.

Having the table serve many purposes introduces many of the challenges; namely, data duplication, data update issues, and increased effort to query data.

# Data Duplication and Modification Anomalies
Notice that for each SalesPerson we have listed both the SalesOffice and OfficeNumber. There are duplicate salesperson data. Duplicated information presents two problems:

1- It increases storage and decrease performance.

2-It becomes more difficult to maintain data changes.

Consider if we move the Chicago office to Evanston, IL. To properly reflect this in our table, we need to update the entries for all the SalesPersons currently in Chicago.  Our table is a small example, but you can see if it were larger, that potentially this could involve hundreds of updates.

These situations are modification anomalies. Database normalization fixes them. 

# There are three modification anomalies that can occur:
- Insert Anomaly 

- Update Anomaly

- Deletion Anomaly

# Search and Sort Issues
The last reason we’ll consider is making it easier to search and sort your data.  In the SalesStaff table if you want to search for a specific customer such as Ford, you would have to write a query like:
```
SELECT SalesOffice
FROM SalesStaff
WHERE Customer1 = ‘Ford’ OR
      Customer2 = ‘Ford’ OR
      Customer3 = ‘Ford’
```

Clearly if the customer were somehow in one column our query would be simpler.  Also, consider if you want to run a query and sort by customer. 

Our current table makes this tough. You would have to use three separate UNION queries! You can eliminate or reduce these anomalies by separating the data into different tables. This puts the data into tables serving a single purpose.

## There are three common forms of database normalization:
1st, 2nd, and 3rd normal form. They are also abbreviated as 1NF, 2NF, and 3NF respectively.

- First Normal Form – The information is stored in a relational table with each column containing atomic values. There are no repeating groups of columns.

- Second Normal Form – The table is in first normal form and all the columns depend on the table’s primary key.

- Third Normal Form – the table is in second normal form and all of its columns are not transitively dependent on the primary key.

