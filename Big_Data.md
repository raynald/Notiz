# Big Data
### 1. Introduction
#### 1.1 Big Data in the Abstract
 - Big Data as a Megatrend
 - Big Data Characteristics: Big Vs
    - Volume: Web data, Sensor data, Proprietary data, Scientific data
    - Velocity: Real-time data
    - Variety: Heterogeneity and Noise
#### 1.2 Big Data in the Sciences 
 - High Energy + Big Data = New Knowledge
 - Sloan Digital Sky Survey
 - Personal Genomics
#### 1.3 Big Data in the New Economy
 - Google Web Search
 - Prediction of Relevance
#### 1.4 Big Data in the "Good Old" Economy
 - Traffic Networks
#### 1.5 Intelligent Systems
 - Machine Intelligence from Big Data
 - Computer Vision, Face Recognition
 - Machine Translation
 - Speech Recognition

### 2. Data Warehousing
#### 2.1 OLTP
 - Traditional database systems
 - ACID: Atomicity, Consistency, Isolation, Durability
 - Normalization
   - First normal form
   - Second normal form
   - Third normal form
#### 2.2 OLAP
 - OLTP: consistent and reliable record keeping 
 - OLAP: analytical processing -> data-based decision support
    - historical, summarized and consolidated data is more important than detailed, individual records

 - Diff: source, purpose, interface, inserts/updates, queries, design, precision, freshness, spend, optimization, space, backup/recovery
#### 2.3 Data Warehouse
 - Data warehouse: is a separate database that copies, integrates, and aggregates data from one or more master OLTP databases in order to support OLAP. is a subject-oriented, integrated, time-variant, and nonvolatile collection of data in support of management's decision-making process. - W. H. Inmon
#### 2.4 ETL: Extract, Transform, Load
 - Extract
 - Transform
 - Load
#### 2.5 OLAP via ROLAP
 ROLAP = relational OLAP
 MOLAP = multidimensional OLAP

 Star Schema
  - Fact table: a very very
  - Measures
  - Dimension tables

#### 2.6 Data Cubes
 MOLAP and Data Cubes
#### 2.7 OLAP Servers
#### 2.8 Conclusion
#### 

### 3. Storage
#### 3.1 File Systems
##### 3.1.1 Google File System
    Raw Data
    Derived Data: operations on raw data
    File Systems for Big Data
        Fault tolerance and robustness
        Big files
        Append write
        Semantics and primitives
        Performance
    GFS and HDFS
    GFS Architecture
        cluster, master, chunservers
    System Components: Chunks, Chunkservers
##### 3.1.2 Metadata Management
    Master Metadata and Policies
        Master: manages three tyeps of metadata
        Locks
##### 3.1.3 Replication and Consistency
##### 3.1.4 Hadoop File System
    Tools for Ingesting Data into HDFS
        Apache Flume
        Apache Sqoop
#### 3.2 Key-Value Stores
##### 3.2.1 Distributed Hash Tables
    Consistent Hashing
    Pros
     - Highly scalable
     - Robust against node failure
     - Self organizing
    Cons
     - Lookup, no search
     - Data integrity
     - Security problems (P2P)
##### 3.2.2 Weak Consistency
     High Level Perspective: Strong consistency, weak consistency, eventual consistency
     Client-side View: Causal consistency, read-your-writes consistency, session consistency, monotonic read consistency, monotonic write consistency
     Server-side View
##### 3.2.3 Dynamo
     Motivation 
#### 3.3 Big Tables
##### 3.3.1 Column Stores
     Limitations of RDBM
        Heavy use of joins, stored procedures, transactions
    Advantage of Column Stores: data locality, compression, simd instructions
    Disadvantage of Column Stores: every query involves a join of the columns, need to "materialize" tuples; copy data, every insert involves n inserts(n columns)
##### 3.3.2 Google's Big Table
    Tablets
    HBase
#### 3.4 Global Databases
##### 3.4.1 Time Stamping
    TrueTime (Google Spanner)
##### 3.4.2 Google's Spanner
#### 3.5 Big Query Engines
##### 3.5.1 Protocol Buffers 
    XML, 
##### 3.5.2 Google's Big Query
##### 3.5.3 Pig and Pig Latin
##### 3.5.4 Hive

### 5. Big Computing
#### 5.1 High Performance Computing
##### 5.1.1 Moore's Law
    Number of transistors in a dense integrated circuit doubles approximately every two years
    Three eras of processor performance: single-core era, multi-core era, heterogeneous systems era
        Single-Core Era
        Multi-Core Era
        Heterogeneous Systems Era
##### 5.1.2 Parallel Computing
    Type of parallelism
        Single Instruction, Multiple Instruction
        Single DAta, Multiple Data
        SISD, MISD, SIMD, MIMD
    Data Parallelism: splitting up the data
        Single Program Multiple Data
    Task Parallelism: parallelizing the algorithm
        Divide and conquer
        Pipeline
    Speedup & Efficiency
        Parallel speed-up: S(n) = T(1) / T(n)
        Parallel efficiency: E(n) = S(n) / n
    Limits of Parallel Computing
        Amdahl's Law: parallel execution time: T(n)>=(B+1/n*(1-B))T(1)
        Resulting maximal speed-up: S(n) = T(1)/T(n)<= 1/(B+(1-B)/n) --> 1/B
    Weak Scaling
        Gutafson's Law: S(n) = B + n(1-B) = n-B(n-1) =(n->infi) (1-B)n
        Strong Scaling (Amdahl)
        Weak Scaling (Gustafson)
