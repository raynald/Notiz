# Big Data
### 1. Introduction
#### 1.1 Big Data in the Abstract
 - Big Data as a Megatrend
 - Big Data Characteristics: Big Vs
    - Volume: Web data, Sensor data, Proprietary data, Scientific data
    - Velocity: Real-time data
    - Variety: Heterogeneity and Noise
 - Data Analytics
    - Human Data Analytics
    - Machine Data Analytics
#### 1.2 Big Data in the Sciences 
 - High Energy + Big Data = New Knowledge
 - Sloan Digital Sky Survey
 - Personal Genomics
#### 1.3 Big Data in the New Economy
 - Google Web Search
 - Prediction of Relevance
    - FB, Amazon, Youtube, Netflix, Web Marketing
#### 1.4 Big Data in the "Good Old" Economy
 - Traffic Networks
#### 1.5 Intelligent Systems
 - Machine Intelligence from Big Data
 - Computer Vision, Face Recognition
 - Machine Translation
 - Speech Recognition

### 2. Data Warehousing
#### 2.1 OLTP (relational databases)
 - Traditional database systems
 - ACID: Atomicity, Consistency, Isolation, Durability
 - Normalization
   - First normal form: remove duplicate data and break data at granular
   - Second normal form: All column data should depend on full primary key and not part
   - Third normal form: No column should depend on other columns
 *High precision is needed, sampling and confidence interavls are not enough*
 *We are interested in processing very fast a high number of simple transactions, a solution which is often slow is not enough*
#### 2.2 OLAP (data warehouse)
 - OLTP: consistent and reliable record keeping 
 - OLAP: analytical processing -> data-based decision support
    - historical, summarized and consolidated data is more important than detailed, individual records

 - Diff: source, purpose, interface, inserts/updates, queries, design, precision, freshness, spend, optimization, space, backup/recovery
 *Complex queries involving aggregation are needed, simple queries are not enough*
 *We are interested in data history, a snapshot of the ongoing business process is not enough*
#### 2.3 Data Warehouse
 - Data warehouse: is a separate database that copies, integrates, and aggregates data from one or more master OLTP databases in order to support OLAP. is a subject-oriented, integrated, time-variant, and nonvolatile collection of data in support of management's decision-making process. - W. H. Inmon
#### 2.4 ETL: Extract, Transform, Load
 - Extract
 - Transform
 - Load
#### 2.5 OLAP via ROLAP
 ROLAP = relational OLAP
 MOLAP = multidimensional OLAP

> Star Schema
  - Fact table: a very very
  - Measures
  - Dimension tables
> Snowflake Schema
  - De-normalizing dimension tables not always feasible/optimal
  - Tradeoff: more compactness/less redundancy vs. higher costs of joining
  - Alternative: normalize dimension tables, introduce additional tables

#### 2.6 Data Cubes
 MOLAP and Data Cubes
#### 2.7 OLAP Servers
##### Inex Structures
> Bitmap indices
 - AND, OR, SUM
> Join indices
##### Materialized Views
##### Server Architectures
> Specialized SQL Servers
> ROLAP Servers
> MOLAP Servers
##### SQL Extensions 
> Extended aggregate functions
> Reporting features
> Multiple Group-By
#### 2.8 Conclusion
##### Pros
> Data Warehousing
> Key aspect
> Natural when data lives in OLTP database already
> Data integration across department or entire organization
##### Cons
> Raw "Big Data" often comes in real-time streams
> What data matters and which ones not it not known beforehand
> Need support for experimentation over the raw data
> Machine Data Analytics: different requirements than OLAP

### 3. Storage
#### 3.1 File Systems
##### 3.1.1 Google File System
> Raw Data
> Derived Data: operations on raw data
> GFS: characterstics for data-intense computation
> File Systems for Big Data
    Fault tolerance and robustness
    Big files
    Append write
    Semantics and primitives
    Performance
    GFS and HDFS
    GFS Architecture
        cluster, master, chunservers
    System Components: 
        Chunks
            basic block
            fixed size
        Chunkservers
        Single Master
        Client code
