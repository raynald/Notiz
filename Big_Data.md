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
 - Non-Volatile: physically separate store of transformed data, separated from operational system
 - Why materialize in advance: scattered across many dabases, consilidation is easier via re-materialization, central access across all databases not available
 Non-technical Challenges: semantics
 Mini-warehouses: Data marts
 Metadata Management
    Metadata: defines warehouse objects
       structure of datawarehouse, schema, view, dimensions 
       data mart locaitons
    Operational meta-data
        data lineage
        monitoring information
#### 2.4 ETL: Extract, Transform, Load
 - Extract: converting data from source systems to consolidated data format
       OLTP: triggers
       Information builders: Oracle Open Connect, Sybase Enterprise Connect...
       **Incremental updates** via replication servers
 - Transform 
        applying business rules
        replace string gender by "sex"
        data cleaning and validation
 - Load
        Checking integrity
        Bulding indices
        loading the data to data warehouse
  ETL vs. modern Big Data
     collides: speed, philosophy, copying
#### 2.5 OLAP via ROLAP
 ROLAP = relational OLAP, support star/snowflake schemas
 MOLAP = multidimensional OLAP, data cube

> Star Schema
  - Fact table: a very large accumulation of facts
  - Measures: quantities of interest (sales, price)
  - Dimension tables: static information about the entities involved in the facts
> Snowflake Schema, multi-level
  - De-normalizing dimension tables not always feasible/optimal
  - Tradeoff: more compactness/less redundancy vs. higher costs of joining
  - Alternative: normalize dimension tables, introduce additional tables

#### 2.6 Data Cubes
 MOLAP and Data Cubes
 Data Cube: Marginal
    Aggregations over multiple dimensions
 Data Cube: Pivoting
    Select some dimensions, then aggregate measure
 Data Cube
    Drill-Down: split dimension into finer detail
    Roll-up: aggregate along one or more dimension
    Slice & Dice: projecting data on subset of dimensions for selected values of the other dimensions
#### 2.7 OLAP Servers
##### Inex Structures
> Bitmap indices
 - AND, OR, SUM
> Join indices
##### Materialized Views
> pre-compute aggregates
##### Server Architectures
> Specialized SQL Servers
    advanced SQL support for star and snowflake schema
> ROLAP Servers
> MOLAP Servers
##### SQL Extensions 
> Extended aggregate functions
    rank, percentile, median
> Reporting features
    time window
> Multiple Group-By
#### 2.8 Conclusion
##### Pros
> Data Warehousing
> Key aspect: ETL into RDBMs
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
> GFS. diff: characterstics for data-intense computation
> File Systems for Big Data
    Fault tolerance and robustness
    Big files, large size, large number
    Append write
    Semantics and primitives
        atomicity: all occur or no occur
        keep synchronization overhead minimum
    Performance: bandwidth/throughput, latency and response times
    GFS and HDFS
    GFS Architecture
        a cluster: master, chunservers
    System Components: 
        Chunks
            basic block
            fixed size
            immutable 64bit chunk handle
        Chunkservers
        Single Master
            maintain metadata
            namespace, access control information
        Client code
            interact with master
            linked into each application
##### 3.1.2 Metadata Management
    Master Metadata and Policies
        Master: manages three tyeps of metadata
            1. names of files, 2. mapping from files to chunks, 3. chunk locations
        Locks, read lock and write lock
    Master Tasks: deal with data replication
            why: data reliability, data availability, maximize bandwidth utilization
    Metadata Consistency
        GFS2: Colossus
            Next-generation cluster-level file system
            automated sharded metadata layer
