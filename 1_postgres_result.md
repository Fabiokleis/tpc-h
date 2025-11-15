```md
| QUERY PLAN                                                                                                                                             |
| ------------------------------------------------------------------------------------------------------------------------------------------------------ |
| Finalize GroupAggregate  (cost=170604.19..170608.95 rows=6 width=236) (actual time=3488.934..3498.467 rows=4 loops=1)                                  |
|   Group Key: l_returnflag, l_linestatus                                                                                                                |
|   ->  Gather Merge  (cost=170604.19..170607.81 rows=30 width=236) (actual time=3488.885..3498.380 rows=24 loops=1)                                     |
|         Workers Planned: 5                                                                                                                             |
|         Workers Launched: 5                                                                                                                            |
|         ->  Sort  (cost=169604.11..169604.12 rows=6 width=236) (actual time=3481.141..3481.153 rows=4 loops=6)                                         |
|               Sort Key: l_returnflag, l_linestatus                                                                                                     |
|               Sort Method: quicksort  Memory: 26kB                                                                                                     |
|               Worker 0:  Sort Method: quicksort  Memory: 26kB                                                                                          |
|               Worker 1:  Sort Method: quicksort  Memory: 26kB                                                                                          |
|               Worker 2:  Sort Method: quicksort  Memory: 26kB                                                                                          |
|               Worker 3:  Sort Method: quicksort  Memory: 26kB                                                                                          |
|               Worker 4:  Sort Method: quicksort  Memory: 26kB                                                                                          |
|               ->  Partial HashAggregate  (cost=169603.90..169604.03 rows=6 width=236) (actual time=3481.091..3481.109 rows=4 loops=6)                  |
|                     Group Key: l_returnflag, l_linestatus                                                                                              |
|                     Batches: 1  Memory Usage: 24kB                                                                                                     |
|                     Worker 0:  Batches: 1  Memory Usage: 24kB                                                                                          |
|                     Worker 1:  Batches: 1  Memory Usage: 24kB                                                                                          |
|                     Worker 2:  Batches: 1  Memory Usage: 24kB                                                                                          |
|                     Worker 3:  Batches: 1  Memory Usage: 24kB                                                                                          |
|                     Worker 4:  Batches: 1  Memory Usage: 24kB                                                                                          |
|                     ->  Parallel Seq Scan on lineitem  (cost=0.00..127602.11 rows=1200051 width=25) (actual time=0.019..1640.720 rows=1000200 loops=6) |
|                           Filter: (l_shipdate <= '1998-11-30 00:00:00'::timestamp without time zone)                                                   |
|                           Rows Removed by Filter: 3                                                                                                    |
| Planning Time: 1.830 ms                                                                                                                                |
| Execution Time: 3498.590 ms                                                                                                                            |
```
