# Data analysis and big data

## NoSQL
Not only SQL refers to data stores that are not replational databases.

NoSQL technologies have very heterogeneous operational, functional and architectural characteristics. They are non-relational, distributed and horizontally scalable and have been developed to address new challenges that emerged after 2009.

These challenges include:
* More volume of data
* Rapid change of data
* Unstructured data

This has led to a higher query execution time

* Structured data: Tabular data, represented by rows and columns
* Semi-structured data: Do not have a fixed format but still has some structuring, ex: tags to separate semantic elements
* Unstructured data: Free text, natural language, audio, images, videos. Hard to index and analyse

RDBMS: hard to scale, needs bigger server, requires DBA to monitor the DB, capacity constraints limit data heterogeneity.

NoSQL: easy to scale, simply add one more node, no need of a DBA, automatic system repair and distribution, specifically designed to cope with heterogeneous data.

Drawbacks of NoSQL:
* Several integrity constraints are  supported, ex: FK
* The absence of a fixed structure may become an issue
* Design process not straightforward
* Lack of SQL

### ACID vs BASE

ACID
* Atomicity: Either all operations in a transaction complete or everything is rolled back.
* Consistency: A transaction takes the DB in a consistent state and leaves it in a consistent state.
* Isolation: Transaction do not interfere with one another, transactions are concurrent but the result is the same as running them serially.
* Durability: The results of a transaction are permanent.

BASE
* Basically available: The store needs to be accessible most of the time.
* Soft state: Stores do not need to be write-consistent, nor do different replicas have to be consistent all the time.
* Evenutal consistency: The system eventually will be consistent at a later point.

Obviously the BASE principles are not good for a bank database, but they are sufficent for social networks.  

In the PAC theorem NoSQL databases choose: Availability and Partition tolerance, however when the network is ok they have both Availability and Consistency.


### Key/Value Stores
Store data in a hash table or a dictionary

Ex: SSN -> phone, SSN -> dept_name, dept_name -> budget.

Operations: Add, remove, modify, find.  
No join operations allowed.

Foreign keys not supported

High horizontal scalability

Very low data seek time, due to the hash table.

Use cases: Store preferences of each user, ex vscode configuration. Storage of multimedia objects.

### Document-oriented Databases
Store, retrieve and manage document oriented information, semi strictured data

Documents contain tags or markers to separate semantic elements or enforce a hierachy. They are a self-describing structure (XML, JSON).

Operations CRUD:
* Creation of a new document
* Retrieval based on key, content or metadata of a document
* Update the content or metadata of a document
* Deletion of a document

Similar to Key/Value Stores but can handle more complex data.

MongoDB: Example of a document-oriented DB, no need to store NULL values, simply remove the attribute
* Row = Document
* Column = Field
* Table = Collection

### Column-oriented Stores
Stores each column separately, rather than storing rows

Convenient when reading a subset of the column or when performing aggregate functions, like finding the average of all salaries.

Easier compression of data which is performed along the columns (lower entropy), this also saves IO costs and many operations can be done without decompressing, ex: SUM with run-length encoding

Good for storing sparse data, no need to store NULL

Overall very scalable and suited for massively parallel processing (MPP), data is partitioned over a large machine cluster

Drawbacks:
* Hard to write a new tuple, we need to break it down to their attributes and write them separately
* Hard to reconstruct a tuple, we need to gather all its components from all the locations

Apache Cassandra is an open-source, distributed column store, providing high-availability and scalability with no point of failure. AP system.

Uses CQL (Cassandra Query Language) to write query, much simpler than SQL, does not allow joins or subqueries among other things.

### Graph Databases

Uses graph structures to represent and store data, can represent semi-structured, highly interconnected data, like those present in social networks.

Can easy answer queries like: how many friends has x? How many friends connect y to x?

Graph databases differ in:
* Storage, how the database is stored, some graph databases use native graph storage, others rely on other NoSQL databases
* Processing, some graph DB are capable of native graph processing by means of **index-free adjacency**: nodes physically point to each other. 

Native graph DB do not rely on indexes but on pointers, this allows for a fast query execution time. Ex: if we want to get the neighbors of x we simply follow the pointers, rather than using an index.

Data models:
* Property graphs
  * Contains nodes and relationships
  * Nodes contain properties
  * Nodes can be labeled with one or more labels
  * Relationships are named and directed
  * Relationships can contain properties
* Hypergraph
  * A relationship can connect more than 2 nodes
  * Useful with many to many relationships
  * We can always convert an hypergraph to a normal graph
* Triples
  * Related to the Semantic Web movement: make internet data machine-readable
  * Harvest useful data and store it in triples for querying
  * A triple is a Subject-Predicate-Object, ex: John likes ice cream


## Data warehousing

Data: Raw, unanalyzed, unorganized material  
Information: Data that has been processed, indexed, organized in a human-friendly format  

Data warehousing is a technique for **collecting and managing** data from different sources to provide meaningful business insights
* **Storage** of a large amount of information designed for query and analysis, instead of transaction processing
* **Data migration and transformation** processes that ensure that only clean and reliable data is stored within the warehouse 

A data warehouse is the central component of a Decision Support System, an information system that supports business or decision-making activities, ex: provide monitoring tools, reports, dashboards. Decisions can also be automated by using Decision Management Systems

Analyst job is easier:
* Single source of data
* Data easily accessible

A data warehouse is a:
* subject-oriented: Focuses on enterprise concepts:
  * Customer
  * Account
  * Order
  * Product
* Integrated and consistent: Data is fed from disparate sources into the warehouse. The warehouse converts, reformats and applies other transformation to this data (ETL - Extract, Transform, Load). Many inconsistencies are resolved in the Transform step, for example naming conventions, units of measure, file extensions and so on.
* Non-volatile: After the data is inserted into the warehouse it is not changed, read only database. When changes occur it keeps an historical record of the data.
* Time variant: The warehouse stores data as it existed many points in the past (5-10 years time horizon)


### Data Mart

A data mart is focused on a signle area of an organization and is used by a specific department, contains a subset of the data stored in the data warehouse.

It can take as input data from outside data or from a data warehouse.  
Smaller in scale than a data warehouse

### Data Lake
Stores large amounts of data
* Every type of data in its native format, no limit on size or type
* Allows accessing data before the ETL process
* Retains data coming from all sources
* Data is only transformed when a user is about to use it
* Less expensive to store information w.r.t a data warehouse
* Not suitable for a data warehouse, more time to answer queries
* Can grow without control (data swamp)

### Data warehouse architecture
