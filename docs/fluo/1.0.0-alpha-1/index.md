---
layout: fluo-doc
title: Fluo 1.0.0-alpha-1 Documentation
redirect_from: /docs/1.0.0-alpha-1/
version: 1.0.0-alpha-1
redirect_to: "{{ site.apache }}{{ page.url }}"
---

Fluo is a [Percolator][2] prototype for [Accumulo][1].  This prototype relies on 
Accumulo 1.6.0 which has [ACCUMULO-1000][3] and [ACCUMULO-112][5].
[ACCUMULO-1000][3] makes cross row transactions possible and  [ACCUMULO-112][5]
makes it possible to efficiently find notifications.  Theoretically, this
prototype is to a point where it could run in a distributed manner.  But this
has not been tested.  The pieces are in place, CAS is done on the tablet server
and the Oracle is a service.  

There is a lot that needs to be done.  If you are interested in contributing
send me an email or check out the issues.

If you want to experiment with Fluo, check out the [phrasecount][7] example.
This example is a simple end-to-end application that is super easy to run.  It
also has handling for real world problems like high cardinality phrases.

Building Fluo
-----------------

Using Maven, you can build Fluo with the following steps.

    git clone https://github.com/fluo-io/fluo.git
    cd fluo
    mvn package

Running `mvn package` will build a tar ball that contains all of the
dependencies needed at runtime.  Currently the tar ball is targeted towards
Hadoop 2.3.0 and Accumulo 1.6.0.  If you have different versions installed on
your cluster, you can try passing the following options to maven package.
Other versions of Hadoop and Accumulo may not work, please open a bug if you
run into problems.

    mvn package -Daccumulo.version=1.6.1-SNAPSHOT -Dhadoop.version=2.4.0

Once the tarball is built, deploy it using the command below which can be modified
to a directory of your choice (rather than `/opt`).

    tar -C /opt -xvzf modules/distribution/target/fluo-1.0.0-alpha-1-SNAPSHOT-bin.tar.gz

Configuring Fluo
--------------------

To configure and run Fluo, you will need a running Accumulo instance as well 
as an observer jar to run with Fluo.  If you have not created your own observer
jar, you can build an observer jar using the [phrasecount][7] example.

First, copy the example configuration files and modify them for you environment.

    cd fluo-1.0.0-alpha-1-SNAPSHOT/conf
    cp examples/* .
    vim fluo-env.sh
    vim fluo.properties

Copy your observer jar to Fluo and set up notifications to your observer in
 `fluo.properties`.  Check out [phrasecount][7] to build an example observer
 jar and find instructions for configuring `fluo.properties`.

    OBSERVER_JAR=<location of observer jar>
    cd fluo-1.0.0-alpha-1-SNAPSHOT/
    cp $OBSERVER_JAR lib/observers
    vim conf/fluo.properties

Finally, initialize your instance which only needs to be called once and stores
 configuration in zookeeper.

    ./bin/initialize.sh

Running Fluo
----------------

A Fluo instance consists of 1 oracle process and multiple worker processes.
These processes can either be run on a YARN cluster or started locally on each
machine.

The preferred method to run Fluo applications is using YARN which will start
up multiple workers as configured in `fluo.properties`.  To start a Fluo cluster 
in YARN, run following commands:

    ./bin/oracle.sh start-yarn
    ./bin/worker.sh start-yarn

The `start-yarn` commands above submit your Fluo applications to YARN.  Therefore, 
they work for a single-node or a large cluster.  By using YARN, you no longer need 
to deploy the Fluo binaries to every node on your cluster or start processes on 
every node.

You can use `yarn application -list` to check the status of the applications. 
Logs are viewable within YARN.  When finished, you can kill the applications
using `yarn application -kill <Application ID>`.  The application ID can be
found using the list command.

If you do not have YARN set up, you can start a local Fluo process using
the following commands:

    ./bin/oracle.sh start-local
    ./bin/worker.sh start-local

In a distributed environment, you will need to deploy the Fluo binary to 
every node and start each process individually.

To stop Fluo processes, run the following commands:

    ./bin/worker.sh stop-local
    ./bin/oracle.sh stop-local

Tuning Accumulo
---------------

Fluo will reread the same data frequently when it checks conditions on
mutations.   When Fluo initializes a table it enables data caching to make
this more efficient.  However you may need to increase the amount of memory
available for caching in the tserver by increasing `tserver.cache.data.size`.
Increasing this may require increasing the maximum tserver java heap size in
`accumulo-env.sh`.  

Fluo will run many client threads, will want to ensure the tablet server
has enough threads.  Should probably increase the
`tserver.server.threads.minimum` Accumulo setting.

Using at least Accumulo 1.6.1 is recommended because multiple performance bugs
were fixed.

Additional Documentation
------------------------

[Stress Testing](stress/)

[1]: http://accumulo.apache.org
[2]: http://research.google.com/pubs/pub36726.html
[3]: https://issues.apache.org/jira/browse/ACCUMULO-1000
[5]: https://issues.apache.org/jira/browse/ACCUMULO-112
[7]: https://github.com/fluo-io/phrasecount
