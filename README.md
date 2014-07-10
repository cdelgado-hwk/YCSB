Yahoo! Cloud System Benchmark (YCSB)
====================================

Links
-----
http://wiki.github.com/brianfrankcooper/YCSB/

http://research.yahoo.com/Web_Information_Management/YCSB/

ycsb-users@yahoogroups.com

Getting Started With YCSB for HBase
-----------------------------------

1. Download the release of YCSB that matches your install.  There are branches for HDP 2.0 and HDP 2.1.

    [HDP 2.0](../../archive/hdp2.0.zip)
    ```sh
    unzip YCSB-hdp2.0.zip
    cd YCSB-hdp2.0
    ```
    
    [HDP 2.1](../../archive/hdp2.1.zip)
    ```sh
    unzip YCSB-hdp2.1.zip
    cd YCSB-hdp2.1
    ```
2. Build YCSB using Maven
    ```sh
    mvn clean package -e -DskipTests
    ```

3. Link all HBase configuration files to the conf directory
    ```sh
    ln -Fs /etc/hbase/conf/hbase-site.xml hbase/src/main/conf/hbase-site.xml 
    ```

4. Create the table that is pre-split from the hbase shell.  In this example the table is pre-replit to 20 regions.
   ```sh
    hbase shell
    hbase(main):001:0> create 'usertable', 'family', {SPLITS => (1..20).map {|i| "user#{1000+i*(9999-1000)/200}"}, MAX_FILESIZE => 4*1024**3}
   ```

5. Run YCSB following commands to load data into the table and run the benchmark.

    ```sh
    ./bin/ycsb load hbase -p columnfamily=family -P workloads/workloada 
    ./bin/ycsb run hbase -p columnfamily=family -P workloads/workloada
    ```

6. To scale up the test for larger clusters the table can be pre-split wider and the benchmark should be run with more records and thread.  For example run with a table split to 300 regions, with 100,000,000 rows, with 1,000,000 operations running on 128 thread.

 Create table:
    ```sh
    create 'usertable', 'family', {SPLITS => (1..300).map {|i| "user#{1000+i*(9999-1000)/200}"}, MAX_FILESIZE => 4*1024**3}
    ```
    Run test:
    ```sh
    ./bin/ycsb load hbase -p columnfamily=family -P workloads/workloada -p recordcount=100000000
    ./bin/ycsb run hbase -p columnfamily=family -P workloads/workloada -p operationcount=1000000 -s -threads 128
    ````

  Running the `ycsb` command without any argument will print the usage.

  See https://github.com/brianfrankcooper/YCSB/wiki/Running-a-Workload
  for a detailed documentation on how to run a workload.

  See https://github.com/brianfrankcooper/YCSB/wiki/Core-Properties for
  the list of available workload properties.