##### 5.1.3 Parallel Computing with Data
    Shared and Distributed Memory
    Multicore Architecture
    Programming Models for Shared Memory Parallelism
        OpenCL, OpenML
    Distributed Memory Hierarchy
    Grid Computing: uses middleware to divide and apportion pieces of a program among several computers
    Message Passing for Parallelism
    Message Passing Interface(MPI): MPI-1, MPI-2, MPI-3
        Pros: standardization, portablity, functionality
        Cons: parallelism is explicit, no support for fault-tolerance
##### 5.1.4 Grid Computing
##### 5.1.5 Discussion 
    High Performance Computing
        Hardware: supercomputer, communication: fast interconnections
        Software: Specialized (Parallel) Programming Tools, MPI, OpenMP
    Pros: Speed! 
    Cons: Difficult to deal with errors and delays
    In Contrast: Commodity Hardware Clusters
        Cons: often slow, communication cost, iteration overhead
        Pros: fault tolerance, easy to program
#### 5.2 MapReduce
##### 5.2.1 History & Roots
    Google: Inverted Index, PageRank, Matrix Computation, Machine Learning
##### 5.2.2 Paradigm
##### 5.2.3 MapReduce Parallelism
    Task Breakup
    Task Granularity
##### 5.2.4 MapReduce Execution
#### 5.3 Algorithms in MapReduce
##### 5.3.1 Matrix Computations in MapReduce
    Vector-Matrix Multiplication
    Mapper = Multiplier, Reducer = Adder
    PageRank
        Power Method
        MapReduce
    Matrix Multiplication with Block Partitioning
##### 5.3.2 SQL-style Computations in MapReduce
    Selection and Projection
        Selection: mapper and reducer
        Projection: mapper, reducer
    Set Operations: union, intersection, difference
    Bag Operations: union, itersection, difference
    Natural Joins 
##### 5.3.3 Machine Learning with MapReduce
    K-means Clustering
    Gradient-based Machine Learning
    Two-way join, three-way join with hashing
##### 5.3.4 Complexity Theory for MapReduce
##### 5.3.5 Resilient Distributed Datasets & SPARK
#### 5.5 Resilient Distributed Datasets & SPARK
    Motivation & Overview
        RDDs: parallel data structures
### 8 Computing with DAta Streams
#### 8.1 Introduction & Motivation
##### 8.1.1 Streaming Data Paradigm
##### 8.1.2 Example: Logs Processing
    Web Search Logs
    Click Through Prediction
    Apache Kafka
        Data Management
##### 8.1.3 Example: Internew Traffic Monitoring and Frequent Items
    Misra-Gries Algorithm
        Analysis
##### 8.1.4 Other Examples
    Smart Meters
    On-line Video
#### 8.2 Streaming Data Management
##### 8.2.1 Streaming DBMS
    Aurora, Borealis
##### 8.2.2 Apache Kafka
##### 8.2.3 Apache Storm
    Spout, Bolt, Topology
##### 8.2.4 Spark Streaming
##### 8.2.5 (Small) Streams in Programming Languages
#### 8.3 Sampling and Sample-based Estimation
##### 8.3.1 Introduction
##### 8.3.2 Sampling
##### 8.3.2 Estimation
#### 8.4 Data Sketching
#### 8.4.1 Bloom Filters
    Data Sketching
#### 8.4.2 Count-Min Sketch
    From Filtering to Counting
    Updating the Count Sketch
    Inner Product Queries
#### 8.4.3 Count-Mean Sketch
#### 8.4.4 Counting Distinct Elements
    Cardinality Estimation via Linear Counting
    LogLog Counting
    HyperLogLog Counting
### 12. Semi-Structured Data
#### Part 1: Syntax
    Semi-Structured Documents
    XML vs JSON
    Element
    Attribute <a attr="value"/>
    Text <a>This</a>
    Comment: <!-- This is a comment -->
    Processing Instruction: <?xml version="1.0"?>
    Well-formedness <a foo="bar" bar="foo"/> <a>1 &lt; 2</a>
        < &lt; > &gt; " &quot; ' &apos; & &amp;
        Character References: &#x03A0; 
        <a attr="a &quot;quote&quot;"/>
    Entity References:
        &myownentity; -> footbar
    CDATA sections:
        <![CDATA[ &<<>>"' ]]>
    Namespace + Local name = {http://s...com}entity

    HTML Elements
        Void: <br>
        Raw text: <style> body {color: black; background: white;} </style>
        Escapable raw text: <title> This is a &quot;title&quot; </title>
    YAML
#### Part 2: Data Models
    XML Infoset
    Post-Schema-Validation Infoset (PSVI)
    Protocol Buffers
    Validation
    Document Type Declaration
    Element Type Declaration
    Attribute-List Declaration
#### Part 3: Querying
    Language Ecosystem
    Let clauses
    For clauses
    Where clauses
#### Part 5: Graph Databases (Intro)
    Resource Description Framework: RDF
    IRI, Literal, Blank node: subject, property, object
    Entailment (and Syllogisms)