##### 3.1.2 Metadata Management
    Master Metadata and Policies
        Master: manages three tyeps of metadata
            1. names of files, 2. mapping from files to chunks, 3. chunk locations
        Locks
    Master Tasks
        data replication
            data reliability, data availability, maximize bandwidth utilization
##### 3.1.3 Replication and Consistency
    Consistency Model
        consistent, defined
    Sequentializing Mutations
        Primary chunkserver
    Snapshoting
##### 3.1.4 Hadoop File System
    NameNode = master, DataNode = Chunkserver, Data blocks = chunks
    Missing concurrent appends
    HDFS, Replica Placement
     > Issues to consider: reliability, read/write bandwidth, block distribution
     > Hadoop's default strategy for replica placement
       > 1st: on client node (or randomly chosen if client outside cluster)
       > 2nd: random, off-rack
       > 3rd: same rack as second, different node
       > more replicas: randomly chosen
    Tools for Ingesting Data into HDFS
        Apache Flume
            Streaming data
        Apache Sqoop
            Structured, RDBS
#### 3.2 Key-Value Stores
##### 3.2.1 Distributed Hash Tables
    Consistent Hashing
    The Chord Ring
    Routing with Finger Tables
        O(log k)
    Pros
     - Highly scalable
       - automatically distributes load to new nodes
     - Robust against node failure
       - data automatically migrated away from failed nodes
     - Self organizing
       - no need for a central server
    Cons
     - Lookup, no search
       - hashing does not preserve namesapce topology
     - Data integrity
       - hard to verify
     - Security problems (P2P)
##### 3.2.2 Weak Consistency
     Idea consistency: when an update is made, all observers see that update
     High Level Perspective: 
        Strong consistency, 
          - An updates data object -> all subsequent access will return this value
        Weak consistency  
          - Some conditions need to be met, before updated value is returned
          - Temporal inconsistency window
        Eventual consistency (increasing availability)
          - Special form of weak consistency
          - Maximum size of inconsistency window can be determined
     Client-side View: Causal consistency, read-your-writes consistency, session consistency, monotonic read consistency, monotonic write consistency
     Server-side View
        Key design parameters
            W + R > N, strong consistency
            W + R <=N, weak consistency
        ?Stickiness of sessions? (support read-your-writes, session, and monotonic consistency)
            - clients are guaranteed to talk to same server -> easy to guarantee
            - slight disadvantages for load balancing and fault tolerance
        Network Partitions
            - implement a merging scheme after partitions have healed
##### 3.2.3 Dynamo: Highly Available Key-Value Store
     Motivation: shopping carts 
       > "write always"
       > available across multiple data centers
     High availability -> weak consistency 
     Service-Oriented Architecture
     Design Considerations & Principles
     Dynamo's DHT
     Versioning
        Version Clock
    Handling Failures: Hinted Hand-offs
        - works well if system membership churn is low and node failures are transient
        - additional mechanism to actively detect inconsistencies
        - additional protocols to deal with node membership management
#### 3.3 Big Tables
##### 3.3.1 Column Stores
    Limitations of RDBM
        Heavy use of joins, stored procedures, transactions
    Advantage of Column Stores
        data locality, compression, simd instructions
    Disadvantage of Column Stores
        every query involves a join of the columns
        need to "materialize" tuples; copy data
        every insert involves n inserts(n columns)
