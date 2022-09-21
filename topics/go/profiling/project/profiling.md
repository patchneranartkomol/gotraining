## Load generator histograms
`hey` Load generator - initial performance
```
hey -m POST -c 100 -n 10000 "http://localhost:5000/search?term=trump&cnn=on&bbc=on&nyt=on"

Summary:
  Total:        4.2451 secs
  Slowest:      0.5359 secs
  Fastest:      0.0008 secs
  Average:      0.0391 secs
  Requests/sec: 2355.6388


Response time histogram:
  0.001 [1]     |
  0.054 [7117]  |■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■
  0.108 [1779]  |■■■■■■■■■■
  0.161 [602]   |■■■
  0.215 [330]   |■■
  0.268 [115]   |■
  0.322 [35]    |
  0.375 [16]    |
  0.429 [3]     |
  0.482 [1]     |
  0.536 [1]     |


Latency distribution:
  10% in 0.0018 secs
  25% in 0.0037 secs
  50% in 0.0087 secs
  75% in 0.0661 secs
  90% in 0.1154 secs
  95% in 0.1615 secs
  99% in 0.2418 secs

Details (average, fastest, slowest):
  DNS+dialup:   0.0001 secs, 0.0008 secs, 0.5359 secs
  DNS-lookup:   0.0001 secs, 0.0000 secs, 0.0263 secs
  req write:    0.0000 secs, 0.0000 secs, 0.0260 secs
  resp wait:    0.0388 secs, 0.0007 secs, 0.5358 secs
  resp read:    0.0001 secs, 0.0000 secs, 0.0135 secs

Status code distribution:
  [200] 10000 responses
```

`hey` Output when running with `GODEBUG=gctrace=1`
```
hey -m POST -c 100 -n 10000 "http://localhost:5000/search?term=trump&cnn=on&bbc=on&nyt=on"

Summary:
  Total:        19.0084 secs
  Slowest:      15.4844 secs
  Fastest:      0.0006 secs
  Average:      0.1868 secs
  Requests/sec: 526.0822


Response time histogram:
  0.001 [1]     |
  1.549 [9899]  |■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■
  3.097 [0]     |
  4.646 [0]     |
  6.194 [0]     |
  7.743 [0]     |
  9.291 [0]     |
  10.839 [0]    |
  12.388 [0]    |
  13.936 [0]    |
  15.484 [100]  |


Latency distribution:
  10% in 0.0016 secs
  25% in 0.0033 secs
  50% in 0.0079 secs
  75% in 0.0607 secs
  90% in 0.1072 secs
  95% in 0.1473 secs
  99% in 15.2585 secs

Details (average, fastest, slowest):
  DNS+dialup:   0.0001 secs, 0.0006 secs, 15.4844 secs
  DNS-lookup:   0.0001 secs, 0.0000 secs, 0.0198 secs
  req write:    0.0000 secs, 0.0000 secs, 0.0150 secs
  resp wait:    0.1866 secs, 0.0005 secs, 15.4624 secs
  resp read:    0.0001 secs, 0.0000 secs, 0.0124 secs

Status code distribution:
  [200] 10000 responses

```

## GC Trace
Trace output

```
GODEBUG=gctrace=1 ./project
2022/09/20 16:08:28.700137 service.go:64: Listening on: 0.0.0.0:5000
gc 1 @23.437s 0%: 0.041+4.4+0.023 ms clock, 0.16+0.29/4.3/7.8+0.094 ms cpu, 3->4->2 MB, 4 MB goal, 2 MB stacks, 0 MB globals, 4 P
2022/09/20 16:08:52.140745 rss.go:109: reloaded cache http://feeds.bbci.co.uk/news/rss.xml
gc 2 @23.451s 0%: 0.021+1.6+0.015 ms clock, 0.085+0.12/1.5/2.9+0.060 ms cpu, 5->5->2 MB, 5 MB goal, 2 MB stacks, 0 MB globals, 4 P
2022/09/20 16:08:52.154074 rss.go:109: reloaded cache http://feeds.bbci.co.uk/news/world/rss.xml
...
gc 592 @37.375s 1%: 0.081+1.5+0.049 ms clock, 0.32+0.73/1.5/0+0.19 ms cpu, 4->5->2 MB, 5 MB goal, 0 MB stacks, 0 MB globals, 4 P
GC forced
```

## Go pprof tool

Visit [pprof endpoint](http://localhost:5000/debug/pprof/) and fetch the relevant binary.

Run with:

```
go tool pprof <binary>
```
Or:
```
go tool pprof http://localhost:5000/debug/pprof/allocs
```

Using the binary from [/debug/pprof/allocs](http://localhost:5000/debug/pprof/allocs?debug=1) for example.

Print the top-most expensive allocations:
```
(pprof) top
Showing nodes accounting for 1348.19MB, 92.44% of 1458.41MB total
Dropped 127 nodes (cum <= 7.29MB)
Showing top 10 nodes out of 56
      flat  flat%   sum%        cum   cum%
  578.59MB 39.67% 39.67%   578.59MB 39.67%  strings.(*Builder).grow (inline)
  369.95MB 25.37% 65.04%   369.95MB 25.37%  bytes.growSlice
  168.88MB 11.58% 76.62%   690.42MB 47.34%  github.com/ardanlabs/gotraining/topics/go/profiling/project/service.render
   83.75MB  5.74% 82.36%   801.18MB 54.94%  github.com/ardanlabs/gotraining/topics/go/profiling/project/service.handler
   51.50MB  3.53% 85.89%       97MB  6.65%  reflect.Value.call
      29MB  1.99% 87.88%   613.59MB 42.07%  github.com/ardanlabs/gotraining/topics/go/profiling/project/search.rssSearch
      22MB  1.51% 89.39%       22MB  1.51%  reflect.FuncOf
   18.51MB  1.27% 90.66%    18.51MB  1.27%  github.com/ardanlabs/gotraining/topics/go/profiling/project/search.Submit
      16MB  1.10% 91.76%      113MB  7.75%  text/template.(*state).evalCall
      10MB  0.69% 92.44%    19.50MB  1.34%  reflect.MakeSlice
```
Show a visualization weighted by the most expensive nodes.
```
(pprof) web
(pprof) svg
```

![allocations profile svg](./profile001.svg)

## After improvements to memory usage

![allocations profile improved svg](./profile002.svg)
