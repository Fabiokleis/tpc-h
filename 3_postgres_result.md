```md
| QUERY PLAN                                                                                                                                                           |
| -------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Limit  (cost=147961.37..147961.40 rows=10 width=44) (actual time=1143.933..1144.601 rows=10 loops=1)                                                                 |
|   ->  Sort  (cost=147961.37..148738.37 rows=310799 width=44) (actual time=1143.929..1144.566 rows=10 loops=1)                                                        |
|         Sort Key: (sum((lineitem.l_extendedprice * ('1'::numeric - lineitem.l_discount)))) DESC, orders.o_orderdate                                                  |
|         Sort Method: top-N heapsort  Memory: 26kB                                                                                                                    |
|         ->  GroupAggregate  (cost=95928.62..141245.12 rows=310799 width=44) (actual time=984.673..1126.110 rows=11620 loops=1)                                       |
|               Group Key: lineitem.l_orderkey, orders.o_orderdate, orders.o_shippriority                                                                              |
|               ->  Gather Merge  (cost=95928.62..132698.14 rows=310799 width=24) (actual time=984.653..1054.184 rows=30519 loops=1)                                   |
|                     Workers Planned: 3                                                                                                                               |
|                     Workers Launched: 3                                                                                                                              |
|                     ->  Sort  (cost=94928.58..95179.22 rows=100258 width=24) (actual time=979.911..991.850 rows=7630 loops=4)                                        |
|                           Sort Key: lineitem.l_orderkey, orders.o_orderdate, orders.o_shippriority                                                                   |
|                           Sort Method: quicksort  Memory: 537kB                                                                                                      |
|                           Worker 0:  Sort Method: quicksort  Memory: 533kB                                                                                           |
|                           Worker 1:  Sort Method: quicksort  Memory: 532kB                                                                                           |
|                           Worker 2:  Sort Method: quicksort  Memory: 540kB                                                                                           |
|                           ->  Nested Loop  (cost=4538.19..85395.07 rows=100258 width=24) (actual time=27.070..967.089 rows=7630 loops=4)                             |
|                                 ->  Parallel Hash Join  (cost=4537.76..37327.07 rows=46164 width=12) (actual time=26.947..674.140 rows=36782 loops=4)                |
|                                       Hash Cond: (orders.o_custkey = customer.c_custkey)                                                                             |
|                                       ->  Parallel Seq Scan on orders  (cost=0.00..32184.39 rows=230437 width=16) (actual time=0.006..299.603 rows=181826 loops=4)   |
|                                             Filter: (o_orderdate < '1995-03-15'::date)                                                                               |
|                                             Rows Removed by Filter: 193174                                                                                           |
|                                       ->  Parallel Hash  (cost=4381.25..4381.25 rows=12521 width=4) (actual time=26.741..26.747 rows=7536 loops=4)                   |
|                                             Buckets: 32768  Batches: 1  Memory Usage: 1472kB                                                                         |
|                                             ->  Parallel Seq Scan on customer  (cost=0.00..4381.25 rows=12521 width=4) (actual time=0.014..14.761 rows=7536 loops=4) |
|                                                   Filter: (c_mktsegment = 'BUILDING'::bpchar)                                                                        |
|                                                   Rows Removed by Filter: 29964                                                                                      |
|                                 ->  Index Scan using lineitem_pkey on lineitem  (cost=0.43..0.95 rows=9 width=16) (actual time=0.004..0.004 rows=0 loops=147126)     |
|                                       Index Cond: (l_orderkey = orders.o_orderkey)                                                                                   |
|                                       Filter: (l_shipdate > '1995-03-15'::date)                                                                                      |
|                                       Rows Removed by Filter: 4                                                                                                      |
| Planning Time: 0.403 ms                                                                                                                                              |
| Execution Time: 1144.707 ms                                                                                                                                          |
```