##### 3.1.3 Replication and Consistency
    Consistency Model
        consistent: readers see same data across replicas
        defined: clients always see what the mutaiton writes in its entirety
        inconsistent
        methods: 1. same order of mutations, 2. chunk version nubmers 3. restoring lost replicas from existing ones
    Sequentializing Mutations
        Select a primary chunkserver
        Primary chunk server gets a lease (expiraiton, revoking)
        Concurrent writes
        Special support for atomic append mutations
        Snapshoting
            copy-on-write, master revokes all leases
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
    decentralized, common P2P
    put(key, value), get(key)
    Consistent Hashing: Chord
        Each node is responsible for storing keys "near" its ID
        replication at multiple successors
    The Chord Ring: add and remove
    Routing with Finger Tables: O(log k)
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
          DNS
     Client-side View: Causal consistency, read-your-writes consistency, session consistency, monotonic read consistency, monotonic write consistency
     Server-side View
        Key design parameters
            N: number of replica, W number of replicas need ackowledge, R number of replicas on a read access
            W + R > N, strong consistency
                problem: < W replicas available, write fails
            W + R <=N, weak consistency
        Stickiness of sessions (support read-your-writes, session, and monotonic consistency)
            - clients are guaranteed to talk to same server -> easy to guarantee
            - slight disadvantages for load balancing and fault tolerance
        Network Partitions
            - requirements: partitions remain available
            - implement a merging scheme after partitions have healed
##### 3.2.3 Dynamo: Highly Available Key-Value Store
     Motivation: shopping carts 
       > "write always"
       > available across multiple data centers
     Decentralized, loosly-coupled, service-oriented system
     High availability -> weak consistency 
     Key-value store
        Read and write access via primary key (only)
        Store values as binary objects = "blobs"
        No need for data schemas and RDBMS
     Amazon's Service-Oriented Architecture
     Dynamo: Design Considerations & Principles
        incremental scalability
        symmmetry and decentralization
        heterogeneity
     Dynamo's DHT
        Virtual nodes
        Replication
        Preference list
     Versioning
        Version Clock
     Executing put and get Requests
        Coordinator
     Handling Failures: Hinted Hand-offs
        - works well if system membership churn is low and node failures are transient
        - additional mechanism to actively detect inconsistencies
        - additional protocols to deal with node membership management
#### 3.3 Big Tables
##### 3.3.1 Column Stores
    Limitations of RDBM
        Heavy use of joins, stored procedures, transactions
        No SQL: stop support for secondary indexes, only have primary keys, no fixed schema 
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
        partion the row set into sub-ranges called 'tablets'
        Units of data distribution and load balancing
    Data Locality
        read and write atomic
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
        transfer data from client to server and back
        writing large volumes of data: Write buffers
    HBase API: Compare and Check
    HBase API: Get
    HBase API: Scans
        iterator
#### 3.4 Global Databases
##### 3.4.1 Time Stamping
    External Consistency vis Timestamps
    Timestamp w/ Global Clock
    Timestamp Invariants
        Timestamp order = commit order
        Timestamp order respects global wall-time order
    TrueTime (Google Spanner)
        "Global wall-clock time" with _bounded uncertainty_
        only localized within an interval
        commit wait
            wait to "sit out" temporal uncertainty
    TrueTime Architecture
    TrueTime Implementation
        now = reference now + local-lock offset
        epsilon = reference epsilon + worst-case local-clock drift
##### 3.4.2 Google's Spanner
    What is Spanner
        Distributed multiversion database
        Bring Bit Data and RDMBS together
    Global Data Distribution Challenge
        Shard data on a global scale
        Shard across 1000x of nodes
        Replicate across data centers
        social data
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
        Raw data: comes as structured data
        Desiderata
            process in situ, data exchange, serialize, update definitions
        XML
            - expensive parse
        JSON
            - compacter than xml, faster parsing
    Universal Nested Data Representation: Protocol Buffers is Flexible, efficient, automated mechanism for serializing structured data
        Fields: name, type, numbered
        required, repeated, optional
        enumeration types
        nested records
