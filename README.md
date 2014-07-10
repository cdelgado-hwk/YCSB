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

4. Run YCSB command.

    ```sh
    bin/ycsb load basic -P workloads/workloada
    bin/ycsb run basic -P workloads/workloada
    ```

  Running the `ycsb` command without any argument will print the usage.

  See https://github.com/brianfrankcooper/YCSB/wiki/Running-a-Workload
  for a detailed documentation on how to run a workload.

  See https://github.com/brianfrankcooper/YCSB/wiki/Core-Properties for
  the list of available workload properties.
