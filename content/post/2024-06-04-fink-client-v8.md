---
layout: post
date: "2024-06-04"
title: Fink client v8
author: fink
tags: [client]
---

Dear users, the Fink client v8 is out. [fink-client](https://github.com/astrolabsoftware/fink-client) is a light package to manipulate catalogs and alerts issued from the fink broker programmatically. It is used in the context of 2 major Fink services: Livestream and Data Transfer.

In this new major version, the installation procedure has been simplified, and new functionalities to the Livestream service have been added. More information in the [Livestream manual](https://fink-broker.readthedocs.io/en/latest/services/livestream/). Below, we give some details.

### Simpler installation procedure

The previous installation procedure was cumbersome because we had to freeze an old version of `fastavro`. Thanks to [one our colleague from AMPEL](https://github.com/fastavro/fastavro/pull/738) we can move on versions, and the installation of the client simply becomes:

```
# get or upgrade to the latest version
pip install -U fink-client
```

### Checking offsets

You might want to check where you are on the different queues, that is retrieving the offsets for each topic that you are polling:

```bash
fink_consumer --display_statistics

Topic [Partition]                                   Committed        Lag
========================================================================
fink_sso_ztf_candidates_ztf  [4]                            1        972
------------------------------------------------------------------------
Total for fink_sso_ztf_candidates_ztf                       1        972
------------------------------------------------------------------------

Topic [Partition]                                   Committed        Lag
========================================================================
------------------------------------------------------------------------
Total for fink_sso_fink_candidates_ztf                      0          2
------------------------------------------------------------------------
```

In this example, I have two topic, `fink_sso_ztf_candidates_ztf` and `fink_sso_fink_candidates_ztf`. 

For the first topic, there is one active partition on the remote Kafka cluster that served data (number `[4]`). I polled 1 alert (`Committed`), and there are `972` remaining alerts to be polled (`Lag`). As there is only one active partition on the remote Kafka cluster, the total is the same (there could be up to 10 active partitions). For the second topic, I did not start polling as `0` alert has been `Committed`.

### Resetting offsets

Sometimes you might want to poll again alerts, that is restarting to poll from the beginning of a queue. For this, you can use:

```bash
fink_consumer --display -start_at earliest
Resetting offsets to BEGINNING
...
assign TopicPartition{topic=fink_sso_fink_candidates_ztf,partition=0,offset=0,leader_epoch=None,error=None}
...
assign TopicPartition{topic=fink_sso_ztf_candidates_ztf,partition=0,offset=0,leader_epoch=None,error=None}
...
# poll restarts at the first offset
```

All your topic partitions will be reset to the starting offset (`0` in this case). Similarly, you can empty all topics, and restarting polling from the last offset:

```bash
fink_consumer --display -start_at latest
...
assign TopicPartition{topic=fink_sso_fink_candidates_ztf,partition=0,offset=0,leader_epoch=None,error=None}
...
assign TopicPartition{topic=fink_sso_fink_candidates_ztf,partition=4,offset=2,leader_epoch=None,error=None}
...
assign TopicPartition{topic=fink_sso_ztf_candidates_ztf,partition=4,offset=973,leader_epoch=None,error=None}
...
No alerts the last 10 seconds
...
```

Empty partitions will have `offset=0`, but others will have their offset to the latest one. The client will then wait for new data to come. Note that the reset will be actually triggered on the next poll. Hence the command `fink_consumer --display_statistics` will not right away display the reset offsets.
This is particularly useful after a bug in the topic (malformed alerts pushed), and you want a fresh restart.

### Troubleshooting schema mismatch

A typical error though would be:

```
Traceback (most recent call last):
  File "/Users/julien/anaconda3/bin/fink_consumer", line 10, in <module>
    sys.exit(main())
  File "/Users/julien/Documents/workspace/myrepos/fink-client/fink_client/scripts/fink_consumer.py", line 92, in main
    topic, alert = consumer.poll(timeout=maxtimeout)
  File "/Users/julien/Documents/workspace/myrepos/fink-client/fink_client/consumer.py", line 94, in poll
    alert = _decode_avro_alert(avro_alert, self._parsed_schema)
  File "/Users/julien/Documents/workspace/myrepos/fink-client/fink_client/avroUtils.py", line 381, in _decode_avro_alert
    return fastavro.schemaless_reader(avro_alert, schema)
  File "fastavro/_read.pyx", line 835, in fastavro._read.schemaless_reader
  File "fastavro/_read.pyx", line 846, in fastavro._read.schemaless_reader
  File "fastavro/_read.pyx", line 561, in fastavro._read._read_data
  File "fastavro/_read.pyx", line 456, in fastavro._read.read_record
  File "fastavro/_read.pyx", line 559, in fastavro._read._read_data
  File "fastavro/_read.pyx", line 431, in fastavro._read.read_union
  File "fastavro/_read.pyx", line 555, in fastavro._read._read_data
  File "fastavro/_read.pyx", line 349, in fastavro._read.read_array
  File "fastavro/_read.pyx", line 561, in fastavro._read._read_data
  File "fastavro/_read.pyx", line 456, in fastavro._read.read_record
  File "fastavro/_read.pyx", line 559, in fastavro._read._read_data
  File "fastavro/_read.pyx", line 405, in fastavro._read.read_union
IndexError: list index out of range
```

This error happens when the schema to decode the alert is not matching the alert content. Usually this should not happen (schema is included in the alert payload). In case it happens though, you can force a schema:

```
fink_consumer [...] -schema [path_to_a_good_schema]
```

In case you do not have replacement schemas, you can save the current (faulty) schema that is contained within an alert packet:

```bash
fink_consumer -limit 1 --dump_schema
```

You will see the traceback above, with the message:

```
Schema saved as schema_2024-06-03T11:12:36.855544+00:00.json
```


Then you can inspect the schema manually, or open an issue on the fink-client repository by attaching this schema to your message.

### What would be next?

We are preparing for the start of LSST. Many functionalities are being developed (automation of workflows, web client, centralised authentication with other Fink services, ...), but let us know what you would like to have! 