##### 3.5.2 Google's Big Query
    Dremel is scalable, interactive ad-hoc query system for analytics of read-only nested data
        restricted SQL-dialect
    In-Situ Processing: Data Model
        Fields can be, unique(n=1), optional(n=0,1), repeated(n>=0)
    Repetition Levels
        Goal: breaking up records into column-based representation
        r(F) \in {0,...n}, NULL represent omitted values
        Example???
    Definition Levels
        How many optional/repeated fields in path are present in the record
        Example???
    Lossless Columnar Representation
        preserve record structure losslessly
        Encoding:
            NULL: definiton level < number of repeated + optional fields
    Record Assembly 
        only assemble partial records that are needed
        FSA
    Query Language
        restricted set of SQL: type of joins
    Query Execution
        Example: one-pass aggregations
##### 3.5.3 Pig and Pig Latin
    A not-so-foreign language for Data processing: Pig Latin: Data Model
    Data Model: flexible, fully nested data model
        Inverted term index
            Map<documentId, Set<positions> >
        term_info: (termId, termString, ...)
        position_info: (termId, documentId, position)
    Commands
        LOAD, FOREACH, FILTER, (CO-)GROUP, JOIN, UNION, CROSS, ORDER, DISTINCT, GENERATE, STORE
    Query Execution
        Lazy evaluation of commands -> logical plan
        Compiled to MapReduce
    Atom, Tuple, Bag, Map
    Pig: Query Execution
##### 3.5.4 Hive
    A warehousing solution over a map-reduce framework
    SQL dialect: HiveQL, compiled to MapReduce
    Three-level hierarchy
        Tables: each table <-> HDFS directory
        Partitions: sub-directory
        Bucket: hash of a column in table, buckets map to files
    Hive: Query Language
        Select, project, join, aggregate, union, all, sub-queries in from clause
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
    Single Data, Multiple Data
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
        Grid: heterogenous computers across internet
        Cluster: mostly homogeneous computers in "one room"
    Message Passing for Parallelism
        synchronization
        movement of data between address spaces
    Message Passing Interface(MPI): MPI-1, MPI-2, MPI-3
        Pros: standardization, portablity, functionality
        Cons: parallelism is explicit, no support for fault-tolerance
##### 5.1.4 Grid Computing
    Message Passing for Parallelism
    MPI Brief History
    MPI: What   
        Language-independent communication protocol to build paralle applications within and across machines
        Interface to define virtual topology, synchronization, and communication between processes
        Point-to-point rendezvous-type send/receive operations
        Used for distributed memory parallelism (shared-nothing systems)
        Low-level, highly efficient
    MPI: Basic Concepts
        Processors can be collected into *groups*
        Groups can have contexts, called *colors*
        Communicator = group + color
        Each process has a rank within each communicator
        Sender has to know whom, what, Reciver has to know who, what
##### 5.1.5 Discussion 
    High Performance Computing
        Hardware: supercomputer, communication: fast interconnections
        Software: Specialized (Parallel) Programming Tools, MPI, OpenMP
    Pros: Speed! 
    Cons: Difficult to deal with errors and delays
    In Contrast: Commodity Hardware Clusters
        Hardware, Software: MapReduce
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
    skew: reducers' execution time
        1. random grouping reducers into tasks
        2. more tasks than nodes -> higher utilization
    Task Granularity
        map tasks >> machines
        minimize time for fault recovery
        pipeline shuffling with map execution
        better dynamic load balancing
    Refinement: Combiners
        Perform pre-aggregation before data shuffling _Combiner_ step
##### 5.2.4 MapReduce Execution
    Data Flow
        scheduler tries to schedule map tasks close to physical location of input data
    Master Coordination
        Each MapReduce is coordinates by a master node
        Master pings worker periodically
        Identify *straddlers跨车*
            towards end of MR: start additional redundant tasks
            see which one finishes first
