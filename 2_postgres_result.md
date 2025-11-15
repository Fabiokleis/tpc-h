```md
| QUERY PLAN                                                                                                                                                                 |
| -------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Sort  (cost=31675.11..31675.11 rows=1 width=270) (actual time=2013.478..2014.388 rows=458 loops=1)                                                                         |
|   Sort Key: supplier.s_acctbal DESC, nation.n_name, supplier.s_name, part.p_partkey                                                                                        |
|   Sort Method: quicksort  Memory: 120kB                                                                                                                                    |
|   ->  Merge Join  (cost=24267.87..31675.10 rows=1 width=270) (actual time=1350.767..2012.736 rows=458 loops=1)                                                             |
|         Merge Cond: (part.p_partkey = partsupp.ps_partkey)                                                                                                                 |
|         Join Filter: (partsupp.ps_supplycost = (SubPlan 1))                                                                                                                |
|         Rows Removed by Join Filter: 171                                                                                                                                   |
|         ->  Gather Merge  (cost=1000.44..8080.06 rows=826 width=30) (actual time=278.001..279.436 rows=774 loops=1)                                                        |
|               Workers Planned: 2                                                                                                                                           |
|               Workers Launched: 2                                                                                                                                          |
|               ->  Parallel Index Scan using part_pkey on part  (cost=0.42..6984.70 rows=344 width=30) (actual time=2.870..217.755 rows=258 loops=3)                        |
|                     Filter: (((p_type)::text ~~ '%STEEL'::text) AND (p_size = 15))                                                                                         |
|                     Rows Removed by Filter: 66409                                                                                                                          |
|         ->  Sort  (cost=23267.43..23279.19 rows=4706 width=250) (actual time=1068.168..1308.829 rows=158603 loops=1)                                                       |
|               Sort Key: partsupp.ps_partkey                                                                                                                                |
|               Sort Method: external sort  Disk: 29760kB                                                                                                                    |
|               ->  Gather  (cost=1386.13..22980.35 rows=4706 width=250) (actual time=67.610..784.019 rows=158960 loops=1)                                                   |
|                     Workers Planned: 3                                                                                                                                     |
|                     Workers Launched: 3                                                                                                                                    |
|                     ->  Hash Join  (cost=386.13..21509.75 rows=1518 width=250) (actual time=62.698..856.465 rows=39740 loops=4)                                            |
|                           Hash Cond: (partsupp.ps_suppkey = supplier.s_suppkey)                                                                                            |
|                           ->  Parallel Seq Scan on partsupp  (cost=0.00..20140.65 rows=258065 width=14) (actual time=2.793..443.186 rows=200000 loops=4)                   |
|                           ->  Hash  (cost=385.40..385.40 rows=59 width=244) (actual time=59.719..59.744 rows=1987 loops=4)                                                 |
|                                 Buckets: 2048 (originally 1024)  Batches: 1 (originally 1)  Memory Usage: 409kB                                                            |
|                                 ->  Hash Join  (cost=24.31..385.40 rows=59 width=244) (actual time=5.088..56.509 rows=1987 loops=4)                                        |
|                                       Hash Cond: (supplier.s_nationkey = nation.n_nationkey)                                                                               |
|                                       ->  Seq Scan on supplier  (cost=0.00..323.00 rows=10000 width=144) (actual time=0.828..34.613 rows=10000 loops=4)                    |
|                                       ->  Hash  (cost=24.29..24.29 rows=1 width=108) (actual time=4.211..4.226 rows=5 loops=4)                                             |
|                                             Buckets: 1024  Batches: 1  Memory Usage: 9kB                                                                                   |
|                                             ->  Hash Join  (cost=12.14..24.29 rows=1 width=108) (actual time=4.132..4.210 rows=5 loops=4)                                  |
|                                                   Hash Cond: (nation.n_regionkey = region.r_regionkey)                                                                     |
|                                                   ->  Seq Scan on nation  (cost=0.00..11.70 rows=170 width=112) (actual time=2.390..2.429 rows=25 loops=4)                 |
|                                                   ->  Hash  (cost=12.12..12.12 rows=1 width=4) (actual time=1.696..1.701 rows=1 loops=4)                                   |
|                                                         Buckets: 1024  Batches: 1  Memory Usage: 9kB                                                                       |
|                                                         ->  Seq Scan on region  (cost=0.00..12.12 rows=1 width=4) (actual time=1.680..1.683 rows=1 loops=4)                |
|                                                               Filter: (r_name = 'EUROPE'::bpchar)                                                                          |
|                                                               Rows Removed by Filter: 4                                                                                    |
|         SubPlan 1                                                                                                                                                          |
|           ->  Aggregate  (cost=15.87..15.88 rows=1 width=32) (actual time=0.304..0.305 rows=1 loops=629)                                                                   |
|                 ->  Nested Loop  (cost=1.01..15.87 rows=1 width=6) (actual time=0.241..0.298 rows=2 loops=629)                                                             |
|                       ->  Nested Loop  (cost=0.85..14.80 rows=4 width=10) (actual time=0.214..0.275 rows=4 loops=629)                                                      |
|                             ->  Nested Loop  (cost=0.71..14.15 rows=4 width=10) (actual time=0.207..0.246 rows=4 loops=629)                                                |
|                                   ->  Index Scan using partsupp_pkey on partsupp partsupp_1  (cost=0.42..4.14 rows=4 width=10) (actual time=0.199..0.206 rows=4 loops=629) |
|                                         Index Cond: (ps_partkey = part.p_partkey)                                                                                          |
|                                   ->  Index Scan using supplier_pkey on supplier supplier_1  (cost=0.29..2.50 rows=1 width=8) (actual time=0.005..0.005 rows=1 loops=2516) |
|                                         Index Cond: (s_suppkey = partsupp_1.ps_suppkey)                                                                                    |
|                             ->  Index Scan using nation_pkey on nation nation_1  (cost=0.14..0.16 rows=1 width=8) (actual time=0.002..0.002 rows=1 loops=2516)             |
|                                   Index Cond: (n_nationkey = supplier_1.s_nationkey)                                                                                       |
|                       ->  Memoize  (cost=0.15..0.25 rows=1 width=4) (actual time=0.002..0.002 rows=0 loops=2516)                                                           |
|                             Cache Key: nation_1.n_regionkey                                                                                                                |
|                             Cache Mode: logical                                                                                                                            |
|                             Hits: 2511  Misses: 5  Evictions: 0  Overflows: 0  Memory Usage: 1kB                                                                           |
|                             ->  Index Scan using region_pkey on region region_1  (cost=0.14..0.24 rows=1 width=4) (actual time=0.006..0.006 rows=0 loops=5)                |
|                                   Index Cond: (r_regionkey = nation_1.n_regionkey)                                                                                         |
|                                   Filter: (r_name = 'EUROPE'::bpchar)                                                                                                      |
|                                   Rows Removed by Filter: 1                                                                                                                |
| Planning Time: 15.858 ms                                                                                                                                                   |
| Execution Time: 2018.225 ms                                                                                                                                                |
```
