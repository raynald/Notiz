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
    Derived Data
    File Systems for Big Data
    GFS and HDFS
    GFS Architecture
        System Components: Chunks, Chunkservers
##### 3.1.2 Metadata Management
    Master Metadata and Policies
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
     Causal consistency, read-your-writes consistency, session consistency, monotonic read consistency, monotonic write consistency
##### 3.2.3 Dynamo


### 5. Big Computing
#### 5.1 High Performance Computing
##### 5.1.1 Moore's Law
    Number of transistors in a dense integrated circuit doubles approximately every two years
    Three eras of processor performance: single-core era, multi-core era, heterogeneous systems era
##### 5.1.2 Parallel Computing
    Type of parallelism
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
        Gutafson's Law
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
    MPI: MPI-1, MPI-2, MPI-3
        Pros: standardization, portablity, functionality
        Cons: parallelism is explicit, no support for fault-tolerance
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
##### 5.2.4 MapReduce Execution
#### 5.3 Algorithms in MapReduce
##### 5.3.1 Matrix Computations in MapReduce
    Vector-Matrix Multiplicationkj
    Mapper = Multiplier, Reducer = Adder
    PageRank
        Power Method
        MapReduce
    Matrix Multiplication with Block Partitioning
##### 5.3.2 SQL-style Computations in MapReduce
    Selection and Projection
    Set Operations
    Bag Operations
    Natural Joins
##### 5.3.3 Machine Learning with MapReduce
    K-means Clustering
    Gradient-based Machine Learning
##### 5.3.4 Complexity Theory for MapReduce

#### 5.5 Resilient Distributed Datasets & SPARK