#### 5.3 Algorithms in MapReduce
##### 5.3.1 Matrix Computations in MapReduce
    Vector-Matrix Multiplication
        Mapper = Multiplier, Reducer = Adder
    PageRank
        Computes quality score for each Web page based on link structure
        a_ij = 1, link from pj to pi
        o_j = \sum_i a_ij
        Markov Chain over state space (1,...,n)
            transition matrix: p_ij = beta * aij/oj + (1-beta)/n
        Power Method
            iterative matrix multiplication
            Improvement: block partitioning of matrix P
    Matrix Multiplication with Block Partitioning
        k^2 square blocks
            Column Strip vs Block Partitioning
        Improved Matrix Multiplication
    PageRank via MapReduce: Final Remarks
        Matrix encoding: adjacency matrix -> column sums
##### 5.3.2 SQL-style Computations in MapReduce
    Selection and Projection
        Selection: mapper and reducer
        Projection: mapper, reducer
    Set Operations: union, intersection, difference
        > mapper (X, t) -> (t, 1)  
        > reducer (t,v) -> (t, \emptyset)
    Bag Operations: multisets
        union, itersection, difference
    Natural Joins 
    Grouping and Aggregation
        (a,b,c)->(a,b)
    Matrix Multiplication via Relational Operations
        Two stages
        One stage
            m_ij -> ((i,k), (j,m_ij)) for each k
            n_jk -> ((i,k), (j, n_jk)) for each i
##### 5.3.3 Machine Learning with MapReduce
    K-means Clustering
        Each iteration = one MapReduce
    Gradient-based Machine Learning
        map: i-> (0,     )