##### 3.3.2 Google's Big Table
    BigTable is Sparse, distributed, persistent multidimensional sorted map
    Map, Sorted, Multidimensional, Persistent, Distributed, Sparse
    What is a Bit Table?
        row and column keys, timestamps
        Data model: uniterpreeted (bytes or strings)
        A sparse map: BT: (row:string, col:string, time:int64) -> string
    Big Web Table
    Big Table vs. Relational Database
    Tablets
        Units of data distribution and load balancing
    Data Locality
    Column Namespace - two level hierarchy
        Top level: Column Families
        Bottom level: individual column keys
    Putting it together
        SortedMap<RowKey, List<SortedMap<Column, List<Value, Timestamp>>>>
##### 3.3.3 HBase
    HBase vs. BitTable
        almost same
        BT allows mapping files to main memory
        BT allows for locality groups
        Different compression algorithms
        Minor differences: caching, log management
    Tablet/Region Server: Internal Architecture
    HBase API: write operations
    RPC = Remote Procedure Call
    HBase: Compare and Check
    HBase: Get
    HBase: Scans
        iterator
#### 3.4 Global Databases
##### 3.4.1 Time Stamping
    External Consistency vis Timestamps
    Timestamp w/ Global Clock
    Timestamp Invariants
        Timestamp order = commit order
    TrueTime (Google Spanner)
        commit wait
        Implementation now epsilon
##### 3.4.2 Google's Spanner
    What is Spanner
        Distributed multiversion database
        Bring Bit Data and RDMBS together
    Global Data Distribution Challenge
    Features
        Lock-free distributed read transactions
    Lock-Free Distributed Read via Snapshoting
        Avoid blocking writes on all nodes
        Create snapshots using time stamp
        Blocking all nodes at global scale would be completely impractical
        Create a local snapshot from version defines via global time stamp
#### 3.5 Big Query Engines
##### 3.5.1 Protocol Buffers 
    Raw Data Representation
        RDMS: rigid data schemata
        XML
            - expensive parse
        JSON
            - compacter than xml, faster parsing
    Universal Nested Data Representation: Protocol Buffers
        Flexible, efficient, automated mechanism for serializing structured data
            Fields: name, type, numbered
            required, repeated, optional
            enumeration types
            nested records
##### 3.5.2 Google's Big Query
    Dremel is scalable, interactive ad-hoc query system for analytics of read-only nested data
    In-Situ Processing: Data Model
    Repetition Levels
        r(F) \in {0,...n}
    Definition Levels
    Lossless Columnar Representation
    Record Assembly 
    Query Language
        restricted set of SQL
    Query Execution
##### 3.5.3 Pig and Pig Latin
    A not-so-foreign language for Data processing: Pig Latin: Data Model
    Inverted term index
        Map<documentId, Set<positions> >
        term_info: (termId, termString, ...)
        position_info: (termId, documentId, position)
    Commands
        LOAD, FOREACH, FILTER, (CO-)GROUP, JOIN, UNION, CROSS, ORDER, DISTINCT, GENERATE, STORE
    Atom, Tuple, Bag, Map
    Pig: Query Execution
##### 3.5.4 Hive
    A warehousing solution over a map-reduce framework
    HiveQL
    Three-level hierarchy
        Tables
        Partitions
        Bucket
    Data Model
        Tables
        Partitions
        Bucket
    Other Execution Platforms
        Red Shift
        Impala
        Shark
#### 3.6 Conclution
    GFS, Dynamo, BigTable, Spanner

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
    "old school" classification: SISD, MISD, SIMD, MIMD
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
**Gutafson's Law**: S(n) = B + n(1-B) = n-B(n-1) =(n->infi) (1-B)n
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
    Message Passing for Parallelism
    MPI Brief History
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
    Functional Programming: map and reduce
##### 5.2.2 Paradigm
    High Level Structure of MapReduce
##### 5.2.3 MapReduce Parallelism
    Task Breakup
    Task Granularity
    Perform pre-aggregation before data shuffling _Combiner_ step
##### 5.2.4 MapReduce Execution
    Data Flow
    Master Coordination
        master node
