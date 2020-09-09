---
name: Beekeeper
description: An early stage data focused startup (2015-2017)
layout: default
---

![beekeeper logo](/assets/beekeeper/beekeeper.png)

A local big data startup founded by the keeper of bees himself, [Matthew Rathbone](https://www.rathbonelabs.com/) (Lately Matthew has reformulated beekeeper as a [popular sql editor](https://www.beekeeperstudio.io/)).

At Beekeeper Data I was lead engineer with a back end focus. We built plenty of database connectors - popular RDBMS like mysql/postgres, but also hive, mongo, h2, and bigquery.  During my time at Beekeeper we shifted our front end UI to Ember, which wasn't half bad compared to spaghetti jquery.

One fun project at Beekeeper was an automated data pipeline we built for a customer to back up their mongo db nightly to AWS EMR.  I wrote scala code that would inspect the mongo tuples from the collections we scanned and could craft the appropriate DML to write structured data to EMR.  Nested columns were very interesting!

Our main product focus for quite a while was automatic emailed reports, and while we email is a very difficult medium to really create anything presentable, our reports looked decent.  

![market station](/assets/beekeeper/market-station.png)
