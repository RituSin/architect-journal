# The story of one website growing up

The map of system design. A system grows by hitting a bottleneck, fixing it, and discovering that the fix creates a new problem. System design is knowing which bottleneck you have now and which fix costs least.

Example app: warehouse staff scan barcodes to record stock.

## Stage 1: One machine does everything

Client, server and database on one machine. Fine for a few hundred users. Breaks when the machine dies, and code and database compete for CPU and memory.

## Stage 2: Database on its own machine

Each can be sized, backed up and fixed separately. Cost: every database call now crosses a network.

## Stage 3: Many app servers and a load balancer

Scale up (a bigger machine) has a ceiling. Scale out (more machines) has none. A load balancer sends each request to a healthy server. Servers must be stateless: no server remembers you between requests, so login lives in a token or a shared store. Cost: the load balancer itself becomes a single point of failure, so run two.

## Stage 4: Cache

A fast memory (for example Redis) that stores recent answers so the database is skipped. Cost: data can be stale. Give each entry a TTL.

## Stage 5: Database replicas

One primary takes writes, replicas serve reads. Cost: replication lag, a replica can be slightly behind. First taste of consistency versus speed.

## Stage 6: CDN

Servers around the world keep copies of static files and serve them from the nearest one. Cost: old copies linger until they expire.

## Stage 7: Queue and workers

Slow jobs go into a queue and workers do them in the background, so the user gets a fast reply. Cost: results are delayed and tasks must be safe to repeat (idempotent).

## Stage 8: Sharding

Split the data across several primaries. Cost: complexity, cross-shard queries, hard to undo. The last step, used only when everything cheaper is exhausted.

## The whole story in one line each

Single point of failure: separate the database and add servers. Overloaded servers: scale out with a load balancer. Repeated reads: cache. Slow database reads: replicas. Slow static files: CDN. Slow jobs: queue and workers. Database too big or too busy: sharding.