#### 5.3 Algorithms in MapReduce
##### 5.3.1 Matrix Computations in MapReduce
    Vector-Matrix Multiplication
    Mapper = Multiplier, Reducer = Adder
    PageRank
        Computes quality score for each Web page based on link structure
        a_ij = 1, link from pj to pi
        o_j = \sum_i a_ij
        Markov Chain over state space (1,...,n)
            p_ij = beta * aij/oj + (1-beta)/n
        Power Method
        MapReduce
    Matrix Multiplication with Block Partitioning
        k^2 square blocks
        Improved Matrix Multiplication
    PageRank via MapReduce: Final Remarks
##### 5.3.2 SQL-style Computations in MapReduce
    Selection and Projection
        Selection: mapper and reducer
        Projection: mapper, reducer
    Set Operations: union, intersection, difference
    Bag Operations: union, itersection, difference
        Multisets
    Natural Joins 
    Grouping and Aggregation
    Matrix Multiplication via Relational Operations
        Two stages
        One stage
##### 5.3.3 Machine Learning with MapReduce
    K-means Clustering
        Each iteration = one MapReduce
    Gradient-based Machine Learning
##### 5.3.4 Complexity Theory for MapReduce
    Communication cost of a task: size of the input (in bytes)
    Communication cost of a computation: sum of communication costs of its tasks
    Two-way join, three-way join with hashing
    Function Workflows
    Communication Cost for map tasks: r+s
    Communication Cost for reduce tasks: O(r+s)
    Reducer size: q = maximal length of value list associated with a single key
        - practical relevance: should not exceed memory of a node
    Replication rate r: # key-value pairs in map / #inputs
        - average communication from Map tasks to Reduce tasks
    Similarity Join 
        Find all similar pairs in set X (e.g. set of images)
        q = 2n / g???
    Lower bound for given reducer size
        \sum_{i=1}^k q_i^2>=n^2
#### 5.5 Resilient Distributed Datasets & SPARK
    Challenge: abstractions for leveraging _distributed memory_
    Motivation & Overview
        RDDs: parallel data structures
    Alternating Least Square
    |Aspect |RDDs   |Distrib. Shared Memory
    |Reads  |Coarse- or fine-grained |Fine-grained
    |Writes |Coarse-grained |Fine-grained
    |Consistency |Trivial (immutable) |Up to app / runtime
    |Fault recovery |Fine-grained and low-overhead using lineage    |Requires checkpoints and rollback
    |Straggler mitigation   |Using backup tasks |Difficult
    |Work placement |Automatic based on data locality   |Up to app
    |Behavior if not enough RAM |Benign degradation |Poor (swapping?)

    RDD = read-only, partitioned collection of records

### 8 Computing with Data Streams
#### 8.1 Introduction & Motivation
##### 8.1.1 Streaming Data Paradigm
    Streaming Data
##### 8.1.2 Example: Logs Processing
    Web Search Logs
    Click Through Prediction
    Apache Kafka
        Kafka maintains feeds of messages in categories called _topics_
        Data Management
            Partition structure
            Partition management
        Consuming Data
##### 8.1.3 Example: Internew Traffic Monitoring and Frequent Items
    Misra-Gries Algorithm (Tetris Algo)
        Approximately find the most frequent items
        Analysis
##### 8.1.4 Other Examples
    Smart Meters
        > deployed by utility companies
        > power sensors
        > data processing
    On-line Video
        > Staggering amounts of video footage
        Example: Content ID system
#### 8.2 Streaming Data Management
##### 8.2.1 Streaming DBMS
    Aurora, Borealis
##### 8.2.2 Apache Kafka
    Logs processing
##### 8.2.3 Apache Storm
    Three types of abstractions
        Spout: source data stream
        Bolt: stream processor
        Topology: a network of spouts and bolts
##### 8.2.4 Spark Streaming
    Discretized Streams
