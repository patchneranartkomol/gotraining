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

Trace output

```
GODEBUG=gctrace=1 ./project
2022/09/20 16:08:28.700137 service.go:64: Listening on: 0.0.0.0:5000
gc 1 @23.437s 0%: 0.041+4.4+0.023 ms clock, 0.16+0.29/4.3/7.8+0.094 ms cpu, 3->4->2 MB, 4 MB goal, 2 MB stacks, 0 MB globals, 4 P
2022/09/20 16:08:52.140745 rss.go:109: reloaded cache http://feeds.bbci.co.uk/news/rss.xml
gc 2 @23.451s 0%: 0.021+1.6+0.015 ms clock, 0.085+0.12/1.5/2.9+0.060 ms cpu, 5->5->2 MB, 5 MB goal, 2 MB stacks, 0 MB globals, 4 P
2022/09/20 16:08:52.154074 rss.go:109: reloaded cache http://feeds.bbci.co.uk/news/world/rss.xml
gc 3 @23.461s 0%: 0.044+1.5+0.017 ms clock, 0.17+0.084/1.4/2.9+0.069 ms cpu, 5->5->2 MB, 5 MB goal, 2 MB stacks, 0 MB globals, 4 P
gc 4 @23.477s 0%: 0.093+3.7+0.037 ms clock, 0.37+0.31/3.5/9.2+0.15 ms cpu, 5->5->2 MB, 5 MB goal, 2 MB stacks, 0 MB globals, 4 P
2022/09/20 16:08:52.182908 rss.go:109: reloaded cache http://feeds.bbci.co.uk/news/politics/rss.xml
2022/09/20 16:08:52.193171 rss.go:109: reloaded cache http://rss.nytimes.com/services/xml/rss/nyt/HomePage.xml
2022/09/20 16:08:52.201969 rss.go:109: reloaded cache http://feeds.bbci.co.uk/news/world/us_and_canada/rss.xml
gc 5 @23.505s 0%: 0.11+1.8+0.003 ms clock, 0.47+1.5/1.6/0.99+0.013 ms cpu, 5->5->2 MB, 5 MB goal, 2 MB stacks, 0 MB globals, 4 P
2022/09/20 16:08:52.283783 rss.go:109: reloaded cache http://rss.cnn.com/rss/cnn_topstories.rss
gc 6 @23.783s 0%: 0.048+4.3+0.052 ms clock, 0.19+0.17/4.0/8.1+0.21 ms cpu, 5->5->2 MB, 5 MB goal, 1 MB stacks, 0 MB globals, 4 P
2022/09/20 16:08:52.485206 rss.go:109: reloaded cache http://rss.cnn.com/rss/cnn_world.rss
2022/09/20 16:08:52.684039 rss.go:109: reloaded cache http://rss.cnn.com/rss/cnn_us.rss
2022/09/20 16:08:52.891149 rss.go:109: reloaded cache http://rss.cnn.com/rss/cnn_allpolitics.rss
gc 7 @28.538s 0%: 0.053+2.9+0.023 ms clock, 0.21+0.33/2.7/5.4+0.093 ms cpu, 5->5->2 MB, 5 MB goal, 1 MB stacks, 0 MB globals, 4 P
2022/09/20 16:08:57.239223 rss.go:109: reloaded cache http://rss.nytimes.com/services/xml/rss/nyt/US.xml
2022/09/20 16:09:02.282584 rss.go:109: reloaded cache http://rss.nytimes.com/services/xml/rss/nyt/Politics.xml
2022/09/20 16:09:02.327024 rss.go:109: reloaded cache http://rss.nytimes.com/services/xml/rss/nyt/Business.xml
gc 8 @33.631s 0%: 0.23+3.7+0.051 ms clock, 0.92+5.8/3.2/0+0.20 ms cpu, 5->5->2 MB, 5 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 9 @33.639s 0%: 0.074+2.5+0.022 ms clock, 0.29+1.6/2.4/0+0.089 ms cpu, 5->6->3 MB, 6 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 10 @33.646s 0%: 0.10+2.0+0.022 ms clock, 0.42+1.9/1.9/0+0.088 ms cpu, 6->7->3 MB, 7 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 11 @33.652s 0%: 0.067+2.3+0.022 ms clock, 0.26+1.2/2.2/0+0.088 ms cpu, 5->6->3 MB, 6 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 12 @33.659s 0%: 0.10+1.7+0.019 ms clock, 0.43+1.9/1.6/0+0.077 ms cpu, 6->6->2 MB, 7 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 13 @33.664s 0%: 0.070+2.4+0.018 ms clock, 0.28+1.5/2.3/0+0.072 ms cpu, 5->6->3 MB, 6 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 14 @33.670s 0%: 0.12+2.5+0.024 ms clock, 0.50+1.5/2.4/0+0.099 ms cpu, 6->6->3 MB, 6 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 15 @33.677s 0%: 0.068+2.2+0.021 ms clock, 0.27+1.6/2.1/0+0.086 ms cpu, 6->6->3 MB, 7 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 16 @33.683s 0%: 0.11+2.4+0.023 ms clock, 0.44+1.9/2.4/0+0.094 ms cpu, 6->6->3 MB, 7 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 17 @33.689s 0%: 0.085+2.4+0.027 ms clock, 0.34+1.5/2.3/0+0.10 ms cpu, 6->6->3 MB, 7 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 18 @33.696s 0%: 0.090+2.0+0.018 ms clock, 0.36+2.0/2.0/0+0.072 ms cpu, 6->6->3 MB, 7 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 19 @33.702s 0%: 0.085+1.9+0.018 ms clock, 0.34+1.6/1.8/0+0.073 ms cpu, 5->6->2 MB, 6 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 20 @33.707s 0%: 0.089+2.0+0.023 ms clock, 0.35+0.74/1.9/0+0.094 ms cpu, 5->6->2 MB, 6 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 21 @33.713s 0%: 0.084+2.2+0.022 ms clock, 0.33+1.6/2.1/0+0.088 ms cpu, 5->6->3 MB, 6 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 22 @33.719s 0%: 0.083+2.2+0.032 ms clock, 0.33+1.6/1.6/0+0.12 ms cpu, 5->6->2 MB, 6 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 23 @33.725s 0%: 0.099+2.0+0.025 ms clock, 0.39+1.4/1.9/0+0.10 ms cpu, 5->6->2 MB, 6 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 24 @33.731s 0%: 0.10+3.1+0.017 ms clock, 0.40+2.4/2.0/0+0.070 ms cpu, 5->6->3 MB, 6 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 25 @33.738s 0%: 0.079+1.6+0.021 ms clock, 0.31+1.8/1.5/0+0.084 ms cpu, 5->6->2 MB, 6 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 26 @33.743s 0%: 0.14+2.2+0.022 ms clock, 0.58+1.4/2.1/0+0.089 ms cpu, 5->5->3 MB, 6 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 27 @33.749s 0%: 0.071+2.1+0.023 ms clock, 0.28+1.5/2.1/0+0.095 ms cpu, 5->6->3 MB, 6 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 28 @33.755s 0%: 0.12+2.4+0.020 ms clock, 0.48+1.4/2.4/0+0.083 ms cpu, 5->6->3 MB, 6 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 29 @33.761s 0%: 0.083+2.2+0.044 ms clock, 0.33+1.4/2.1/0+0.17 ms cpu, 5->6->3 MB, 7 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 30 @33.766s 0%: 0.076+3.2+0.004 ms clock, 0.30+1.3/2.5/0+0.019 ms cpu, 5->6->3 MB, 6 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 31 @33.774s 0%: 0.073+2.5+0.026 ms clock, 0.29+1.0/2.4/0+0.10 ms cpu, 6->7->3 MB, 7 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 32 @33.781s 0%: 0.072+1.9+0.028 ms clock, 0.29+1.6/1.8/0+0.11 ms cpu, 6->6->2 MB, 7 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 33 @33.786s 0%: 0.089+2.4+0.023 ms clock, 0.35+1.4/2.3/0+0.093 ms cpu, 5->5->2 MB, 6 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 34 @33.793s 0%: 0.10+1.9+0.12 ms clock, 0.41+1.5/1.8/0+0.49 ms cpu, 5->6->2 MB, 6 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 35 @33.798s 0%: 0.069+2.3+0.017 ms clock, 0.27+1.3/2.2/0+0.071 ms cpu, 5->6->3 MB, 6 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 36 @33.804s 0%: 0.079+2.5+0.025 ms clock, 0.31+1.6/2.4/0+0.10 ms cpu, 5->6->3 MB, 6 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 37 @33.811s 0%: 0.063+2.2+0.064 ms clock, 0.25+1.0/2.1/0+0.25 ms cpu, 5->6->3 MB, 7 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 38 @33.817s 0%: 0.069+2.3+0.021 ms clock, 0.27+1.6/2.1/0+0.084 ms cpu, 6->6->3 MB, 6 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 39 @33.825s 0%: 0.11+2.2+0.024 ms clock, 0.46+2.1/2.0/0+0.098 ms cpu, 5->6->3 MB, 6 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 40 @33.831s 0%: 0.12+4.6+0.003 ms clock, 0.48+0.45/3.0/0+0.014 ms cpu, 5->6->3 MB, 6 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 41 @33.842s 0%: 0.065+2.0+0.021 ms clock, 0.26+2.5/1.8/0+0.085 ms cpu, 7->7->2 MB, 7 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 42 @33.847s 0%: 0.076+2.7+0.061 ms clock, 0.30+1.2/2.6/0+0.24 ms cpu, 5->5->3 MB, 6 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 43 @33.855s 0%: 0.076+2.1+0.021 ms clock, 0.30+1.4/2.0/0+0.087 ms cpu, 6->6->2 MB, 6 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 44 @33.860s 0%: 0.069+3.0+0.058 ms clock, 0.27+1.6/2.8/0+0.23 ms cpu, 5->6->3 MB, 6 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 45 @33.867s 0%: 0.077+2.3+0.026 ms clock, 0.30+1.5/2.3/0+0.10 ms cpu, 6->6->3 MB, 7 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 46 @33.873s 0%: 0.080+2.2+0.028 ms clock, 0.32+2.2/2.0/0+0.11 ms cpu, 5->6->3 MB, 7 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 47 @33.879s 0%: 0.075+2.2+0.025 ms clock, 0.30+1.9/2.0/0+0.10 ms cpu, 5->6->3 MB, 6 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 48 @33.884s 0%: 0.080+3.0+0.036 ms clock, 0.32+1.1/2.3/0+0.14 ms cpu, 5->6->3 MB, 6 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 49 @33.892s 0%: 0.071+2.4+0.035 ms clock, 0.28+1.8/2.1/0+0.14 ms cpu, 6->7->3 MB, 7 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 50 @33.898s 0%: 0.069+3.2+0.040 ms clock, 0.27+1.3/3.0/0+0.16 ms cpu, 5->6->3 MB, 6 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 51 @33.906s 0%: 0.082+2.1+0.020 ms clock, 0.33+1.7/2.0/0+0.081 ms cpu, 6->6->3 MB, 7 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 52 @33.912s 0%: 0.078+2.8+0.060 ms clock, 0.31+1.6/2.3/0+0.24 ms cpu, 5->6->3 MB, 6 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 53 @33.919s 0%: 0.079+2.5+0.028 ms clock, 0.31+1.3/2.4/0+0.11 ms cpu, 6->6->3 MB, 7 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 54 @33.925s 0%: 0.088+2.2+0.023 ms clock, 0.35+1.5/2.1/0+0.095 ms cpu, 6->6->3 MB, 7 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 55 @33.931s 0%: 0.098+2.7+0.020 ms clock, 0.39+1.8/2.6/0+0.080 ms cpu, 5->6->3 MB, 6 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 56 @33.937s 0%: 0.10+2.3+0.024 ms clock, 0.40+1.2/2.2/0+0.096 ms cpu, 5->6->3 MB, 6 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 57 @33.943s 0%: 0.068+2.2+0.017 ms clock, 0.27+1.8/2.1/0+0.070 ms cpu, 5->6->3 MB, 6 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 58 @33.949s 0%: 0.082+2.7+0.027 ms clock, 0.33+1.4/2.5/0+0.10 ms cpu, 5->6->3 MB, 6 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 59 @33.956s 0%: 0.10+3.5+0.003 ms clock, 0.42+1.5/2.9/0+0.015 ms cpu, 6->6->3 MB, 7 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 60 @33.964s 0%: 0.072+2.5+0.050 ms clock, 0.28+1.4/2.4/0+0.20 ms cpu, 5->6->3 MB, 6 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 61 @33.970s 0%: 0.075+2.2+0.024 ms clock, 0.30+1.5/2.1/0+0.099 ms cpu, 6->6->3 MB, 7 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 62 @33.976s 0%: 0.088+2.0+0.038 ms clock, 0.35+1.7/1.9/0+0.15 ms cpu, 5->6->2 MB, 6 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 63 @33.981s 0%: 0.071+2.1+0.032 ms clock, 0.28+1.3/1.9/0+0.13 ms cpu, 5->6->3 MB, 6 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 64 @33.987s 0%: 0.073+2.3+0.027 ms clock, 0.29+1.5/2.0/0+0.10 ms cpu, 5->6->3 MB, 6 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 65 @33.993s 0%: 0.080+2.2+0.026 ms clock, 0.32+1.5/2.1/0+0.10 ms cpu, 5->6->3 MB, 6 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 66 @33.999s 0%: 0.069+2.0+0.023 ms clock, 0.27+1.6/2.0/0+0.092 ms cpu, 5->6->2 MB, 6 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 67 @34.005s 0%: 0.20+2.1+0.024 ms clock, 0.80+1.2/2.0/0+0.098 ms cpu, 5->6->3 MB, 6 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 68 @34.011s 0%: 0.072+2.9+0.022 ms clock, 0.28+1.3/2.8/0+0.090 ms cpu, 5->6->3 MB, 6 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 69 @34.018s 0%: 0.069+2.1+0.019 ms clock, 0.27+1.5/2.0/0+0.078 ms cpu, 6->6->3 MB, 7 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 70 @34.024s 0%: 0.11+2.1+0.017 ms clock, 0.47+1.6/2.0/0+0.071 ms cpu, 5->6->3 MB, 6 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 71 @34.030s 0%: 0.12+2.9+0.025 ms clock, 0.50+1.9/2.6/0+0.10 ms cpu, 5->6->3 MB, 6 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 72 @34.037s 0%: 0.083+2.3+0.023 ms clock, 0.33+1.3/2.2/0+0.093 ms cpu, 6->6->3 MB, 7 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 73 @34.042s 0%: 0.074+2.1+0.023 ms clock, 0.29+1.9/2.1/0+0.093 ms cpu, 5->6->3 MB, 6 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 74 @34.048s 0%: 0.083+2.0+0.023 ms clock, 0.33+1.6/1.9/0+0.093 ms cpu, 5->6->3 MB, 6 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 75 @34.053s 0%: 0.088+2.7+0.019 ms clock, 0.35+1.8/2.7/0+0.077 ms cpu, 5->6->3 MB, 6 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 76 @34.060s 0%: 0.081+3.5+0.015 ms clock, 0.32+1.4/2.4/0+0.063 ms cpu, 6->6->3 MB, 7 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 77 @34.068s 0%: 0.073+1.9+0.024 ms clock, 0.29+2.0/1.8/0+0.096 ms cpu, 6->6->2 MB, 6 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 78 @34.074s 0%: 0.065+3.0+0.023 ms clock, 0.26+1.3/2.4/0+0.094 ms cpu, 5->6->3 MB, 6 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 79 @34.081s 0%: 0.066+2.4+0.024 ms clock, 0.26+1.4/2.2/0+0.097 ms cpu, 6->7->3 MB, 7 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 80 @34.088s 0%: 0.079+2.4+0.028 ms clock, 0.31+1.4/2.3/0+0.11 ms cpu, 5->6->3 MB, 6 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 81 @34.094s 0%: 0.074+2.6+0.024 ms clock, 0.29+1.7/2.6/0+0.099 ms cpu, 5->6->3 MB, 7 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 82 @34.101s 0%: 0.087+2.7+0.019 ms clock, 0.34+1.2/2.7/0+0.076 ms cpu, 6->7->3 MB, 7 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 83 @34.108s 0%: 0.091+3.5+0.003 ms clock, 0.36+1.7/2.4/0+0.015 ms cpu, 6->7->3 MB, 7 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 84 @34.116s 0%: 0.073+2.4+0.028 ms clock, 0.29+1.3/2.3/0+0.11 ms cpu, 6->7->3 MB, 7 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 85 @34.122s 0%: 0.089+2.5+0.018 ms clock, 0.35+1.5/2.4/0+0.073 ms cpu, 6->6->3 MB, 7 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 86 @34.129s 0%: 0.076+2.4+0.023 ms clock, 0.30+1.3/2.3/0+0.093 ms cpu, 5->6->3 MB, 6 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 87 @34.135s 0%: 0.078+2.1+0.022 ms clock, 0.31+1.9/2.1/0+0.090 ms cpu, 6->6->3 MB, 7 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 88 @34.140s 0%: 0.14+2.3+0.022 ms clock, 0.56+1.5/2.2/0+0.088 ms cpu, 5->6->3 MB, 6 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 89 @34.146s 0%: 0.14+2.2+0.018 ms clock, 0.56+1.4/2.1/0+0.075 ms cpu, 5->6->3 MB, 6 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 90 @34.153s 0%: 0.067+2.1+0.018 ms clock, 0.27+1.7/2.0/0+0.074 ms cpu, 5->6->3 MB, 7 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 91 @34.158s 0%: 0.082+2.4+0.063 ms clock, 0.32+1.3/2.2/0+0.25 ms cpu, 5->6->3 MB, 6 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 92 @34.165s 0%: 0.076+2.8+0.024 ms clock, 0.30+1.5/2.2/0+0.099 ms cpu, 6->6->3 MB, 7 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 93 @34.171s 0%: 0.088+2.9+0.015 ms clock, 0.35+1.3/2.3/0+0.060 ms cpu, 6->7->3 MB, 7 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 94 @34.179s 0%: 0.093+2.6+0.030 ms clock, 0.37+1.2/2.4/0+0.12 ms cpu, 6->7->3 MB, 7 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 95 @34.185s 0%: 0.070+2.6+0.021 ms clock, 0.28+1.4/2.5/0+0.084 ms cpu, 6->7->3 MB, 7 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 96 @34.193s 0%: 0.50+3.2+0.023 ms clock, 2.0+1.5/2.3/0+0.093 ms cpu, 6->7->3 MB, 7 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 97 @34.200s 0%: 0.084+2.5+0.025 ms clock, 0.33+1.2/2.3/0+0.10 ms cpu, 6->7->3 MB, 7 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 98 @34.207s 0%: 0.074+2.2+0.024 ms clock, 0.29+1.9/1.9/0+0.099 ms cpu, 6->6->3 MB, 7 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 99 @34.213s 0%: 0.14+2.6+0.028 ms clock, 0.56+1.6/2.4/0+0.11 ms cpu, 5->6->3 MB, 6 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 100 @34.219s 0%: 0.068+1.9+0.021 ms clock, 0.27+1.6/1.8/0+0.084 ms cpu, 5->6->2 MB, 7 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 101 @34.224s 0%: 0.079+3.4+0.021 ms clock, 0.31+1.8/2.9/0+0.087 ms cpu, 5->6->3 MB, 6 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 102 @34.232s 0%: 0.13+3.6+0.048 ms clock, 0.54+1.2/2.8/0+0.19 ms cpu, 6->8->3 MB, 8 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 103 @34.241s 0%: 0.081+2.5+0.022 ms clock, 0.32+1.1/2.4/0+0.090 ms cpu, 7->7->3 MB, 8 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 104 @34.247s 0%: 0.088+2.2+0.042 ms clock, 0.35+1.6/2.1/0+0.17 ms cpu, 6->6->3 MB, 7 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 105 @34.253s 0%: 0.070+1.9+0.021 ms clock, 0.28+1.6/1.8/0+0.087 ms cpu, 5->6->2 MB, 6 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 106 @34.259s 0%: 0.12+2.4+0.027 ms clock, 0.49+1.2/2.3/0+0.10 ms cpu, 5->6->3 MB, 6 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 107 @34.265s 0%: 0.072+2.2+0.024 ms clock, 0.28+1.6/2.1/0+0.096 ms cpu, 5->6->3 MB, 6 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 108 @34.271s 0%: 0.075+2.5+0.026 ms clock, 0.30+1.5/2.3/0+0.10 ms cpu, 5->6->3 MB, 6 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 109 @34.277s 0%: 0.065+2.1+0.022 ms clock, 0.26+1.3/2.0/0+0.090 ms cpu, 5->6->3 MB, 7 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 110 @34.283s 0%: 0.11+2.0+0.020 ms clock, 0.47+1.7/1.9/0+0.081 ms cpu, 5->6->3 MB, 6 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 111 @34.288s 0%: 1.3+3.2+0.040 ms clock, 5.3+0.76/2.6/0+0.16 ms cpu, 5->6->3 MB, 6 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 112 @34.297s 0%: 0.069+2.3+0.024 ms clock, 0.27+1.7/2.3/0+0.098 ms cpu, 6->7->3 MB, 7 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 113 @34.303s 0%: 0.074+3.8+0.28 ms clock, 0.29+2.2/2.6/0+1.1 ms cpu, 5->6->3 MB, 7 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 114 @34.313s 0%: 0.31+2.5+0.027 ms clock, 1.2+1.1/2.4/0+0.10 ms cpu, 6->7->3 MB, 7 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 115 @34.319s 0%: 0.078+2.7+0.025 ms clock, 0.31+1.7/2.5/0+0.10 ms cpu, 6->6->3 MB, 7 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 116 @34.326s 0%: 0.071+2.4+0.022 ms clock, 0.28+1.3/2.3/0+0.091 ms cpu, 5->6->3 MB, 7 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 117 @34.332s 0%: 0.16+4.1+0.004 ms clock, 0.67+0.93/2.7/0+0.018 ms cpu, 5->7->3 MB, 7 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 118 @34.341s 0%: 0.072+2.6+0.022 ms clock, 0.28+1.4/2.5/0+0.090 ms cpu, 7->7->3 MB, 8 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 119 @34.347s 0%: 0.084+2.3+0.024 ms clock, 0.33+2.3/2.2/0+0.099 ms cpu, 6->6->3 MB, 7 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 120 @34.353s 0%: 0.13+2.7+0.022 ms clock, 0.55+1.4/2.6/0+0.090 ms cpu, 5->6->3 MB, 6 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 121 @34.360s 0%: 0.086+3.1+0.039 ms clock, 0.34+1.2/2.9/0+0.15 ms cpu, 5->6->3 MB, 7 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 122 @34.367s 0%: 0.078+2.7+0.036 ms clock, 0.31+1.6/2.7/0+0.14 ms cpu, 6->7->3 MB, 7 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 123 @34.374s 0%: 0.094+2.0+0.020 ms clock, 0.37+1.9/1.9/0+0.082 ms cpu, 6->6->3 MB, 7 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 124 @34.380s 0%: 0.088+2.1+0.022 ms clock, 0.35+1.6/2.0/0+0.088 ms cpu, 5->6->3 MB, 6 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 125 @34.385s 0%: 0.11+2.5+0.14 ms clock, 0.44+2.0/1.7/0+0.59 ms cpu, 5->6->3 MB, 6 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 126 @34.392s 0%: 0.076+2.4+0.024 ms clock, 0.30+1.6/2.4/0+0.098 ms cpu, 5->6->3 MB, 6 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 127 @34.398s 0%: 0.064+2.2+0.024 ms clock, 0.25+1.6/2.1/0+0.099 ms cpu, 6->6->3 MB, 7 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 128 @34.404s 0%: 0.083+2.6+0.026 ms clock, 0.33+2.0/2.2/0+0.10 ms cpu, 5->6->3 MB, 6 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 129 @34.411s 0%: 0.072+2.5+0.029 ms clock, 0.29+1.0/2.4/0+0.11 ms cpu, 6->6->3 MB, 7 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 130 @34.417s 0%: 0.086+2.4+0.092 ms clock, 0.34+1.4/2.3/0+0.36 ms cpu, 6->7->3 MB, 7 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 131 @34.424s 0%: 0.10+4.4+0.030 ms clock, 0.40+1.4/2.4/0+0.12 ms cpu, 6->7->3 MB, 7 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 132 @34.433s 0%: 0.072+2.8+0.024 ms clock, 0.29+1.8/2.5/0+0.097 ms cpu, 6->7->3 MB, 7 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 133 @34.440s 0%: 0.072+2.7+0.028 ms clock, 0.29+1.3/2.5/0+0.11 ms cpu, 6->7->3 MB, 7 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 134 @34.446s 0%: 0.075+3.1+0.019 ms clock, 0.30+1.5/2.9/0+0.078 ms cpu, 6->7->3 MB, 7 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 135 @34.453s 0%: 0.069+2.2+0.033 ms clock, 0.27+1.6/2.0/0+0.13 ms cpu, 6->7->3 MB, 7 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 136 @34.460s 0%: 0.10+2.6+0.041 ms clock, 0.43+1.4/2.2/0+0.16 ms cpu, 6->6->3 MB, 7 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 137 @34.467s 0%: 0.10+4.6+0.003 ms clock, 0.41+7.9/0.030/0+0.015 ms cpu, 6->7->3 MB, 7 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 138 @34.476s 0%: 0.074+2.3+0.022 ms clock, 0.29+1.9/2.2/0+0.089 ms cpu, 6->7->3 MB, 8 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 139 @34.482s 0%: 0.077+2.4+0.019 ms clock, 0.30+1.4/2.4/0+0.076 ms cpu, 5->6->3 MB, 6 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 140 @34.488s 0%: 0.090+1.9+0.025 ms clock, 0.36+1.8/1.8/0+0.10 ms cpu, 5->6->2 MB, 7 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 141 @34.494s 0%: 0.13+2.3+0.025 ms clock, 0.53+1.8/2.2/0+0.10 ms cpu, 5->6->3 MB, 6 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 142 @34.500s 0%: 0.076+2.4+0.049 ms clock, 0.30+1.1/2.3/0+0.19 ms cpu, 5->6->3 MB, 6 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 143 @34.508s 0%: 0.075+1.7+0.022 ms clock, 0.30+2.1/1.6/0+0.091 ms cpu, 6->7->2 MB, 7 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 144 @34.513s 0%: 0.077+3.0+0.025 ms clock, 0.31+0.94/2.5/0+0.10 ms cpu, 5->6->3 MB, 6 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 145 @34.520s 0%: 0.087+2.0+0.022 ms clock, 0.35+1.6/1.9/0+0.091 ms cpu, 6->7->2 MB, 7 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 146 @34.526s 0%: 0.089+3.4+0.022 ms clock, 0.35+2.3/2.2/0+0.091 ms cpu, 5->6->3 MB, 6 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 147 @34.533s 0%: 0.087+2.1+0.026 ms clock, 0.34+1.7/2.0/0+0.10 ms cpu, 6->6->3 MB, 7 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 148 @34.538s 0%: 0.091+2.2+0.026 ms clock, 0.36+1.7/2.1/0+0.10 ms cpu, 5->6->3 MB, 6 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 149 @34.544s 0%: 0.066+2.4+0.024 ms clock, 0.26+1.5/2.2/0+0.098 ms cpu, 5->6->3 MB, 6 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 150 @34.550s 0%: 0.086+4.0+0.028 ms clock, 0.34+1.4/2.1/0+0.11 ms cpu, 5->7->3 MB, 7 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 151 @34.559s 0%: 0.090+2.5+0.028 ms clock, 0.36+1.3/2.4/0+0.11 ms cpu, 7->7->3 MB, 8 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 152 @34.567s 0%: 0.19+2.1+0.024 ms clock, 0.79+1.8/2.1/0+0.099 ms cpu, 6->6->2 MB, 7 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 153 @34.573s 0%: 0.090+2.4+0.028 ms clock, 0.36+1.5/2.3/0+0.11 ms cpu, 5->6->3 MB, 6 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 154 @34.579s 0%: 0.070+2.7+0.030 ms clock, 0.28+1.4/2.7/0+0.12 ms cpu, 6->6->3 MB, 7 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 155 @34.586s 0%: 0.10+4.4+0.026 ms clock, 0.41+1.7/2.2/0+0.10 ms cpu, 6->7->3 MB, 7 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 156 @34.596s 0%: 0.093+2.3+0.025 ms clock, 0.37+2.6/1.9/0+0.10 ms cpu, 7->7->3 MB, 8 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 157 @34.602s 0%: 0.11+3.2+0.029 ms clock, 0.44+1.8/3.1/0+0.11 ms cpu, 5->6->3 MB, 7 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 158 @34.610s 0%: 0.082+3.2+0.025 ms clock, 0.33+1.8/3.1/0+0.10 ms cpu, 6->7->3 MB, 7 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 159 @34.618s 0%: 0.071+2.3+0.024 ms clock, 0.28+1.7/2.2/0+0.098 ms cpu, 6->6->3 MB, 7 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 160 @34.624s 0%: 0.071+2.7+0.018 ms clock, 0.28+1.8/2.6/0+0.073 ms cpu, 5->6->3 MB, 6 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 161 @34.631s 0%: 0.13+2.4+0.017 ms clock, 0.53+1.5/2.3/0+0.069 ms cpu, 6->6->3 MB, 7 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 162 @34.637s 0%: 0.081+2.3+0.029 ms clock, 0.32+1.1/2.2/0+0.11 ms cpu, 5->6->2 MB, 7 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 163 @34.643s 0%: 0.091+2.5+0.020 ms clock, 0.36+1.3/2.4/0+0.082 ms cpu, 5->6->3 MB, 6 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 164 @34.649s 0%: 0.074+2.8+0.023 ms clock, 0.29+1.4/2.1/0+0.092 ms cpu, 6->6->3 MB, 7 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 165 @34.657s 0%: 0.067+2.4+0.022 ms clock, 0.26+1.4/2.3/0+0.090 ms cpu, 6->6->3 MB, 7 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 166 @34.663s 0%: 0.070+2.5+0.028 ms clock, 0.28+1.6/2.4/0+0.11 ms cpu, 5->6->3 MB, 7 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 167 @34.669s 0%: 0.095+2.4+0.025 ms clock, 0.38+1.7/2.3/0+0.10 ms cpu, 6->6->3 MB, 7 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 168 @34.675s 0%: 0.090+2.4+0.022 ms clock, 0.36+1.5/2.0/0+0.091 ms cpu, 5->6->3 MB, 7 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 169 @34.682s 0%: 0.092+3.0+0.027 ms clock, 0.36+1.5/3.0/0+0.10 ms cpu, 5->6->3 MB, 6 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 170 @34.689s 0%: 0.083+2.1+0.022 ms clock, 0.33+1.5/2.0/0+0.091 ms cpu, 6->6->3 MB, 7 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 171 @34.695s 0%: 0.088+2.3+0.035 ms clock, 0.35+1.6/1.9/0+0.14 ms cpu, 5->6->3 MB, 6 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 172 @34.701s 0%: 0.073+4.5+0.033 ms clock, 0.29+2.0/3.6/0+0.13 ms cpu, 5->6->3 MB, 6 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 173 @34.710s 0%: 0.069+4.3+0.034 ms clock, 0.27+1.0/3.0/0+0.13 ms cpu, 6->8->3 MB, 7 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 174 @34.719s 0%: 0.12+2.7+0.025 ms clock, 0.50+1.1/2.6/0+0.10 ms cpu, 7->8->3 MB, 8 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 175 @34.726s 0%: 0.072+2.8+0.028 ms clock, 0.29+1.2/2.6/0+0.11 ms cpu, 6->7->3 MB, 7 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 176 @34.733s 0%: 0.080+2.4+0.023 ms clock, 0.32+1.7/2.2/0+0.093 ms cpu, 6->6->3 MB, 7 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 177 @34.739s 0%: 0.080+2.5+0.021 ms clock, 0.32+1.8/2.4/0+0.084 ms cpu, 6->6->3 MB, 7 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 178 @34.746s 0%: 0.093+2.0+0.018 ms clock, 0.37+1.9/1.9/0+0.075 ms cpu, 6->6->2 MB, 7 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 179 @34.751s 0%: 0.086+3.8+0.027 ms clock, 0.34+1.8/1.9/0+0.10 ms cpu, 5->6->3 MB, 6 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 180 @34.760s 0%: 0.087+2.2+0.026 ms clock, 0.34+1.7/2.1/0+0.10 ms cpu, 6->7->3 MB, 8 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 181 @34.767s 0%: 0.067+2.3+0.022 ms clock, 0.27+2.2/2.1/0+0.089 ms cpu, 6->6->2 MB, 6 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 182 @34.773s 0%: 0.089+2.9+0.040 ms clock, 0.35+1.9/2.6/0+0.16 ms cpu, 5->6->3 MB, 6 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 183 @34.780s 0%: 0.068+2.6+0.031 ms clock, 0.27+1.5/2.2/0+0.12 ms cpu, 6->7->3 MB, 7 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 184 @34.788s 0%: 0.071+2.6+0.023 ms clock, 0.28+1.6/2.5/0+0.095 ms cpu, 6->6->3 MB, 7 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 185 @34.795s 0%: 0.090+2.3+0.027 ms clock, 0.36+1.3/2.2/0+0.11 ms cpu, 6->6->3 MB, 7 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 186 @34.801s 0%: 0.073+4.3+0.007 ms clock, 0.29+1.9/4.2/0+0.029 ms cpu, 5->7->3 MB, 7 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 187 @34.811s 0%: 0.092+2.4+0.024 ms clock, 0.36+1.4/2.3/0+0.099 ms cpu, 7->7->3 MB, 8 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 188 @34.818s 0%: 0.10+1.6+0.022 ms clock, 0.40+2.0/1.6/0+0.090 ms cpu, 6->6->2 MB, 6 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 189 @34.823s 0%: 0.10+2.3+0.021 ms clock, 0.40+1.3/2.1/0+0.086 ms cpu, 5->5->3 MB, 6 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 190 @34.829s 0%: 0.092+2.0+0.023 ms clock, 0.36+2.1/2.0/0+0.092 ms cpu, 6->6->2 MB, 6 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 191 @34.835s 0%: 0.086+2.3+0.064 ms clock, 0.34+1.4/2.2/0+0.25 ms cpu, 5->6->3 MB, 6 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 192 @34.841s 0%: 0.073+2.1+0.020 ms clock, 0.29+1.9/2.1/0+0.081 ms cpu, 6->6->3 MB, 7 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 193 @34.847s 0%: 0.084+2.2+0.022 ms clock, 0.33+1.5/2.1/0+0.088 ms cpu, 5->6->3 MB, 6 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 194 @34.853s 0%: 0.074+3.0+0.025 ms clock, 0.29+1.7/2.0/0+0.10 ms cpu, 5->6->3 MB, 6 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 195 @34.860s 0%: 0.074+3.1+0.026 ms clock, 0.29+1.4/3.1/0+0.10 ms cpu, 6->7->3 MB, 7 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 196 @34.867s 0%: 0.069+3.8+0.020 ms clock, 0.27+1.4/2.3/0+0.080 ms cpu, 6->7->3 MB, 7 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 197 @34.876s 0%: 0.090+2.6+0.032 ms clock, 0.36+0.95/2.5/0+0.13 ms cpu, 6->7->3 MB, 8 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 198 @34.883s 0%: 0.072+2.0+0.031 ms clock, 0.28+1.9/2.0/0+0.12 ms cpu, 6->6->3 MB, 7 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 199 @34.889s 0%: 0.091+2.4+0.024 ms clock, 0.36+1.5/2.0/0+0.096 ms cpu, 5->6->3 MB, 6 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 200 @34.895s 0%: 0.10+3.2+0.047 ms clock, 0.40+1.6/2.6/0+0.18 ms cpu, 5->6->3 MB, 6 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 201 @34.903s 0%: 0.10+2.7+0.032 ms clock, 0.42+1.1/2.6/0+0.13 ms cpu, 6->7->3 MB, 7 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 202 @34.910s 0%: 0.11+2.3+0.031 ms clock, 0.47+1.9/2.3/0+0.12 ms cpu, 6->6->3 MB, 7 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 203 @34.916s 0%: 0.087+3.2+0.033 ms clock, 0.35+2.6/2.4/0+0.13 ms cpu, 5->6->3 MB, 6 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 204 @34.924s 0%: 0.096+3.0+0.033 ms clock, 0.38+1.3/2.4/0+0.13 ms cpu, 6->7->3 MB, 7 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 205 @34.931s 0%: 0.095+2.3+0.024 ms clock, 0.38+1.7/2.3/0+0.096 ms cpu, 6->6->3 MB, 7 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 206 @34.937s 0%: 0.084+4.7+0.19 ms clock, 0.33+1.0/2.6/0+0.79 ms cpu, 5->7->4 MB, 6 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 207 @34.948s 0%: 0.073+2.1+0.033 ms clock, 0.29+1.9/1.9/0+0.13 ms cpu, 7->7->3 MB, 8 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 208 @34.954s 0%: 0.084+2.7+0.024 ms clock, 0.33+1.5/2.6/0+0.097 ms cpu, 5->6->3 MB, 6 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 209 @34.960s 0%: 0.071+2.7+0.025 ms clock, 0.28+1.7/2.2/0+0.10 ms cpu, 6->6->3 MB, 7 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 210 @34.967s 0%: 0.072+1.6+0.023 ms clock, 0.28+3.0/0.99/0+0.094 ms cpu, 6->6->2 MB, 7 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 211 @34.972s 0%: 0.066+2.4+0.019 ms clock, 0.26+1.3/2.3/0+0.079 ms cpu, 5->5->3 MB, 6 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 212 @34.978s 0%: 0.10+3.1+0.003 ms clock, 0.41+1.5/2.1/0+0.014 ms cpu, 5->6->3 MB, 7 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 213 @34.985s 0%: 0.10+3.1+0.033 ms clock, 0.42+0.96/2.9/0+0.13 ms cpu, 6->6->3 MB, 7 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 214 @34.992s 0%: 0.10+3.6+0.006 ms clock, 0.41+1.4/2.4/0+0.024 ms cpu, 6->7->3 MB, 7 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 215 @35.000s 0%: 0.11+3.1+0.026 ms clock, 0.47+0.62/3.0/0+0.10 ms cpu, 6->7->3 MB, 7 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 216 @35.008s 0%: 0.085+2.1+0.021 ms clock, 0.34+2.0/2.0/0+0.087 ms cpu, 6->7->3 MB, 7 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 217 @35.015s 0%: 0.11+2.2+0.026 ms clock, 0.47+2.1/2.1/0+0.10 ms cpu, 5->6->3 MB, 6 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 218 @35.020s 0%: 0.067+2.2+0.022 ms clock, 0.27+1.3/2.1/0+0.088 ms cpu, 5->6->3 MB, 6 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 219 @35.026s 0%: 0.077+2.6+0.026 ms clock, 0.31+1.5/2.4/0+0.10 ms cpu, 5->6->3 MB, 6 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 220 @35.033s 0%: 0.099+2.5+0.026 ms clock, 0.39+1.5/2.4/0+0.10 ms cpu, 6->6->3 MB, 7 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 221 @35.040s 0%: 0.090+2.7+0.021 ms clock, 0.36+1.1/2.5/0+0.085 ms cpu, 6->6->3 MB, 7 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 222 @35.046s 0%: 0.083+1.9+0.023 ms clock, 0.33+1.7/1.8/0+0.092 ms cpu, 6->6->3 MB, 7 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 223 @35.052s 0%: 0.10+2.4+0.017 ms clock, 0.41+1.8/2.3/0+0.068 ms cpu, 5->6->3 MB, 6 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 224 @35.058s 0%: 0.10+3.0+0.005 ms clock, 0.40+1.3/2.4/0+0.023 ms cpu, 6->6->3 MB, 7 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 225 @35.065s 0%: 0.088+2.8+0.027 ms clock, 0.35+1.6/2.7/0+0.10 ms cpu, 6->7->3 MB, 7 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 226 @35.073s 0%: 0.062+3.2+0.025 ms clock, 0.24+1.6/2.5/0+0.10 ms cpu, 6->7->3 MB, 7 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 227 @35.080s 0%: 0.093+4.1+0.011 ms clock, 0.37+2.2/2.2/0+0.046 ms cpu, 6->7->3 MB, 7 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 228 @35.089s 0%: 0.088+2.0+0.030 ms clock, 0.35+2.3/2.0/0+0.12 ms cpu, 6->6->3 MB, 7 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 229 @35.095s 0%: 0.093+2.1+0.021 ms clock, 0.37+1.6/2.1/0+0.086 ms cpu, 5->6->3 MB, 6 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 230 @35.101s 0%: 0.087+1.6+0.032 ms clock, 0.35+2.4/1.6/0+0.12 ms cpu, 5->5->2 MB, 6 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 231 @35.106s 0%: 0.082+2.0+0.023 ms clock, 0.32+2.1/2.0/0+0.092 ms cpu, 5->5->3 MB, 6 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 232 @35.111s 0%: 0.072+2.6+0.026 ms clock, 0.28+1.9/2.5/0+0.10 ms cpu, 5->6->3 MB, 6 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 233 @35.118s 0%: 0.073+3.3+0.015 ms clock, 0.29+1.4/2.4/0+0.061 ms cpu, 6->7->3 MB, 7 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 234 @35.126s 0%: 0.097+2.5+0.028 ms clock, 0.39+1.3/2.5/0+0.11 ms cpu, 6->7->3 MB, 7 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 235 @35.133s 0%: 0.087+2.7+0.031 ms clock, 0.34+1.4/2.6/0+0.12 ms cpu, 6->6->3 MB, 7 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 236 @35.139s 0%: 0.078+2.7+0.023 ms clock, 0.31+1.5/2.5/0+0.095 ms cpu, 6->7->3 MB, 7 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 237 @35.147s 0%: 0.069+3.9+0.014 ms clock, 0.27+1.3/2.4/0+0.059 ms cpu, 6->7->3 MB, 7 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 238 @35.155s 0%: 0.080+2.5+0.021 ms clock, 0.32+2.1/2.4/0+0.087 ms cpu, 6->7->3 MB, 7 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 239 @35.161s 0%: 0.073+2.5+0.027 ms clock, 0.29+1.6/2.4/0+0.10 ms cpu, 6->6->3 MB, 7 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 240 @35.168s 0%: 0.076+2.5+0.021 ms clock, 0.30+1.5/2.4/0+0.086 ms cpu, 5->6->3 MB, 7 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 241 @35.174s 0%: 0.086+3.1+0.027 ms clock, 0.34+1.3/2.5/0+0.11 ms cpu, 6->7->3 MB, 7 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 242 @35.182s 0%: 0.081+2.3+0.016 ms clock, 0.32+1.8/1.9/0+0.065 ms cpu, 6->7->3 MB, 7 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 243 @35.188s 0%: 0.080+2.9+0.028 ms clock, 0.32+1.2/2.8/0+0.11 ms cpu, 5->6->3 MB, 6 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 244 @35.195s 0%: 0.087+2.3+0.022 ms clock, 0.34+1.5/2.2/0+0.088 ms cpu, 6->6->3 MB, 7 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 245 @35.201s 0%: 0.071+3.2+0.026 ms clock, 0.28+0.76/2.6/0+0.10 ms cpu, 5->6->3 MB, 7 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 246 @35.208s 0%: 0.10+2.3+0.022 ms clock, 0.42+1.5/2.2/0+0.091 ms cpu, 6->7->3 MB, 7 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 247 @35.215s 0%: 0.067+2.5+0.020 ms clock, 0.27+1.2/2.4/0+0.083 ms cpu, 5->6->3 MB, 6 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 248 @35.221s 0%: 0.094+4.5+0.017 ms clock, 0.37+1.5/4.1/0.11+0.070 ms cpu, 6->7->3 MB, 7 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 249 @35.230s 0%: 0.18+2.8+0.026 ms clock, 0.74+0.76/2.7/0+0.10 ms cpu, 6->7->3 MB, 7 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 250 @35.238s 0%: 0.10+2.7+0.024 ms clock, 0.42+1.9/2.3/0+0.096 ms cpu, 6->7->3 MB, 7 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 251 @35.245s 0%: 0.081+3.3+0.024 ms clock, 0.32+1.6/3.2/0+0.096 ms cpu, 5->7->3 MB, 7 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 252 @35.253s 0%: 0.11+2.7+0.021 ms clock, 0.47+1.7/2.7/0+0.086 ms cpu, 6->7->3 MB, 8 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 253 @35.260s 0%: 0.10+2.4+0.026 ms clock, 0.42+1.5/2.3/0+0.10 ms cpu, 6->6->3 MB, 7 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 254 @35.266s 0%: 0.071+2.0+0.020 ms clock, 0.28+1.9/1.9/0+0.080 ms cpu, 5->6->2 MB, 7 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 255 @35.271s 0%: 0.080+2.4+0.088 ms clock, 0.32+1.4/2.2/0+0.35 ms cpu, 5->6->3 MB, 6 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 256 @35.278s 0%: 0.093+1.9+0.036 ms clock, 0.37+2.0/1.6/0+0.14 ms cpu, 5->6->2 MB, 7 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 257 @35.283s 0%: 0.10+2.8+0.025 ms clock, 0.42+1.4/2.7/0+0.10 ms cpu, 5->6->3 MB, 6 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 258 @35.290s 0%: 0.15+2.2+0.027 ms clock, 0.61+1.6/1.9/0+0.10 ms cpu, 6->6->3 MB, 7 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 259 @35.296s 0%: 0.19+2.6+0.025 ms clock, 0.77+1.6/2.5/0+0.10 ms cpu, 5->6->3 MB, 6 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 260 @35.302s 0%: 0.069+5.3+0.034 ms clock, 0.27+3.7/2.0/0+0.13 ms cpu, 6->7->3 MB, 7 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 261 @35.312s 0%: 0.095+2.8+0.030 ms clock, 0.38+0.46/2.6/0+0.12 ms cpu, 6->7->3 MB, 7 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 262 @35.320s 0%: 0.10+2.0+0.021 ms clock, 0.42+2.2/1.8/0+0.087 ms cpu, 6->6->2 MB, 7 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 263 @35.326s 0%: 0.081+2.5+0.033 ms clock, 0.32+1.5/2.3/0+0.13 ms cpu, 5->6->3 MB, 6 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 264 @35.333s 0%: 0.066+2.1+0.018 ms clock, 0.26+1.6/2.1/0+0.074 ms cpu, 6->6->3 MB, 7 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 265 @35.339s 0%: 0.087+3.0+0.027 ms clock, 0.34+1.7/2.2/0+0.11 ms cpu, 5->6->3 MB, 6 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 266 @35.346s 0%: 0.17+2.9+0.025 ms clock, 0.69+1.1/2.7/0+0.10 ms cpu, 6->7->3 MB, 7 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 267 @35.353s 0%: 0.12+2.1+0.024 ms clock, 0.49+1.6/2.0/0+0.098 ms cpu, 6->6->3 MB, 7 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 268 @35.359s 0%: 0.093+3.3+0.048 ms clock, 0.37+1.4/2.7/0+0.19 ms cpu, 5->6->3 MB, 6 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 269 @35.367s 0%: 0.069+4.6+0.026 ms clock, 0.27+2.0/4.4/0+0.10 ms cpu, 6->7->3 MB, 7 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 270 @35.376s 0%: 0.073+2.9+0.025 ms clock, 0.29+1.7/2.8/0+0.10 ms cpu, 6->7->3 MB, 7 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 271 @35.383s 0%: 0.068+2.4+0.021 ms clock, 0.27+2.0/2.3/0+0.087 ms cpu, 6->7->3 MB, 7 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 272 @35.389s 0%: 0.074+2.4+0.025 ms clock, 0.29+1.8/2.4/0+0.10 ms cpu, 6->6->3 MB, 7 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 273 @35.396s 0%: 0.083+2.1+0.027 ms clock, 0.33+1.7/2.0/0+0.11 ms cpu, 6->6->3 MB, 7 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 274 @35.401s 0%: 0.080+2.5+0.009 ms clock, 0.32+1.7/2.4/0+0.036 ms cpu, 5->6->3 MB, 6 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 275 @35.408s 0%: 0.081+1.9+0.020 ms clock, 0.32+1.8/1.8/0+0.083 ms cpu, 6->6->3 MB, 7 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 276 @35.413s 0%: 0.21+2.5+0.051 ms clock, 0.85+0.90/2.4/0+0.20 ms cpu, 5->6->3 MB, 6 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 277 @35.419s 0%: 0.079+3.7+0.026 ms clock, 0.31+1.3/2.4/0+0.10 ms cpu, 6->7->3 MB, 7 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 278 @35.428s 0%: 0.088+2.1+0.064 ms clock, 0.35+1.2/1.9/0+0.25 ms cpu, 7->7->2 MB, 8 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 279 @35.434s 0%: 0.11+2.1+0.022 ms clock, 0.46+1.4/2.0/0+0.090 ms cpu, 6->6->3 MB, 6 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 280 @35.440s 0%: 0.081+2.2+0.023 ms clock, 0.32+1.6/2.1/0+0.092 ms cpu, 5->6->3 MB, 6 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 281 @35.445s 0%: 0.95+1.3+0.026 ms clock, 3.8+1.2/2.1/0+0.10 ms cpu, 6->6->3 MB, 6 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 282 @35.451s 0%: 0.14+2.4+0.025 ms clock, 0.57+1.4/2.2/0+0.10 ms cpu, 5->6->3 MB, 6 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 283 @35.458s 0%: 0.097+3.6+0.028 ms clock, 0.39+1.4/2.4/0+0.11 ms cpu, 5->7->3 MB, 7 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 284 @35.466s 0%: 0.083+2.6+0.049 ms clock, 0.33+1.3/2.3/0+0.19 ms cpu, 6->7->3 MB, 8 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 285 @35.473s 0%: 0.093+3.4+0.024 ms clock, 0.37+1.0/2.7/0+0.098 ms cpu, 6->7->3 MB, 7 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 286 @35.480s 0%: 0.12+3.4+0.026 ms clock, 0.50+1.2/2.6/0+0.10 ms cpu, 6->7->3 MB, 7 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 287 @35.488s 0%: 0.084+3.1+0.023 ms clock, 0.33+1.0/3.0/0+0.093 ms cpu, 6->7->3 MB, 8 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 288 @35.496s 0%: 0.084+3.0+0.022 ms clock, 0.33+1.3/2.8/0+0.091 ms cpu, 6->7->3 MB, 7 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 289 @35.505s 0%: 0.075+2.6+0.018 ms clock, 0.30+1.6/2.5/0+0.073 ms cpu, 6->7->3 MB, 7 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 290 @35.511s 0%: 0.081+2.7+0.015 ms clock, 0.32+1.3/2.1/0+0.061 ms cpu, 6->7->3 MB, 7 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 291 @35.518s 0%: 0.086+2.6+0.028 ms clock, 0.34+1.1/2.4/0+0.11 ms cpu, 6->7->3 MB, 7 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 292 @35.525s 0%: 0.083+2.7+0.023 ms clock, 0.33+1.8/2.6/0+0.093 ms cpu, 6->7->3 MB, 7 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 293 @35.532s 0%: 0.20+2.4+0.020 ms clock, 0.83+1.5/2.3/0+0.081 ms cpu, 6->6->3 MB, 7 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 294 @35.538s 0%: 0.093+2.1+0.022 ms clock, 0.37+2.1/2.0/0+0.091 ms cpu, 6->6->3 MB, 6 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 295 @35.544s 0%: 0.099+2.6+0.021 ms clock, 0.39+1.3/2.4/0+0.087 ms cpu, 5->6->3 MB, 6 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 296 @35.551s 0%: 0.085+3.6+0.058 ms clock, 0.34+1.5/2.0/0+0.23 ms cpu, 6->7->3 MB, 7 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 297 @35.559s 0%: 0.10+2.5+0.026 ms clock, 0.40+1.2/2.4/0+0.10 ms cpu, 6->7->3 MB, 7 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 298 @35.566s 0%: 0.10+2.3+0.020 ms clock, 0.40+1.4/2.2/0+0.083 ms cpu, 6->7->3 MB, 7 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 299 @35.572s 0%: 0.081+2.5+0.027 ms clock, 0.32+1.7/2.4/0+0.11 ms cpu, 5->6->3 MB, 6 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 300 @35.579s 0%: 0.089+2.5+0.020 ms clock, 0.35+1.7/2.4/0+0.083 ms cpu, 6->6->3 MB, 7 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 301 @35.585s 0%: 0.077+3.2+0.031 ms clock, 0.31+1.2/2.5/0+0.12 ms cpu, 6->6->3 MB, 7 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 302 @35.593s 0%: 0.073+2.5+0.022 ms clock, 0.29+1.4/2.4/0+0.089 ms cpu, 6->7->3 MB, 7 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 303 @35.599s 0%: 0.080+2.5+0.022 ms clock, 0.32+1.5/2.2/0+0.091 ms cpu, 6->6->3 MB, 7 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 304 @35.605s 0%: 0.085+2.6+0.022 ms clock, 0.34+1.0/2.5/0+0.091 ms cpu, 5->6->3 MB, 7 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 305 @35.611s 0%: 0.12+2.5+0.028 ms clock, 0.51+1.4/2.3/0+0.11 ms cpu, 5->6->3 MB, 7 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 306 @35.618s 0%: 0.086+2.5+0.023 ms clock, 0.34+1.5/2.4/0+0.094 ms cpu, 6->6->3 MB, 7 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 307 @35.624s 0%: 0.090+3.0+0.034 ms clock, 0.36+1.1/2.5/0+0.13 ms cpu, 6->6->3 MB, 7 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 308 @35.633s 0%: 0.091+2.5+0.027 ms clock, 0.36+1.6/2.4/0+0.11 ms cpu, 6->7->3 MB, 7 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 309 @35.639s 0%: 0.13+1.9+0.021 ms clock, 0.52+1.9/1.9/0+0.086 ms cpu, 6->6->2 MB, 7 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 310 @35.644s 0%: 0.067+2.9+0.021 ms clock, 0.27+2.1/2.5/0+0.087 ms cpu, 5->6->3 MB, 6 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 311 @35.652s 0%: 0.072+1.4+0.027 ms clock, 0.29+4.1/0/0+0.10 ms cpu, 6->6->2 MB, 7 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 312 @35.656s 0%: 0.18+2.0+0.021 ms clock, 0.72+4.3/0/0+0.085 ms cpu, 5->5->2 MB, 6 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 313 @35.663s 0%: 0.084+1.3+0.020 ms clock, 0.33+2.3/1.1/0+0.083 ms cpu, 6->6->2 MB, 6 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 314 @35.667s 0%: 0.15+2.0+0.023 ms clock, 0.61+1.6/2.0/0+0.095 ms cpu, 4->5->3 MB, 5 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 315 @35.673s 0%: 0.072+1.9+0.023 ms clock, 0.29+2.5/1.1/0+0.093 ms cpu, 5->6->2 MB, 6 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 316 @35.678s 0%: 0.093+1.7+0.043 ms clock, 0.37+4.3/0/0+0.17 ms cpu, 5->5->2 MB, 6 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 317 @35.683s 0%: 0.10+1.4+0.031 ms clock, 0.42+3.4/0.20/0+0.12 ms cpu, 5->5->2 MB, 6 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 318 @35.687s 0%: 0.096+2.9+0.025 ms clock, 0.38+1.7/1.9/0+0.10 ms cpu, 4->5->3 MB, 5 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 319 @35.694s 0%: 0.083+2.7+0.026 ms clock, 0.33+1.6/2.6/0+0.10 ms cpu, 5->6->3 MB, 7 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 320 @35.701s 1%: 0.18+2.3+0.036 ms clock, 0.75+1.4/2.3/0+0.14 ms cpu, 6->6->2 MB, 7 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 321 @35.707s 1%: 0.091+3.0+0.025 ms clock, 0.36+1.7/2.7/0+0.10 ms cpu, 5->6->3 MB, 6 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 322 @35.714s 1%: 0.084+3.1+0.029 ms clock, 0.33+4.2/0/0+0.11 ms cpu, 5->6->3 MB, 6 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 323 @35.721s 1%: 0.067+2.7+0.025 ms clock, 0.27+1.5/2.6/0+0.10 ms cpu, 5->6->3 MB, 6 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 324 @35.728s 1%: 0.086+3.7+0.024 ms clock, 0.34+5.4/3.6/0+0.098 ms cpu, 6->6->3 MB, 7 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 325 @35.735s 1%: 0.072+2.8+0.024 ms clock, 0.28+1.3/2.5/0+0.097 ms cpu, 5->6->3 MB, 6 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 326 @35.743s 1%: 0.084+2.1+0.19 ms clock, 0.33+2.5/1.7/0+0.77 ms cpu, 6->7->2 MB, 7 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 327 @35.749s 1%: 0.074+3.1+0.038 ms clock, 0.29+1.4/2.6/0+0.15 ms cpu, 5->6->3 MB, 6 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 328 @35.758s 1%: 0.073+1.9+0.027 ms clock, 0.29+2.1/1.8/0+0.10 ms cpu, 6->7->2 MB, 7 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 329 @35.763s 1%: 0.083+2.4+0.034 ms clock, 0.33+2.1/2.2/0+0.13 ms cpu, 5->5->3 MB, 6 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 330 @35.770s 1%: 0.067+2.1+0.023 ms clock, 0.27+1.8/2.0/0+0.093 ms cpu, 6->6->3 MB, 6 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 331 @35.776s 1%: 0.092+2.5+0.022 ms clock, 0.36+1.2/2.4/0+0.090 ms cpu, 5->6->3 MB, 6 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 332 @35.783s 1%: 0.085+2.4+0.022 ms clock, 0.34+1.6/1.9/0+0.089 ms cpu, 6->6->3 MB, 7 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 333 @35.789s 1%: 0.067+2.6+0.025 ms clock, 0.26+1.4/2.3/0+0.10 ms cpu, 5->6->3 MB, 6 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 334 @35.795s 1%: 0.11+2.8+0.022 ms clock, 0.44+1.5/2.7/0+0.089 ms cpu, 6->6->3 MB, 7 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 335 @35.802s 1%: 0.082+2.6+0.022 ms clock, 0.33+1.8/2.5/0+0.089 ms cpu, 6->7->3 MB, 7 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 336 @35.809s 1%: 0.089+2.8+0.022 ms clock, 0.35+1.4/2.5/0+0.089 ms cpu, 6->6->3 MB, 7 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 337 @35.815s 1%: 0.081+2.5+0.055 ms clock, 0.32+1.3/2.3/0+0.22 ms cpu, 6->6->3 MB, 7 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 338 @35.822s 1%: 0.089+2.1+0.023 ms clock, 0.35+1.8/2.0/0+0.095 ms cpu, 6->6->2 MB, 7 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 339 @35.827s 1%: 0.069+2.4+0.023 ms clock, 0.27+1.2/2.4/0+0.095 ms cpu, 5->6->3 MB, 6 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 340 @35.833s 1%: 0.071+2.4+0.039 ms clock, 0.28+1.3/2.3/0+0.15 ms cpu, 5->6->3 MB, 6 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 341 @35.840s 1%: 0.090+3.2+0.024 ms clock, 0.36+3.2/3.1/0+0.096 ms cpu, 5->6->3 MB, 7 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 342 @35.847s 1%: 0.098+2.0+0.022 ms clock, 0.39+1.9/1.9/0+0.089 ms cpu, 6->6->2 MB, 7 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 343 @35.853s 1%: 0.11+1.9+0.041 ms clock, 0.44+1.5/1.9/0+0.16 ms cpu, 5->6->2 MB, 6 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 344 @35.859s 1%: 0.071+2.4+0.026 ms clock, 0.28+1.4/2.3/0+0.10 ms cpu, 5->6->3 MB, 6 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 345 @35.865s 1%: 0.076+4.5+0.022 ms clock, 0.30+2.7/3.9/0.10+0.088 ms cpu, 5->6->3 MB, 7 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 346 @35.873s 1%: 0.078+4.1+0.029 ms clock, 0.31+0.97/2.7/0+0.11 ms cpu, 6->7->4 MB, 7 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 347 @35.883s 1%: 0.064+2.4+0.027 ms clock, 0.25+1.5/2.3/0+0.10 ms cpu, 7->8->3 MB, 8 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 348 @35.889s 1%: 0.076+2.6+0.026 ms clock, 0.30+1.3/2.5/0+0.10 ms cpu, 5->6->3 MB, 7 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 349 @35.895s 1%: 0.089+2.4+0.024 ms clock, 0.35+2.3/2.3/0+0.099 ms cpu, 5->6->3 MB, 7 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 350 @35.902s 1%: 0.076+2.3+0.025 ms clock, 0.30+2.0/2.3/0+0.10 ms cpu, 5->6->3 MB, 7 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 351 @35.908s 1%: 0.12+2.5+0.019 ms clock, 0.50+2.1/2.4/0+0.078 ms cpu, 5->6->3 MB, 7 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 352 @35.914s 1%: 0.073+2.3+0.024 ms clock, 0.29+2.0/2.2/0+0.097 ms cpu, 5->6->3 MB, 7 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 353 @35.920s 1%: 0.072+2.3+0.027 ms clock, 0.28+1.6/2.0/0+0.10 ms cpu, 5->6->3 MB, 6 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 354 @35.926s 1%: 0.083+1.9+0.023 ms clock, 0.33+1.8/1.8/0+0.093 ms cpu, 5->6->2 MB, 6 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 355 @35.932s 1%: 0.14+2.3+0.018 ms clock, 0.58+1.4/2.2/0+0.072 ms cpu, 5->6->3 MB, 6 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 356 @35.938s 1%: 0.089+2.6+0.018 ms clock, 0.35+1.2/2.3/0+0.072 ms cpu, 5->6->3 MB, 7 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 357 @35.944s 1%: 0.078+2.5+0.032 ms clock, 0.31+1.2/2.4/0+0.13 ms cpu, 6->6->3 MB, 7 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 358 @35.950s 1%: 0.10+2.1+0.024 ms clock, 0.41+1.5/2.0/0+0.099 ms cpu, 5->6->3 MB, 7 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 359 @35.956s 1%: 0.063+2.3+0.026 ms clock, 0.25+1.6/2.3/0+0.10 ms cpu, 5->6->3 MB, 6 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 360 @35.962s 1%: 0.071+2.1+0.024 ms clock, 0.28+1.5/2.0/0+0.097 ms cpu, 5->6->2 MB, 6 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 361 @35.969s 1%: 0.13+3.0+0.022 ms clock, 0.53+1.7/2.9/0+0.091 ms cpu, 5->6->3 MB, 6 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 362 @35.976s 1%: 0.088+2.1+0.021 ms clock, 0.35+1.9/2.0/0+0.087 ms cpu, 5->6->3 MB, 7 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 363 @35.981s 1%: 0.080+2.4+0.024 ms clock, 0.32+1.2/2.3/0+0.098 ms cpu, 5->6->3 MB, 6 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 364 @35.987s 1%: 0.091+2.2+0.026 ms clock, 0.36+1.6/2.1/0+0.10 ms cpu, 5->6->3 MB, 7 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 365 @35.993s 1%: 0.11+2.1+0.023 ms clock, 0.46+1.3/1.7/0+0.095 ms cpu, 5->6->2 MB, 6 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 366 @35.999s 1%: 0.091+2.0+0.023 ms clock, 0.36+1.2/2.0/0+0.093 ms cpu, 5->6->2 MB, 6 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 367 @36.004s 1%: 0.10+2.1+0.021 ms clock, 0.43+1.2/2.0/0+0.087 ms cpu, 5->6->2 MB, 6 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 368 @36.010s 1%: 0.095+1.6+0.017 ms clock, 0.38+1.6/1.6/0+0.071 ms cpu, 5->5->2 MB, 6 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 369 @36.015s 1%: 0.084+2.1+0.037 ms clock, 0.33+1.5/2.0/0+0.14 ms cpu, 5->5->3 MB, 6 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 370 @36.020s 1%: 0.14+2.1+0.023 ms clock, 0.57+1.5/2.0/0+0.093 ms cpu, 5->6->3 MB, 6 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 371 @36.026s 1%: 0.074+2.1+0.020 ms clock, 0.29+1.5/2.1/0+0.082 ms cpu, 5->6->3 MB, 6 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 372 @36.032s 1%: 0.067+1.9+0.017 ms clock, 0.27+1.8/1.8/0+0.071 ms cpu, 5->6->2 MB, 6 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 373 @36.037s 1%: 0.097+2.0+0.019 ms clock, 0.39+1.4/2.0/0+0.078 ms cpu, 5->5->2 MB, 6 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 374 @36.042s 1%: 0.10+2.3+0.016 ms clock, 0.41+1.3/2.2/0+0.067 ms cpu, 5->6->3 MB, 6 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 375 @36.048s 1%: 0.088+2.3+0.018 ms clock, 0.35+1.3/2.2/0+0.074 ms cpu, 5->6->3 MB, 6 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 376 @36.054s 1%: 0.089+2.4+0.051 ms clock, 0.35+1.3/2.0/0+0.20 ms cpu, 5->6->3 MB, 6 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 377 @36.060s 1%: 0.089+2.1+0.026 ms clock, 0.35+1.5/2.0/0+0.10 ms cpu, 5->6->3 MB, 6 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 378 @36.065s 1%: 0.11+2.7+0.024 ms clock, 0.45+1.0/2.6/0+0.097 ms cpu, 5->6->3 MB, 6 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 379 @36.072s 1%: 0.069+2.1+0.018 ms clock, 0.27+1.3/2.1/0+0.075 ms cpu, 6->6->3 MB, 7 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 380 @36.077s 1%: 0.091+1.8+0.023 ms clock, 0.36+1.6/1.7/0+0.092 ms cpu, 5->6->2 MB, 6 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 381 @36.083s 1%: 0.080+2.5+0.025 ms clock, 0.32+1.2/2.3/0+0.10 ms cpu, 5->6->3 MB, 6 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 382 @36.090s 1%: 0.073+1.8+0.048 ms clock, 0.29+1.7/1.6/0+0.19 ms cpu, 5->6->2 MB, 7 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 383 @36.095s 1%: 0.090+2.8+0.023 ms clock, 0.36+1.6/2.7/0+0.094 ms cpu, 5->6->3 MB, 6 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 384 @36.101s 1%: 0.12+2.3+0.019 ms clock, 0.50+1.4/2.2/0+0.077 ms cpu, 6->7->3 MB, 7 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 385 @36.112s 1%: 0.063+1.7+0.022 ms clock, 0.25+2.4/1.7/0+0.090 ms cpu, 6->7->2 MB, 6 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 386 @36.117s 1%: 0.072+2.2+0.022 ms clock, 0.28+1.9/2.1/0+0.088 ms cpu, 5->5->3 MB, 6 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 387 @36.123s 1%: 0.070+2.1+0.018 ms clock, 0.28+1.8/2.0/0+0.075 ms cpu, 5->6->3 MB, 6 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 388 @36.128s 1%: 0.070+3.2+0.016 ms clock, 0.28+1.7/2.4/0+0.067 ms cpu, 5->6->3 MB, 6 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 389 @36.136s 1%: 0.14+2.9+0.096 ms clock, 0.56+1.6/2.8/0+0.38 ms cpu, 6->7->3 MB, 7 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 390 @36.143s 1%: 0.089+2.6+0.027 ms clock, 0.35+1.5/2.0/0+0.11 ms cpu, 6->7->3 MB, 7 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 391 @36.151s 1%: 0.074+1.7+0.026 ms clock, 0.29+2.1/1.6/0+0.10 ms cpu, 6->7->2 MB, 7 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 392 @36.156s 1%: 0.099+2.5+0.022 ms clock, 0.39+1.2/2.4/0+0.091 ms cpu, 5->6->3 MB, 6 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 393 @36.163s 1%: 0.068+1.8+0.019 ms clock, 0.27+1.8/1.8/0+0.079 ms cpu, 6->6->2 MB, 7 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 394 @36.168s 1%: 0.092+2.4+0.021 ms clock, 0.37+1.3/2.3/0+0.085 ms cpu, 5->6->3 MB, 6 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 395 @36.175s 1%: 0.084+2.0+0.020 ms clock, 0.33+1.7/1.9/0+0.083 ms cpu, 6->6->3 MB, 7 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 396 @36.180s 1%: 0.082+2.3+0.021 ms clock, 0.32+1.5/2.3/0+0.085 ms cpu, 5->6->3 MB, 6 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 397 @36.186s 1%: 0.066+2.5+0.021 ms clock, 0.26+1.5/2.4/0+0.084 ms cpu, 6->6->3 MB, 7 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 398 @36.193s 1%: 0.079+2.3+0.030 ms clock, 0.31+1.5/2.2/0+0.12 ms cpu, 6->6->3 MB, 7 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 399 @36.199s 1%: 0.092+2.4+0.028 ms clock, 0.36+1.4/2.3/0+0.11 ms cpu, 6->6->3 MB, 7 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 400 @36.205s 1%: 0.068+1.9+0.020 ms clock, 0.27+1.6/1.8/0+0.080 ms cpu, 5->6->2 MB, 7 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 401 @36.210s 1%: 0.093+2.0+0.018 ms clock, 0.37+1.1/1.9/0+0.074 ms cpu, 5->6->2 MB, 6 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 402 @36.216s 1%: 0.091+2.3+0.030 ms clock, 0.36+1.3/2.2/0+0.12 ms cpu, 5->5->3 MB, 6 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 403 @36.221s 1%: 0.11+2.2+0.006 ms clock, 0.44+1.3/2.1/0+0.024 ms cpu, 5->6->3 MB, 6 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 404 @36.227s 1%: 0.074+1.8+0.021 ms clock, 0.29+1.5/1.7/0+0.086 ms cpu, 5->6->2 MB, 6 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 405 @36.232s 1%: 0.082+3.0+0.029 ms clock, 0.33+1.4/2.0/0+0.11 ms cpu, 5->6->3 MB, 6 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 406 @36.240s 1%: 0.089+3.0+0.025 ms clock, 0.35+1.4/2.9/0+0.10 ms cpu, 6->7->3 MB, 7 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 407 @36.247s 1%: 0.095+2.2+0.025 ms clock, 0.38+1.4/2.1/0+0.10 ms cpu, 6->6->3 MB, 7 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 408 @36.254s 1%: 0.077+2.5+0.025 ms clock, 0.30+1.5/2.4/0+0.10 ms cpu, 5->6->3 MB, 6 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 409 @36.261s 1%: 0.10+2.7+0.026 ms clock, 0.41+1.3/2.6/0+0.10 ms cpu, 5->6->3 MB, 7 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 410 @36.267s 1%: 0.080+2.0+0.018 ms clock, 0.32+1.5/1.9/0+0.075 ms cpu, 6->6->3 MB, 7 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 411 @36.273s 1%: 0.48+3.3+0.027 ms clock, 1.9+0.68/3.0/0+0.11 ms cpu, 5->6->3 MB, 6 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 412 @36.280s 1%: 0.082+2.4+0.034 ms clock, 0.33+1.1/2.3/0+0.13 ms cpu, 6->6->3 MB, 7 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 413 @36.286s 1%: 0.087+2.2+0.026 ms clock, 0.35+1.5/2.1/0+0.10 ms cpu, 5->6->3 MB, 7 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 414 @36.292s 1%: 0.090+2.3+0.027 ms clock, 0.36+1.1/2.2/0+0.10 ms cpu, 5->6->3 MB, 6 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 415 @36.298s 1%: 0.10+1.9+0.022 ms clock, 0.40+1.6/1.8/0+0.088 ms cpu, 5->6->2 MB, 7 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 416 @36.304s 1%: 0.070+3.8+0.53 ms clock, 0.28+2.0/2.8/0+2.1 ms cpu, 5->6->3 MB, 6 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 417 @36.314s 1%: 0.080+3.4+0.023 ms clock, 0.32+1.1/2.3/0+0.094 ms cpu, 6->7->3 MB, 7 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 418 @36.321s 1%: 0.085+3.0+0.027 ms clock, 0.34+1.4/2.1/0+0.11 ms cpu, 6->7->3 MB, 7 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 419 @36.328s 1%: 0.12+2.4+0.026 ms clock, 0.51+1.2/2.4/0+0.10 ms cpu, 6->7->3 MB, 7 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 420 @36.334s 1%: 0.090+2.9+0.017 ms clock, 0.36+1.8/2.8/0+0.071 ms cpu, 5->6->3 MB, 7 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 421 @36.342s 1%: 0.087+2.2+0.023 ms clock, 0.34+1.6/2.1/0+0.095 ms cpu, 6->6->3 MB, 7 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 422 @36.347s 1%: 0.072+2.2+0.024 ms clock, 0.29+1.9/2.1/0+0.096 ms cpu, 5->6->2 MB, 6 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 423 @36.353s 1%: 0.11+2.1+0.022 ms clock, 0.46+1.6/2.0/0+0.088 ms cpu, 5->5->2 MB, 6 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 424 @36.360s 1%: 0.085+14+0.005 ms clock, 0.34+0.76/4.2/0+0.020 ms cpu, 5->8->5 MB, 6 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 425 @36.387s 1%: 0.18+4.2+0.023 ms clock, 0.72+1.6/4.1/0+0.094 ms cpu, 9->10->3 MB, 11 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 426 @36.399s 1%: 0.13+4.4+0.021 ms clock, 0.53+2.5/4.2/0+0.084 ms cpu, 7->8->3 MB, 8 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 427 @36.407s 1%: 0.085+4.4+0.42 ms clock, 0.34+1.7/3.0/0+1.6 ms cpu, 5->6->3 MB, 6 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 428 @36.420s 1%: 0.089+3.2+0.027 ms clock, 0.35+0.99/2.9/0+0.10 ms cpu, 7->8->3 MB, 8 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 429 @36.427s 1%: 0.090+2.5+0.020 ms clock, 0.36+1.3/2.4/0+0.081 ms cpu, 6->7->3 MB, 7 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 430 @36.436s 1%: 0.071+2.6+0.022 ms clock, 0.28+1.8/2.6/0+0.091 ms cpu, 6->6->3 MB, 7 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 431 @36.442s 1%: 0.085+2.1+0.034 ms clock, 0.34+2.0/2.0/0+0.13 ms cpu, 6->6->3 MB, 7 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 432 @36.448s 1%: 0.076+4.5+0.024 ms clock, 0.30+4.6/4.4/0+0.098 ms cpu, 5->6->3 MB, 6 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 433 @36.457s 1%: 0.087+2.6+0.021 ms clock, 0.34+1.7/2.1/0+0.084 ms cpu, 5->6->3 MB, 7 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 434 @36.463s 1%: 0.086+3.3+0.059 ms clock, 0.34+1.6/2.7/0+0.23 ms cpu, 6->6->3 MB, 7 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 435 @36.472s 1%: 0.080+2.7+0.021 ms clock, 0.32+1.8/2.6/0+0.084 ms cpu, 6->6->3 MB, 7 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 436 @36.478s 1%: 0.087+2.3+0.028 ms clock, 0.34+1.8/2.1/0+0.11 ms cpu, 6->6->3 MB, 7 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 437 @36.486s 1%: 0.093+2.7+0.026 ms clock, 0.37+1.0/2.7/0+0.10 ms cpu, 5->6->3 MB, 6 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 438 @36.493s 1%: 0.10+2.5+0.021 ms clock, 0.42+1.9/2.4/0+0.084 ms cpu, 6->6->3 MB, 7 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 439 @36.499s 1%: 0.58+7.5+0.041 ms clock, 2.3+0.77/4.2/0+0.16 ms cpu, 5->7->4 MB, 7 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 440 @36.513s 1%: 0.075+3.8+0.034 ms clock, 0.30+1.6/2.3/0+0.13 ms cpu, 7->8->3 MB, 9 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 441 @36.523s 1%: 0.098+3.2+0.025 ms clock, 0.39+0.97/3.1/0+0.10 ms cpu, 6->7->3 MB, 7 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 442 @36.531s 1%: 0.071+4.5+0.047 ms clock, 0.28+3.8/3.2/0+0.18 ms cpu, 6->7->3 MB, 7 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 443 @36.540s 1%: 0.11+2.5+0.027 ms clock, 0.44+1.8/2.2/0+0.10 ms cpu, 6->7->3 MB, 7 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 444 @36.547s 1%: 0.090+2.2+0.022 ms clock, 0.36+1.7/2.1/0+0.088 ms cpu, 6->6->3 MB, 7 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 445 @36.554s 1%: 0.090+3.6+0.027 ms clock, 0.36+1.7/2.2/0+0.11 ms cpu, 5->6->3 MB, 6 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 446 @36.562s 1%: 0.10+2.9+0.032 ms clock, 0.40+6.0/0/0+0.12 ms cpu, 6->7->3 MB, 8 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 447 @36.572s 1%: 0.10+3.7+0.004 ms clock, 0.41+2.8/2.9/0+0.018 ms cpu, 5->6->3 MB, 6 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 448 @36.582s 1%: 0.075+6.4+0.034 ms clock, 0.30+1.0/2.8/0+0.13 ms cpu, 6->8->3 MB, 7 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 449 @36.593s 1%: 0.093+2.7+0.037 ms clock, 0.37+0.89/2.5/0+0.14 ms cpu, 6->7->3 MB, 8 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 450 @36.601s 1%: 0.076+1.7+0.021 ms clock, 0.30+1.9/1.5/0+0.086 ms cpu, 6->6->2 MB, 7 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 451 @36.606s 1%: 0.13+2.2+0.024 ms clock, 0.53+1.7/2.0/0+0.099 ms cpu, 5->5->3 MB, 6 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 452 @36.612s 1%: 0.088+2.7+0.025 ms clock, 0.35+2.0/2.2/0+0.10 ms cpu, 5->6->3 MB, 6 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 453 @36.618s 1%: 0.097+2.4+0.028 ms clock, 0.38+1.7/2.2/0+0.11 ms cpu, 6->6->3 MB, 7 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 454 @36.625s 1%: 0.090+2.6+0.020 ms clock, 0.36+2.1/2.5/0+0.083 ms cpu, 5->6->3 MB, 6 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 455 @36.631s 1%: 0.096+2.3+0.026 ms clock, 0.38+1.4/2.2/0+0.10 ms cpu, 5->6->3 MB, 7 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 456 @36.638s 1%: 0.074+2.5+0.030 ms clock, 0.29+1.7/2.0/0+0.12 ms cpu, 6->7->3 MB, 7 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 457 @36.645s 1%: 0.12+2.5+0.023 ms clock, 0.50+1.2/2.4/0+0.092 ms cpu, 6->6->3 MB, 7 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 458 @36.652s 1%: 0.069+2.2+0.027 ms clock, 0.27+1.4/2.2/0+0.10 ms cpu, 6->7->3 MB, 7 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 459 @36.658s 1%: 0.079+2.5+0.028 ms clock, 0.31+1.1/2.4/0+0.11 ms cpu, 5->6->3 MB, 7 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 460 @36.664s 1%: 0.068+2.8+0.026 ms clock, 0.27+1.5/2.3/0+0.10 ms cpu, 6->7->3 MB, 7 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 461 @36.671s 1%: 0.076+2.3+0.046 ms clock, 0.30+1.7/2.1/0+0.18 ms cpu, 6->6->3 MB, 7 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 462 @36.678s 1%: 0.071+2.2+0.020 ms clock, 0.28+1.6/2.2/0+0.082 ms cpu, 6->6->2 MB, 6 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 463 @36.683s 1%: 0.076+2.6+0.025 ms clock, 0.30+1.0/2.5/0+0.10 ms cpu, 5->6->3 MB, 6 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 464 @36.690s 1%: 0.070+2.6+0.026 ms clock, 0.28+1.3/2.5/0+0.10 ms cpu, 6->6->3 MB, 7 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 465 @36.696s 1%: 0.11+2.0+0.026 ms clock, 0.46+1.5/2.0/0+0.10 ms cpu, 6->6->2 MB, 7 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 466 @36.702s 1%: 0.24+3.9+0.10 ms clock, 0.99+0.91/2.5/0+0.40 ms cpu, 5->6->3 MB, 6 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 467 @36.710s 1%: 0.11+2.2+0.026 ms clock, 0.46+2.0/2.1/0+0.10 ms cpu, 6->6->3 MB, 7 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 468 @36.716s 1%: 0.074+2.1+0.080 ms clock, 0.29+1.5/2.0/0+0.32 ms cpu, 5->6->2 MB, 6 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 469 @36.722s 1%: 0.093+2.4+0.027 ms clock, 0.37+1.4/2.3/0+0.11 ms cpu, 5->6->3 MB, 6 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 470 @36.728s 1%: 0.10+2.9+0.019 ms clock, 0.43+1.1/2.4/0+0.079 ms cpu, 5->6->3 MB, 6 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 471 @36.735s 1%: 0.075+3.1+0.035 ms clock, 0.30+0.98/2.5/0+0.14 ms cpu, 6->7->3 MB, 7 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 472 @36.743s 1%: 0.088+2.5+0.023 ms clock, 0.35+1.2/2.3/0+0.095 ms cpu, 6->6->3 MB, 7 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 473 @36.749s 1%: 0.095+2.0+0.024 ms clock, 0.38+1.6/2.0/0+0.096 ms cpu, 5->6->2 MB, 6 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 474 @36.755s 1%: 0.12+1.9+0.020 ms clock, 0.51+1.3/1.8/0+0.083 ms cpu, 5->5->2 MB, 6 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 475 @36.761s 1%: 0.073+2.5+0.026 ms clock, 0.29+1.9/2.2/0+0.10 ms cpu, 5->6->3 MB, 6 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 476 @36.767s 1%: 0.071+2.5+0.023 ms clock, 0.28+1.1/2.4/0+0.095 ms cpu, 5->6->3 MB, 6 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 477 @36.774s 1%: 0.088+2.0+0.023 ms clock, 0.35+1.4/1.9/0+0.093 ms cpu, 6->6->2 MB, 6 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 478 @36.779s 1%: 0.063+2.6+0.041 ms clock, 0.25+0.93/2.5/0+0.16 ms cpu, 5->6->3 MB, 6 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 479 @36.785s 1%: 0.066+2.5+0.030 ms clock, 0.26+1.1/2.2/0+0.12 ms cpu, 6->6->3 MB, 7 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 480 @36.791s 1%: 0.10+2.2+0.021 ms clock, 0.42+1.6/2.1/0+0.086 ms cpu, 5->6->3 MB, 6 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 481 @36.797s 1%: 0.096+2.1+0.032 ms clock, 0.38+1.4/2.0/0+0.13 ms cpu, 5->6->2 MB, 6 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 482 @36.802s 1%: 0.073+2.5+0.028 ms clock, 0.29+1.0/2.4/0+0.11 ms cpu, 5->6->3 MB, 6 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 483 @36.809s 1%: 0.092+1.8+0.029 ms clock, 0.37+1.6/1.7/0+0.11 ms cpu, 5->6->2 MB, 7 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 484 @36.814s 1%: 0.086+2.1+0.020 ms clock, 0.34+1.4/2.0/0+0.081 ms cpu, 5->5->2 MB, 6 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 485 @36.819s 1%: 0.17+1.5+0.023 ms clock, 0.68+1.8/1.4/0+0.095 ms cpu, 5->5->2 MB, 6 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 486 @36.824s 1%: 0.066+2.0+0.027 ms clock, 0.26+1.4/1.6/0+0.11 ms cpu, 5->5->2 MB, 5 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 487 @36.829s 1%: 0.087+1.8+0.017 ms clock, 0.34+1.7/1.7/0+0.071 ms cpu, 5->5->2 MB, 6 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 488 @36.834s 1%: 0.066+3.6+0.009 ms clock, 0.26+3.7/2.6/0+0.036 ms cpu, 5->6->3 MB, 6 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 489 @36.843s 1%: 0.087+2.5+0.022 ms clock, 0.34+1.2/2.4/0+0.088 ms cpu, 6->7->3 MB, 7 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 490 @36.851s 1%: 0.086+1.2+0.018 ms clock, 0.34+2.0/1.1/0+0.072 ms cpu, 6->6->2 MB, 7 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 491 @36.855s 1%: 0.070+1.7+0.022 ms clock, 0.28+1.6/1.7/0+0.091 ms cpu, 4->5->2 MB, 5 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 492 @36.861s 1%: 0.087+1.8+0.022 ms clock, 0.35+1.6/1.7/0+0.090 ms cpu, 5->6->2 MB, 6 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 493 @36.866s 1%: 0.080+2.0+0.022 ms clock, 0.32+1.2/2.0/0+0.088 ms cpu, 5->6->2 MB, 6 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 494 @36.872s 1%: 0.077+1.6+0.033 ms clock, 0.30+1.5/1.5/0+0.13 ms cpu, 5->6->2 MB, 6 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 495 @36.877s 1%: 0.071+2.6+0.026 ms clock, 0.28+1.6/1.6/0+0.10 ms cpu, 5->5->3 MB, 6 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 496 @36.883s 1%: 0.14+2.3+0.027 ms clock, 0.58+1.4/2.2/0+0.10 ms cpu, 5->6->3 MB, 6 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 497 @36.889s 1%: 0.081+2.4+0.036 ms clock, 0.32+1.4/2.3/0+0.14 ms cpu, 5->6->3 MB, 6 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 498 @36.895s 1%: 0.093+2.3+0.026 ms clock, 0.37+1.2/2.1/0+0.10 ms cpu, 6->6->3 MB, 7 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 499 @36.901s 1%: 0.083+2.2+0.022 ms clock, 0.33+1.5/2.1/0+0.090 ms cpu, 5->6->3 MB, 6 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 500 @36.906s 1%: 0.082+2.3+0.027 ms clock, 0.33+1.3/2.1/0+0.10 ms cpu, 5->6->3 MB, 6 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 501 @36.912s 1%: 0.11+2.1+0.027 ms clock, 0.46+1.6/2.0/0+0.10 ms cpu, 5->6->2 MB, 6 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 502 @36.918s 1%: 0.084+2.7+0.035 ms clock, 0.33+1.0/2.6/0+0.14 ms cpu, 5->6->3 MB, 6 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 503 @36.924s 1%: 0.17+2.0+0.021 ms clock, 0.69+1.4/1.9/0+0.087 ms cpu, 5->6->2 MB, 7 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 504 @36.930s 1%: 0.087+2.1+0.022 ms clock, 0.34+1.3/2.0/0+0.090 ms cpu, 5->6->2 MB, 6 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 505 @36.935s 1%: 0.10+2.0+0.026 ms clock, 0.40+1.6/1.7/0+0.10 ms cpu, 5->5->2 MB, 6 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 506 @36.940s 1%: 0.12+2.0+0.035 ms clock, 0.49+1.4/1.9/0+0.14 ms cpu, 5->5->2 MB, 6 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 507 @36.945s 1%: 0.080+1.5+0.025 ms clock, 0.32+3.7/0/0+0.10 ms cpu, 5->5->2 MB, 6 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 508 @36.949s 1%: 0.085+2.0+0.022 ms clock, 0.34+1.9/1.7/0+0.091 ms cpu, 4->5->2 MB, 5 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 509 @36.955s 1%: 0.089+2.4+0.025 ms clock, 0.35+1.2/1.8/0+0.10 ms cpu, 5->6->3 MB, 6 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 510 @36.961s 1%: 0.11+2.3+0.021 ms clock, 0.45+0.89/2.1/0+0.086 ms cpu, 5->6->3 MB, 6 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 511 @36.966s 1%: 0.077+3.7+0.029 ms clock, 0.31+1.1/2.1/0+0.11 ms cpu, 5->6->3 MB, 6 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 512 @36.974s 1%: 0.095+2.0+0.023 ms clock, 0.38+0.82/1.8/0+0.095 ms cpu, 6->6->2 MB, 7 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 513 @36.979s 1%: 0.094+2.1+0.026 ms clock, 0.37+1.2/2.0/0+0.10 ms cpu, 5->5->2 MB, 6 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 514 @36.984s 1%: 0.084+3.6+0.029 ms clock, 0.33+1.5/1.9/0+0.11 ms cpu, 5->6->3 MB, 6 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 515 @36.992s 1%: 0.070+2.1+0.020 ms clock, 0.28+1.7/1.9/0+0.081 ms cpu, 6->7->2 MB, 7 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 516 @36.998s 1%: 0.067+2.3+0.023 ms clock, 0.26+1.3/2.2/0+0.092 ms cpu, 5->6->3 MB, 6 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 517 @37.003s 1%: 0.076+2.4+0.023 ms clock, 0.30+1.5/2.3/0+0.093 ms cpu, 5->6->3 MB, 6 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 518 @37.009s 1%: 0.074+2.3+0.029 ms clock, 0.29+0.92/2.2/0+0.11 ms cpu, 5->6->3 MB, 6 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 519 @37.016s 1%: 0.94+3.2+0.021 ms clock, 3.7+3.4/3.7/0+0.084 ms cpu, 5->6->3 MB, 6 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 520 @37.024s 1%: 0.084+2.4+0.023 ms clock, 0.33+1.2/2.2/0+0.095 ms cpu, 5->6->3 MB, 6 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 521 @37.031s 1%: 0.12+3.0+0.035 ms clock, 0.48+3.8/1.8/0+0.14 ms cpu, 6->7->2 MB, 6 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 522 @37.038s 1%: 0.087+1.9+0.024 ms clock, 0.34+1.2/1.9/0+0.098 ms cpu, 5->5->2 MB, 6 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 523 @37.045s 1%: 0.068+1.1+0.018 ms clock, 0.27+2.0/1.0/0+0.073 ms cpu, 6->6->2 MB, 6 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 524 @37.048s 1%: 0.11+1.9+0.023 ms clock, 0.45+1.4/1.7/0+0.093 ms cpu, 4->5->2 MB, 5 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 525 @37.054s 1%: 0.13+1.5+0.024 ms clock, 0.55+1.6/1.4/0+0.099 ms cpu, 5->5->2 MB, 6 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 526 @37.059s 1%: 0.091+1.7+0.020 ms clock, 0.36+1.5/1.6/0+0.080 ms cpu, 4->5->2 MB, 5 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 527 @37.063s 1%: 0.085+1.5+0.053 ms clock, 0.34+1.7/1.4/0+0.21 ms cpu, 5->5->2 MB, 6 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 528 @37.068s 1%: 0.092+2.1+0.022 ms clock, 0.36+1.3/2.0/0+0.091 ms cpu, 4->5->2 MB, 5 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 529 @37.073s 1%: 0.074+1.7+0.021 ms clock, 0.29+1.5/1.6/0+0.087 ms cpu, 5->5->2 MB, 6 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 530 @37.077s 1%: 0.071+1.6+0.019 ms clock, 0.28+1.4/1.6/0+0.079 ms cpu, 5->5->2 MB, 6 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 531 @37.082s 1%: 0.069+3.2+0.004 ms clock, 0.27+1.6/2.0/0+0.017 ms cpu, 5->5->3 MB, 6 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 532 @37.089s 1%: 0.071+2.5+0.027 ms clock, 0.28+1.2/2.4/0+0.10 ms cpu, 5->6->3 MB, 6 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 533 @37.095s 1%: 0.084+4.1+0.015 ms clock, 0.33+2.3/3.8/0+0.061 ms cpu, 5->6->3 MB, 6 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 534 @37.104s 1%: 0.076+3.0+0.026 ms clock, 0.30+0.83/2.3/0+0.10 ms cpu, 5->6->3 MB, 6 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 535 @37.111s 1%: 0.067+2.4+0.034 ms clock, 0.27+1.1/2.1/0+0.13 ms cpu, 6->7->3 MB, 7 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 536 @37.117s 1%: 0.072+1.8+0.027 ms clock, 0.28+1.4/1.7/0+0.11 ms cpu, 5->6->2 MB, 6 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 537 @37.121s 1%: 0.092+2.0+0.023 ms clock, 0.37+1.0/1.9/0+0.092 ms cpu, 5->5->2 MB, 6 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 538 @37.127s 1%: 0.095+2.2+0.022 ms clock, 0.38+1.5/2.1/0+0.088 ms cpu, 5->6->2 MB, 6 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 539 @37.132s 1%: 0.091+1.5+0.022 ms clock, 0.36+1.6/1.5/0+0.088 ms cpu, 5->5->2 MB, 6 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 540 @37.136s 1%: 0.085+1.7+0.019 ms clock, 0.34+1.4/1.7/0+0.078 ms cpu, 5->5->2 MB, 5 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 541 @37.141s 1%: 0.13+1.3+0.021 ms clock, 0.55+1.6/1.3/0+0.087 ms cpu, 5->5->2 MB, 6 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 542 @37.146s 1%: 0.081+2.5+0.029 ms clock, 0.32+1.1/2.0/0+0.11 ms cpu, 5->5->3 MB, 5 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 543 @37.151s 1%: 0.078+1.9+0.025 ms clock, 0.31+1.2/1.8/0+0.10 ms cpu, 5->6->2 MB, 6 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 544 @37.157s 1%: 0.069+2.0+0.023 ms clock, 0.27+1.3/1.9/0+0.095 ms cpu, 5->5->2 MB, 6 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 545 @37.162s 1%: 0.088+2.0+0.017 ms clock, 0.35+1.0/1.9/0+0.071 ms cpu, 5->5->2 MB, 6 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 546 @37.167s 1%: 0.083+1.6+0.027 ms clock, 0.33+1.8/1.6/0+0.10 ms cpu, 5->5->2 MB, 6 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 547 @37.172s 1%: 0.074+2.0+0.022 ms clock, 0.29+1.2/1.9/0+0.090 ms cpu, 4->5->2 MB, 5 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 548 @37.177s 1%: 0.10+1.5+0.019 ms clock, 0.43+1.4/1.4/0+0.078 ms cpu, 5->5->2 MB, 6 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 549 @37.181s 1%: 0.072+2.2+0.017 ms clock, 0.28+1.3/2.1/0+0.071 ms cpu, 4->5->2 MB, 5 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 550 @37.186s 1%: 0.080+1.8+0.024 ms clock, 0.32+1.1/1.7/0+0.096 ms cpu, 5->5->2 MB, 6 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 551 @37.191s 1%: 0.074+1.4+0.023 ms clock, 0.29+1.5/1.3/0+0.092 ms cpu, 5->5->2 MB, 6 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 552 @37.195s 1%: 0.072+1.7+0.019 ms clock, 0.28+1.4/1.6/0+0.078 ms cpu, 4->5->2 MB, 5 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 553 @37.199s 1%: 0.12+1.4+0.020 ms clock, 0.48+1.4/1.3/0+0.081 ms cpu, 5->5->2 MB, 5 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 554 @37.203s 1%: 0.065+1.3+0.018 ms clock, 0.26+1.3/1.2/0+0.074 ms cpu, 4->5->2 MB, 5 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 555 @37.207s 1%: 0.068+1.8+0.021 ms clock, 0.27+1.1/1.8/0+0.085 ms cpu, 4->5->2 MB, 5 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 556 @37.212s 1%: 0.062+1.5+0.023 ms clock, 0.25+1.4/1.4/0+0.092 ms cpu, 5->5->2 MB, 6 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 557 @37.216s 1%: 0.087+1.6+0.018 ms clock, 0.34+1.3/1.6/0+0.074 ms cpu, 4->5->2 MB, 5 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 558 @37.221s 1%: 0.14+1.5+0.021 ms clock, 0.58+1.6/1.5/0+0.086 ms cpu, 5->5->2 MB, 5 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 559 @37.227s 1%: 0.12+1.8+0.026 ms clock, 0.50+1.0/1.7/0+0.10 ms cpu, 4->5->2 MB, 5 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 560 @37.231s 1%: 0.091+1.5+0.019 ms clock, 0.36+1.0/1.4/0+0.076 ms cpu, 5->5->2 MB, 6 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 561 @37.235s 1%: 0.089+1.5+0.019 ms clock, 0.35+1.3/1.5/0+0.077 ms cpu, 4->5->2 MB, 5 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 562 @37.239s 1%: 0.090+2.3+0.045 ms clock, 0.36+1.1/1.6/0+0.18 ms cpu, 4->5->2 MB, 5 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 563 @37.245s 1%: 0.064+1.6+0.021 ms clock, 0.25+1.4/1.5/0+0.085 ms cpu, 5->5->2 MB, 6 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 564 @37.249s 1%: 0.074+1.6+0.021 ms clock, 0.29+1.3/1.5/0+0.087 ms cpu, 4->5->2 MB, 5 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 565 @37.253s 1%: 0.10+1.5+0.025 ms clock, 0.42+1.0/1.4/0+0.10 ms cpu, 4->5->2 MB, 5 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 566 @37.258s 1%: 0.086+1.4+0.022 ms clock, 0.34+1.5/1.3/0+0.090 ms cpu, 5->5->2 MB, 5 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 567 @37.263s 1%: 0.078+1.7+0.028 ms clock, 0.31+1.0/1.6/0+0.11 ms cpu, 4->5->2 MB, 5 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 568 @37.267s 1%: 0.089+1.6+0.017 ms clock, 0.35+0.65/1.6/0+0.069 ms cpu, 5->5->2 MB, 5 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 569 @37.272s 1%: 0.075+1.6+0.026 ms clock, 0.30+1.1/1.4/0+0.10 ms cpu, 5->5->2 MB, 5 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 570 @37.276s 1%: 0.082+1.5+0.025 ms clock, 0.33+1.1/1.4/0+0.10 ms cpu, 4->5->2 MB, 5 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 571 @37.280s 1%: 0.078+1.5+0.022 ms clock, 0.31+1.1/1.4/0+0.090 ms cpu, 4->5->2 MB, 5 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 572 @37.285s 1%: 0.082+1.5+0.016 ms clock, 0.33+1.2/1.4/0+0.067 ms cpu, 4->5->2 MB, 5 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 573 @37.289s 1%: 0.072+1.6+0.023 ms clock, 0.28+1.1/1.5/0+0.095 ms cpu, 4->5->2 MB, 5 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 574 @37.293s 1%: 0.15+1.7+0.020 ms clock, 0.61+0.73/1.7/0+0.083 ms cpu, 4->5->2 MB, 5 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 575 @37.298s 1%: 0.077+1.3+0.025 ms clock, 0.31+1.3/1.2/0+0.10 ms cpu, 4->5->2 MB, 5 MB goal, 1 MB stacks, 0 MB globals, 4 P
gc 576 @37.301s 1%: 0.11+1.7+0.021 ms clock, 0.45+1.2/1.6/0+0.085 ms cpu, 4->5->2 MB, 5 MB goal, 0 MB stacks, 0 MB globals, 4 P
gc 577 @37.306s 1%: 0.10+1.8+0.023 ms clock, 0.43+0.62/1.7/0+0.093 ms cpu, 4->5->2 MB, 5 MB goal, 0 MB stacks, 0 MB globals, 4 P
gc 578 @37.311s 1%: 0.079+1.5+0.021 ms clock, 0.31+1.1/1.4/0+0.085 ms cpu, 4->5->2 MB, 5 MB goal, 0 MB stacks, 0 MB globals, 4 P
gc 579 @37.316s 1%: 0.095+1.6+0.024 ms clock, 0.38+1.1/1.5/0+0.098 ms cpu, 4->5->2 MB, 5 MB goal, 0 MB stacks, 0 MB globals, 4 P
gc 580 @37.320s 1%: 0.066+1.2+0.024 ms clock, 0.26+1.3/1.1/0+0.097 ms cpu, 4->4->2 MB, 5 MB goal, 0 MB stacks, 0 MB globals, 4 P
gc 581 @37.323s 1%: 0.062+1.7+0.023 ms clock, 0.25+1.1/1.5/0+0.094 ms cpu, 4->4->2 MB, 5 MB goal, 0 MB stacks, 0 MB globals, 4 P
gc 582 @37.328s 1%: 0.077+1.4+0.025 ms clock, 0.30+1.3/1.3/0+0.10 ms cpu, 4->5->2 MB, 5 MB goal, 0 MB stacks, 0 MB globals, 4 P
gc 583 @37.332s 1%: 0.063+1.4+0.027 ms clock, 0.25+0.85/1.3/0+0.11 ms cpu, 4->5->2 MB, 5 MB goal, 0 MB stacks, 0 MB globals, 4 P
gc 584 @37.336s 1%: 0.074+1.3+0.024 ms clock, 0.29+1.0/1.3/0+0.099 ms cpu, 4->5->2 MB, 5 MB goal, 0 MB stacks, 0 MB globals, 4 P
gc 585 @37.339s 1%: 0.071+1.6+0.027 ms clock, 0.28+0.93/1.5/0+0.10 ms cpu, 4->5->2 MB, 5 MB goal, 0 MB stacks, 0 MB globals, 4 P
gc 586 @37.344s 1%: 0.066+1.4+0.038 ms clock, 0.26+1.3/1.0/0+0.15 ms cpu, 5->5->2 MB, 5 MB goal, 0 MB stacks, 0 MB globals, 4 P
gc 587 @37.348s 1%: 0.58+3.1+0.032 ms clock, 2.3+2.3/2.7/0+0.13 ms cpu, 4->5->2 MB, 5 MB goal, 0 MB stacks, 0 MB globals, 4 P
gc 588 @37.354s 1%: 0.072+2.0+0.045 ms clock, 0.28+0.83/1.8/0+0.18 ms cpu, 5->5->2 MB, 6 MB goal, 0 MB stacks, 0 MB globals, 4 P
gc 589 @37.360s 1%: 0.080+2.3+0.030 ms clock, 0.32+0.72/1.3/0+0.12 ms cpu, 5->5->2 MB, 6 MB goal, 0 MB stacks, 0 MB globals, 4 P
gc 590 @37.365s 1%: 0.071+2.0+0.024 ms clock, 0.28+0.65/1.7/0+0.099 ms cpu, 4->5->2 MB, 5 MB goal, 0 MB stacks, 0 MB globals, 4 P
gc 591 @37.371s 1%: 0.082+1.4+0.026 ms clock, 0.33+1.1/1.3/0+0.10 ms cpu, 5->5->2 MB, 5 MB goal, 0 MB stacks, 0 MB globals, 4 P
gc 592 @37.375s 1%: 0.081+1.5+0.049 ms clock, 0.32+0.73/1.5/0+0.19 ms cpu, 4->5->2 MB, 5 MB goal, 0 MB stacks, 0 MB globals, 4 P
GC forced
```
