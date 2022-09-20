hey Load generator - initial performance
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