##### 8.2.5 (Small) Streams in Programming Languages
#### 8.3 Sampling and Sample-based Estimation
##### 8.3.1 Introduction
    Pros
        - conceptual simplicity, ease of implementation
        - developed statistical theory & methodology
        - typically: model-free (but: not assumption free)
        - adaptivity and trade-off of accuracy and effort/resources
    Cons
        - not suitable, if answer depends on a few items (e.g. maximum)
        - can be expensive, relative to other methods (see later: sketching)
        - randomization effects, sampling noise may be hard to quantify
        - difficult to make work in general for complex (SQL-style) queries
    Notation and Formalization
    (Sub-)Sampling
        - _with_ replacement
        - _without_ replacement
##### 8.3.2 Sampling
    Bernoulli Sampling
    Poisson Sampling: generalization of Bernoulli sampling
    Stratified Sampling?
    Reservoir Samping
    Longitudinal Sampling?
##### 8.3.3 Estimation
    Statistical Estimators
    Empirical Mean Estimator
        Unibasedness, variance
        Concentration
            Chebychev's inequality
        Confidence Intervals for Normal Distributions
        Central Limit Theorem
    Mean Estimator
    Non-uniform Sampling: Hansen-Hurwitz Estimator?
        with replacement
    Neyman Allocation for Stratified Sampling
    Bootstrap
#### 8.4 Data Sketching
#### 8.4.1 Bloom Filters
    Data Sketching
    False positive rate
        f(k*;n,m)=(1/2)^k* = (0.6185)^{n/m}
    Partitioned Bloom Filter
#### 8.4.2 Count-Min Sketch
    From Filtering to Counting
    Updating the Count Sketch
    Inner Product Queries
    Range Queries
    Heavy Hitters
#### 8.4.3 Count-Mean Sketch
#### 8.4.4 Counting Distinct Elements
    Cardinality Estimation via Linear Counting
    LogLog Counting
    HyperLogLog Counting
#### 8.4.5 L2-Norm Estimation
    Frequency Moment Estimation
    Tug of War Sketch
    Final Tug of War Sketch
    Random Projections

### 12. Semi-Structured Data
    NoSQL Data Stores
        Key-value stores
        Triple stores
        Document Stores
        Column Stores
    Semi-Structured Documents
#### Part 1: Syntax
    Well-formedness
    one syntax = one language 
        yes D is well-formed
        no D is not well-formed
    XML vs JSON
    Element
        opening tag, closing tag, empty tag
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
    Default Namespace xmlns

    JSON
        String
        Number
        Boolean
        Null
        Array
        Object

    HTML Elements
        Void: <br>
        Raw text: 
            <style> 
                body {color: black; background: white;} 
                em {foot-style:normal;color:red;}
            </style>
        Escapable raw text: 
            <title> 
                This is a &quot;title&quot; 
            </title>
        Foreign
        Normal
            <ol>
                <li>ett</li>
            </ol>
    XHTML Syntax
    YAML
