

## Quick Start

This README describes how to run YCSB on VoltDB. 

Here at VoltDB we use 4 machines for testing - 1 client and a 3 node cluster.

### 1. Install Java on all the machines involved.

By default VoltDB uses [Oracle Java](https://www.oracle.com/technetwork/java/javase/downloads/index.html)

### 2. Install Maven

Maven is a pre-requisite for YCSB. It can be downloaded [here](https://maven.apache.org/download.cgi).
    
### 3. Install and configure VoltDB

If you don't already have a copy of VoltDB you should [download](https://www.voltdb.com/try-voltdb/)  and install it. 
Make sure you know the hostnames/ip addresses of all the nodes in the cluster and that [port 21212](https://docs.voltdb.com/AdminGuide/HostConfigPortOpts.php) is open for your client.

A representative VoltDB cluster would have 3 nodes and a '[K factor](https://docs.voltdb.com/UsingVoltDB/KSafeEnable.php)' of 1. This configuration allows work to continue if a server dies.

Note: If you contact us we can give you access to AWS Cloudformation scripts and a matching AMI to create a cluster for you.

Install the VoltDB binaries on all the machines - we need JAR files from them to compile our client.

### 4. InstallYCSB

Download the [latest YCSB](https://github.com/brianfrankcooper/YCSB/releases/latest) file. Follow the instructions.

   	
### 5. Compile VoltDB's jar files

We need two jar files - `ycsb-procs.jar` which contains the stored procedures we run in VoltDB and `ycsb_client.jar`, which contains our YCSB interface. They can be created by cd'ing the Voltdb subdirectory, setting YCSB_HOME to the location of the YCSB release and then calling:

    ./run.sh jars

### 6. Create the YCSB schema in VoltDB

VoltDB's YCSB schema includes tables and stored procedures, which are in ycsb-procs.jar. To create the database you call the DDL script from the same directory it lives in. In the example below the database is on the same machine. If running against a cluster the name or ip address of any member of the cluster will suffice

    cd YCSB
    cd voltdb
    sqlcmd --servers=localhost < ycsb_ddl.sql

Once this has done you should be able to log into the VoltDB web GUI at 'http://localhost:8080' (or whatever the correct ip address is) and see the tables and procedures we use.


### 7. Configure VoltDB parameters


Edit the file base.properties in the VoltDB subdirectory

- `voltdb.servers`
   	- This should be a comma delimited list of servers that make up the VoltDB cluster.
   
- `voltdb.user`
   	- Username. Only needed if username/passwords enabled.
- `voltdb.password`
   	- Password. Only needed if username/passwords enabled.
- `voltdb.ratelimit`
   	- Maximum number of transactions allowed per second. Note that as you increase the workload you eventually get to a point where throwing more and more transactions at a given configuration is counterproductive. For the three node configuration we mentioned above 70000 would be a good starting point for this value.
   	
   
### 8. Run YCSB

See: [https://github.com/brianfrankcooper/YCSB/wiki/Running-a-Workload](https://github.com/brianfrankcooper/YCSB/wiki/Running-a-Workload)