##### 5.3.4 Complexity Theory for MapReduce
    Function Workflows
    Communication cost of a task: size of the input (in bytes)
    Communication cost of a computation: sum of communication costs of its tasks
    Two-way join
        Communication Cost for map tasks: r+s
        Communication Cost for reduce tasks: O(r+s) (after aggregation)
    Three-way join = 2xTwo-Way Join
        (R X S) X T O(r+s+t+p*r*s)
    Three-way join with hashing
        hash B to u buckets and C to v buckets
        (b,c)\in S -> ((g(b), h(c)), ("S",b,c)
        (a,b)\in R -> (g(b),i), ("R", a,b), i=1..u
        (c,d)\in T -> (j,h(c)), ("T", c,d), j=1..v
        s + ur + vt
        Langrange funciton optimize #buckets
            L(u,v,\lambda) = ur + s + vt - \lambda (uv - k)
            s + 2\sqrt(krt)
    Example: facebook
    Star Joins
    Complexity Theory of MapReduce
        Reducer size: q = maximal length of value list associated with a single key
            - practical relevance: should not exceed memory of a node
        Replication rate r: # key-value pairs in map / #inputs
            - average communication from Map tasks to Reduce tasks
        One-Pass Matrix Multiplication, qr >= 2n^2
        Similarity Join 
            1. Find all similar pairs in set X (e.g. set of images)
                reduce sizse q = 2, replication r = n-1
            2. (i, x_i) -> {({group(i),v}, (i,x_i))
                r = g-1, q = 2n / g
            Lower bound for given reducer size
                \sum_{i=1}^k q_i^2>=n^2
                r>=n/q, r = 2 n/q, optimal with factor 2
#### 5.5 Resilient Distributed Datasets & SPARK
    Challenge: abstractions for leveraging _distributed memory_
        Iterative Apps, 90% of time doing I/O
        PageRank: repeatedly multiply sparse matrix and vector
    Motivation & Overview
        RDDs: parallel data structures
            fault tolerance (by lineage graph)
            control over partitioning and replication
            persist intermediate results in memory
            rich set of operators for data manipulation
    |Aspect |RDDs   |Distrib. Shared Memory
    |Reads  |Coarse- or fine-grained |Fine-grained
    |Writes |Coarse-grained |Fine-grained
    |Consistency |Trivial (immutable) |Up to app / runtime
    |Fault recovery |Fine-grained and low-overhead using lineage    |Requires checkpoints and rollback
    |Straggler mitigation   |Using backup tasks |Difficult
    |Work placement |Automatic based on data locality   |Up to app
    |Behavior if not enough RAM |Benign degradation |Poor (swapping)

    RDD = read-only, partitioned collection of records
        (1) data in stable storage or (2) other RDDs
    Spark Runtime
        Driver program connects to a cluster of workers
    Alternating Least Square

### 8 Computing with Data Streams
#### 8.1 Introduction & Motivation
##### 8.1.1 Streaming Data Paradigm
    Streaming Data
        Storage space
##### 8.1.2 Example: Logs Processing
    Web Search Logs
        impressions, queries, clicks
    Click Through Prediction
        Subsampling, dynamically changing query/click distribution
        Advertisement
    Apache Kafka: high throughput log management tool / message broker
        Distributed, partitioned, replicated commit log service
        Kafka maintains feeds of messages in categories called _topics_
        producers, subscribers, broker: intermediate server, communication via TCP
        Data Management
            Partition structure
            Partition management
        Consuming Data
            each consumer instance is part of a consumer group
            message deliverd to one consumer instance within each subscribing consumer group
            can have as amny consumers per group as partitions
##### 8.1.3 Example: Internew Traffic Monitoring and Frequent Items
    Misra-Gries Algorithm (Tetris Algo)
        Approximately find the most frequent items
        Analysis
            fj - m/k <= A[j]
            if fj > m/k then j\in key(A)
##### 8.1.4 Other Examples
    Smart Meters
        > deployed by utility companies
            predict energy needs, detect faults and fraud
            provide customers insights into usage patterns
        > power sensors
            each device: emasurements of instantaneous power consumption
            maximal sampling rates: around 1MHz
            on-device subsampling and averaging capabilities
        > data processing
    On-line Video
        > Staggering amounts of video footage
        Example: Content ID system
            similarity join
            comparison with reduced representation
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
    How is data grouped between bolts/spouts?
        shuffleGrouping
        fieldsGrouping
##### 8.2.4 Spark Streaming
    Many input sources
    Input stream is divided into batches
    Discretized Streams
##### 8.2.5 (Small) Streams in Programming Languages
    Streams in Scala = lazily evaluated list
    Streams in Java 8 
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
        Sample space S
        Data X \in S
    (Sub-)Sampling
        Based on sample size!!!
        - _with_ replacement
        - _without_ replacement
##### 8.3.2 Sampling
    Bernoulli Sampling: without replacement
    Poisson Sampling: generalization of Bernoulli sampling
        without replacement
    Stratified Sampling
        Partition data set into m *strata*
        x<12?1,  x<18?2, otherwise 0
    Reservoir Samping
    Longitudinal Sampling: randomly sample a set of users
        Extra memory (inconvenient)
        Requires to sample users in a coordinated manner (bad!)
        given IDs, compute X(a) = {x:x.key=a}
        Hash
            deterministic approximation on randomness
##### 8.3.3 Estimation
    Estimator - a random function
    Statistical Estimators
        bias:  E_S f_head - Ef
        Var[f_head] = E_S[f_head(S) - E_S f_head]^2
        MSE[f_head] = E_S[f_head(S) - E f]^2
        MSE[f_head] = Var[f_head] + bias[f_head]^2
    Empirical Mean Estimator
        Law of large numbers
        Unibasedness: formula???  
        variance: formula???  sigma^2 / n
        Concentration
            Chebychev's inequality P{|X-E X|\le k sigma} \le 1 / k^2
            \sigma^2 / n / \epsilon^2
        Confidence Intervals for Normal Distributions
            ^Y = 1/n \sum_{i=1}^n Y_i
            S^2 = 1/(n-1) \sum_{i=1}^n (Y_i - ^Y)^2
            pivotal quantity: T = \sqrt(n) (^Y- \mu) / S
                not depends on \mu, by sample size n 
                P{-c<=T<\c} = 1 - \sigma
                P{^Y-cs / \sqrt(n) \le \mu \le ^Y + cs / \sqrt(n)} = 1 - \sigma
        Central Limit Theorem
            Assume Y_i are i.i.d from some distribution with mean \mu and variance \sigma^2 < infi
            Lindeberg-Levy: \sqrt(n)((1/n \sum_{i=1}^n Y_i) - \mu) -> N(0, \sigma^2)
    Mean Estimator: Sampling without replacement
        Var[^f] = 1 / n ^ 2 \sum_i,j} f_i f_j = \sigma^2/n + (n-1)/n Cov(f_1, f_2)
        i.i.d Cov(f_1, f_2) = 0, here < 0
        Cov(f_1, f_2) = - sigma^2 / (N-1)
        improvement 1 - (n-1)/(N-1)
    Non-uniform Sampling: Hansen-Hurwitz Estimator
        with replacement
        sample size n
        ^f_HH = 1 / n \sum_{i=1}^n f_i / N / pi_i
        Unbiasedness: formula
        Variance: formula
    Horvitz-Thompson Estimator
        without replacement
        ^f_HT = 1 / N \sum_{i=1}^N Z_i/pi_i f_i
        Unbiasedness: formula
        Variance hard to compute
    Neyman Allocation for Stratified Sampling
        ^f formula
        Var formula
            Variance reduction
        Optimal Neyman allocation scheme
            n_k = n \frac{N_k \sigma_k^2}{ \sum_{l=1}^{m} N_l \sigma_l^2}
    Example
        Body weight
        Click-through rates
    Bootstrap sampling
        Estimate sampling variance from bootstrap samples
    Union Bound (Boole's inequality)
        formula
        Multiple Hypothesis Testing: Illustrative Example
            Correct analysis (Bonferroni correction)
#### 8.4 Data Sketching
#### 8.4.1 Bloom Filters
    Data Sketching: compute data statistics with small memory footprint
    answer set membership query
    False positive rate
    Set up the filter b = B(S) = V_{x\inS} B(x)
    Analysis
        formula
        f(k*;n,m)=(1/2)^k* = (0.6185)^{n/m}
        Optimal k: ln2 n/m, ~q = 1/2
    Partitioned Bloom Filter
        formula: q=(1-1/n)^{km} >= (1-k/n)^m = q'
        larger false positive probabilities
#### 8.4.2 Count-Min Sketch
    From Filtering to Counting
        m events
        d (depth) different hash function h_i 
        w (width) possible values in hash function range
    Updating the Count Sketch
        C \in [1:d] * [1:w]
    Analysis
        w = [e / \epsilon] 
        d = [ln 1 / \delta]
        bound formula
        Increasing width results in higher accuracy
        Increasing depth helps to avoid large deviations
    Zipfian Distribution
    Improved Update Rule: Minimum Increase
        if h_i(a) = j and C_{ij} = CM(a)
    Inner Product Queries
        CM(<n,m>) = min_i \sum_j C_ij(n) C_ij(m)
        E[CM(<n,m>)] - <n,m> \le epsilon / e |n| |m|
    Range Queries
        compute(approximately) Q(l,r) := \sum_{i=1}^r n_i
        Trick: use dyadic ranges
            (x,y):[x 2^y + 1: (x+1)2^y]
        Sketch update: O(d log m) operations
        Every range [l:r] can be represented by \le 2 log m dyadic ranges
        Analysis: bound
    Heavy Hitters??
        Find the most frequent elements in a data stream
        there can be between 0 and 1/phi heavy hitters
        n_e >= phi |n|
        n_e >= (phi - epsilon) |n|
        Space needed: O(???)
#### 8.4.3 Count-Mean Sketch
    Analysis 
        bias(C_ij) = \frac{n-C_ij}{w-1}
#### 8.4.4 Counting Distinct Elements
    Cardinality Estimation via Linear Counting
        number of distinct elements
    ^m = - k ln \frac{k-w}{k}
    Cardinality Estimation from Trailing Zeros
        Analysis:
    LogLog Counting
        leading zeros
    HyperLogLog Counting
        Space Needs
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
    8. sketching 草图；写生；写生画, dyadic 二价的；双值的；二数的