#### Part 2: Data Models
    Syntax vs. Data Models
        Physical view vs. Logical view
    Data models
        XML Infoset
            Document (doc), Element (metadata), Attribute (xmlns:dc), Processing Instruction, Character, Comment, Namespace (dc->systems, xml->ns), Unexpanded Entity Reference, DTD, Unparsed Entity, Notation
        Post-Schema-Validation Infoset (PSVI)
            Infoset + Types
        XQuery ans XPath Data Model (XDM)
            Sequences
            7 nodes: Document, ...
        Document Object Model (DOM)
    Types (General):
        Atomic Types 
            Strings, Numbers, Booleans, Dates and Times, Time Intervals, Binaries, Null
            Lexical Space vs. Value Space
        Structured Types
            Data Structure
                Associate Arrays (a.k.a. maps)
                Ordered Lists
            Cardinality: one, *, ?, +
    Protocol Buffers
    Scalar types
        double, float, int32, int64, bool, string, bytes
        enum Title { MR=1;MS=2;MRS=3;}
    Validation
    Document Type Declaration
        - <!DOCTYPE a>
        - <!DOCTYPE a [
          ]>
        - External Subset: <!DOCTYPE root SYSTEM "schema.dtd">
    Element Type Declaration
        <!ELEMENT a (#PCDATA)>
        <!ELEMENT a (foo, bar)>
    Attribute-List Declaration
        <!ATTLIST a foo CDATA #REQUIRED>
        <!ATTLIST a foo CDATA #IMPLIED>
        <!ATTLIST a foo CDATA "bar">
        <!ATTLIST a foo NMTOKEN #REQUIRED> 
    XML SCHEMA
        Simple Typs: Built-in
        Strings: string, anyURI, QName
        Numbers: decimal, integer, float, double, long int short byte, positiveInteger nonNegativeInteger..., unsignedLong unsignedInt..
        Boolans: boolean
        Dates and Times: dateTime, time, date, gYearMonth
        Time Intervals: duration, yearMonthDuration
        Binaries: hexBinary base64Binary
        Null: -
        Restriction: <xs:restriction base="">... </xs:restriction>
        List: <xs:list itemType=""/>
        Union: <xs:union memberTypes="xs:.."/>
#### Part 3: Querying
    Declarative Languages: What vs. How
    Language Ecosystem
        |           |XML        |JSON
        |Navigation |XPATH      |JSONPath, JSONSelect
        |Transform  |XSLT       |JSONT
        |Query      |XQuery 1.0/3.0 |Xquery 3.1, JSONiq
        |Update, Scripting  |XQuery Update Facility & Scripting |JSONiq
    XML Navigation (XPATH, XQUERY)
        slash operator: doc("")/child::countries/child::country
        All Axes: self:: attribute::, child::, descendant::, parent::, ancestor::..
        Filters: eq
        Joker:
        Kind test:
    Construction (XPATH, XQUERY)
        String escaping
        Numbers
        Booleans
        Other Simple Types: date, dateTime, long
    XQUERY: Basic operations
        concatenation: , 
        range: 1 to 100
        addition, substraction, multiplication, division, integer division(idiv), module(mod)
        String: concatenation:||, concat, string-join(), sub-string: substr, length: string-length
        Value Comparison: equality, inequality, greater than, less than
        Logics: conjunction, disjunction, not, universal, ...
        Effective Boolean Value: "", "foo", 0, 42, ()..
        Composability: for $i in 1 to 10 return string($i)
    Data Flow (XQUERY)
    Try Catch
    FlWOR Expressions (XQUERY)
        Let clauses
        For clauses
        Where clauses
        Order by clauses
        Gropu by clauses
    Types
        xs:integer
        element(foo)
    Querying JSON (XQUERY 3.1/ JSONIQ)
#### Part 4: Document Stores
    Encoding: character 0s and 1s
    Common Character Encodings: ASCII, UTF-8
    BSON
    Relational table
    Common approaches
        Schema-based shredding
        Edge shredding
        Tree encoding
    Impedance mismatch
    Document Stores vs. RDBMS 
        +Projection, +Selection, -Joins
        CRUD: Create, Read, Update, Delete
#### Part 5: Graph Databases (Intro)
    Resource Description Framework: RDF
    RDF Format: RDF/XML, Turtle, JSON-LD, RDFa, N-Triples
    Self-awareness
    SPARQL
    IRI, Literal, Blank node: subject, property, object
    Entailment (and Syllogisms)

# Vocabulary
    1. Gyroscope 陀螺仪, barometer 气压计；睛雨表
    3. Motto 座右铭，格言, observatory 天文台, stale 陈腐的, rack, quorum 法定人数, in-situ 原位, colossus 巨像，巨人, provenance 出处，起源
    5. granularity 间隔尺寸, straddlers, brittle 易碎的，脆弱的
    8. sketching 草图；写生；写生画

