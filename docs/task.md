# TASKS

* story 1: quick preparation for performance test

    * prepare performance test environment(vm, packages)

    * start fabric topology across hosts with simple scripts and configuration

    * scripts for fabric topology deployment

* story 2: implement HFP framework

    * implement the service framework(hfp-agent, hfp-server, hfp-client)

    * configuration definition(for both fabric network and test parameters)

    * hfp-agent: collect fabric logs

    * hfp-agent: parse/analysis log for warning or erro, then send to hfp-server

    * hfp-agent: collect fabric invoke events and send to hfp-server

    * hfp-agent: parse block event and send core info to hfp-server for analysis

    * hfp-agent: collect agent server's load info, send event to hfp-server if
    server's load has reached the threshold that has been set

    * hfp-server: should server as the event server

    * hfp-server: receive event from hfp-agent check for emerging fabric error

    * hfp-server: save txn info to db for tps calculation

    * hfp-server: generate the performance test report

    * hfp-client: parse performance test parameters

    * hfp-client: query/invoke to fabirc

    * hfp-client: implement the static performance policy

    * hfp-client: implement the dynamic performance policy

* story 3: tuning fabric performance with help with HFP

    * profiling fabric and analysis logs to find the bottleneck

    * check performance with different topology with performance report

    * tuning configuration parameters to check result

    * log all factors that may affect fabric performance

    * raise some solutions for fabric performance enhancement

