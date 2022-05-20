## DynamoDB's Single-Table Design

DynamoDB provides many benefits that other databases don’t, such as a flexible pricing, a stateless connection model that works perfectly with server less compute, and consistent response time even as the database scales to enormous size. But the major pain point is data modeling design for people likes me who are used to with relational databases which have dominated the industry for several decades now. There are several quirks present but the main one in my opinion will be putting all your records to a single table and its a AWS's recommendation.

What, storing all records in a single table? Most of the relational db case it would be a very bad data architecture but for DynamoDB its not the case. Today we will explore more on the single table design with DynamoDB. Most of the examples and images taken from  [Alex DeBrie's blog post. ](https://www.alexdebrie.com/posts/dynamodb-single-table/) 

### What is Single table design ?

As its name suggests having a single table in a db to keep all our records. Before dive into the architecture lets try to understand why we need this and to understand the need let first have a small background of the relational db's SQL joins.

With relational databases, we generally normalize our data by creating a table for each type of entity in our application. For example, if we’re making an e-commerce application, we’ll have one table for Customers and one table for Orders.


![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1625171797811/kOb9u0L-G.png)

Each Order belongs to a specific Customer, and we use foreign keys to refer from a record in one table to a record in another. These foreign keys are pointers — if we need more information about a Customer that placed a particular Order, we can simply follow the foreign key reference to retrieve items about the Customer.


![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1625171976302/HY7cdZOic.png)

And to do this in SQL language we have a concept call `joins`. Joins allow us to combine records from two or more tables at read-time.

### But we don't have joins in DynamoDB

DynamoDBs does not support joins and its also makes sense because joins are very expensive as they scans large portions of the multiple tables in RDBMS to return you the result but DynamoDB was built for enormous, high-velocity use cases which can not tolerate the join operations expensiveness. To work with multiple tables engineers rather fixing the scaling problem due to joins they invented a new solution by removing the join use completely.

If we design the DD(DynamoDB) tables in relational way  then without joins we have to query two times to fetch the both the customer and order data like below 


![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1625288944167/xfF-PHSfm.png)

This serial querying can become a big issue in your application when it scales, this pattern gets slower and slower. Then what is the solution?

### Pre-joining data in Item collections

The solution to above no use of join problem is pre joining our data into item collections. An item collection in DynamoDB refers to all the items in a table or index that share a partition key. In the example below, we have a DynamoDB table that contains actors and the movies in which they have played. The primary key is a composite primary key where the partition key is the actor’s name and the sort key is the movie name.


![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1625289287447/we4kBTH4F.png)

In above example the two items `Toy Story` & `Cast Away` are belongs to same partition key `Tom Hanks`. Here we organized the data in way we want to read so by using DD `Query` api we can fetch heterogenous data with superfast speed without any joins. So if we design our Customer & Order table  in the same way as did with actor an movie tables then the access patterns will be like below


![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1625289633580/UhFv_w6lp.png)

This is what single-table design is all about — tuning your table so that your access patterns can be handled with as few requests to DynamoDB as possible, ideally one.

Apart from reducing the request count some other benefits of single table design will be


- Less operational overhead as we need to manage only one table.
- Less cost as we need to provision read and write capacity units for a single table.
- In general, when thinking about single-table design, you should consider the main benefit to be the performance improvement by making a single request to retrieve all needed items.

DyanmoDB also comes with some cons

- The steep learning curve to understand single-table design.
- The inflexibility of adding new access patterns.
- The difficulty of exporting your tables for analytics.

In this post, we reviewed the concept of single-table design in DynamoDB. We looked at the Pros and some cons to single-table design. Deciding which database to be used is always vary from use cases to use cases but its not a harm to learn this unique design philosophy for high speed access. There are people of two different thoughts, one is we can use DynamoDB for all cases and other RDBMS still can be a global DB. Its up to you what to best suits you. Based on the various use cases and evolving technology and data size I will recommend when in doubt what to use go for DynamoDB.

Please let me know what your thoughts on DynamoDB. Happy Learning!
 