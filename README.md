# docker-vegeta

A simple container that you can use to run
HTTP load tests through the awesome [vegeta](https://github.com/tsenart/vegeta).

```
~/projects/docker-vegeta (master ✘)✭ ᐅ docker run -ti vegeta
root@cb6740a3f8b7:/go# echo "GET https://example.com/" | vegeta attack -duration=3s -rate=5 -timeout=1m | tee results.bin | vegeta report
Requests	[total]				15
Duration	[total, attack, wait]		2.949280725s, 2.798913162s, 150.367563ms
Latencies	[mean, 50, 95, 99, max]		288.16816ms, 239.913779ms, 579.811387ms, 579.811387ms, 617.736089ms
Bytes In	[total, mean]			2441521, 162768.07
Bytes Out	[total, mean]			0, 0.00
Success		[ratio]				100.00%
Status Codes	[code:count]			200:15  
Error Set:
root@cb6740a3f8b7:/go# 
```

## Usage

First go inside the contaner:

```
docker run -ti odino/vegeta
```

then have fun with vegeta:

```
echo "GET https://example.com/" | vegeta attack -duration=3s -rate=5 -timeout=1m | tee results.bin | vegeta report
```

## Why not simply using the compiled vegeta binary from your machine?

Because this lets you do real world load tests from
a common hardware, without the need to spin up
good machines on a provider like AWS.

Vegeta is a very good tool but at the same time, on
a single HW it will hit system bottlenecks like
open files limit and so on -- not superfun at all!

Luckily you can simply virtualize some "machines"
through docker and trigger less stressing requests
from the containers themselves.

ie. on my machine I used something like:

```
echo "GET https://example.com/" | vegeta attack -duration=10m -rate=100 -timeout=1m | tee results.bin | vegeta report
```

and started seeing some funky error with file descriptors,
ulimit and so on.

You can instead launch 10 containers and simply do:

```
echo "GET https://example.com/" | vegeta attack -duration=10m -rate=10 -timeout=1m | tee results.bin | vegeta report
```

and everything will work like a charm :)

## Addons

The container comes with vim preinstalled: this
is to facilitate your life if you need to play
around with your `/etc/hosts` (ie. you wanna stress
test different machines / setups).


