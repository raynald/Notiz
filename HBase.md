# HBase
### 1. General HBase shell commands 
#### 1.1 status
 - status
 - status 'simple'
 - status 'summary'
 - status 'detailed'

#### 1.2 version
 - version

#### 1.3 whoami
 - whoami

### 2. Tables Management commands
#### 2.1 alter
 - To change or add the 'f1' column family in 't1' from current value to keep a maximum of 5 cell VERSIONS
   - alter 't1', NAME => 'f1', VERSIONS => 5
   - alter 't1', 'f1', {NAME => 'f2', IN_MEMORY => true}, {NAME => 'f3', VERSIONS => 5}
 - To delete 'f1' column family in table 't1'
   - alter 't1', DELETE => 'f1' 
 - Change the max size of a region to 128MB
   - alter 't1', MAX_FILESIZE => '134217728' 
 - Add a table coprocess by setting a table coprocessor attribute:
   - [coprocessor jar file location] | class name | [priority] | [arguments]
   - alter 't1', 'coprocessor' => 'hdfs://foo.jar|com.foo.FooRegionObserver|1001|arg1=1,ar' 
 - Set configuration settings specific to this table or column family
   - alter 't1', CONFIGURATION => {'hbase.hregion.scan.loadColumnFamiliesOnDemand' => 'true'}
   - alter 't1', {NAME => 'f2', CONFIGURATION => {'hbase.hstore.blockingStoreFiles' => '10'}}
 - Remove a table-scope attribute
   - alter 't1', METHOD => 'table_att_unset', NAME => 'MAX_FILESIZE'
   - alter 't1', METHOD => 'table_att_unset', NAME => 'coprocessor$1'

#### 2.2 create
 - Create table
   - create 't1', {NAME => 'f1', VERSIONS => 5}
   - create 't1', {NAME => 'f1}, {NAME => 'f2'}, {NAME => 'f3'}

#### 2.3 describe
 - Describe the named table
   - describe 't1'

