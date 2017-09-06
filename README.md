# fabric-perf
Hyperledger Performance

# Design Of Fabric-Perf

This one is the simplest and most practical way to start a fabric performance
test base on our existing works.

following questions should be answered before performance test:

### before we start

* which kind of topology we want to test?

    + orderType kafka
    + multiple orderers and peers deployed on different host
    + there should be multiple SDK client for parallel test
    + each SDK client should connect to multiple peer and orderer to balance request

* which configuration you think may affect performance?

    Here we only consider for the invoke action:

    + block batch time and size
    + service log level
    + the chaincode logic(only consider the simple benchmark chaincode)
    + the payload size for the invoke action
    + the number of concurrent SDK client
    + how many routine each SDK client starts for the test
    + how each SDK client routine continues to invoke
    + try testing with multiple channel

* what is the output of the performance test?

    + server load
    + TPS for different configurations

* what kind of standard the output result is based on?

    + the server load should not be the bottleneck
    + the system limit should not be the bottleneck
    + there should be no error in fabric log
    + all fabric peer node should sync ledger correctly
    + all SDK client invoke should be successfully

### how to do it

* prepare docker environment for all hosts
* create MSP materials to start fabric network

  with 3 kafka/zookeeper, 4 peer, 3 orderer.

* setup configuration for the network

    * the topology name, we support some topology
    * ip and roles for each node
    * channel name, or multiple channel names
    * chaincodes to be installed
    * the test action type, invoke or query
    * number of SDK client
    * the type of the performance test, static or dynamic

        static will use the number of routine to test whether fabric network can
        sustain the request load, the operator can check the service status(log)
        to see whether your topology and configuration can sustain this test.

        dynamic will start from user's configuration(number of routine) and try
        to increase the routing number until service status is not healthy.

    * number of routine for each SDK client

        depending on the type of performance test.

    * and how each routing continues to run the performance test

        one after another or sleep for some seconds.

* start fabric network with a single command

    This helps up to easily setup a test environment.

* how to check service status?

    * the log should not container any error messages
    * all transaction should be valid
    * all peer node should have the same block height
    * all peer node should be running

* how to gather logs ?

    * by logstash or fluentd
    * or by using hpcloud/tail to gather peer node status by the peer agent

### LLD of HFP

* configuration

    * we should have some default topologies

* tools(golang/shell)

    we suppose the hosts and docker swarm has been deployed.

    * read from configurations
    * init MSP material
    * deploy fabric network and perf-agents

* agent

    * gather peer and orderer logs
    * gather fabric event
    * send error messages to queue server
    * send invalid transaction messages to queue server

* server

    * start a queue server
    * collects error messages(txn or service error)
    * collects transaction info from rabbitmq and save to db
    * calculates TPS with sql

### dependency

| components | version | commits |
|-|-|-|
| fabric-sdk-go | v1.0.1  | 08be791008bfd5d4e8298abb74f8948d7900dda3 |
|fabric | v1.0.1 |  e43b68fa22123cd0e80788fd46977b7534a51688|

