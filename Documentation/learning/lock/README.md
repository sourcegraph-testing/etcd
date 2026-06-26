# What is this?
This directory provides an executable example of the scenarios described in [the article by Martin Kleppmann][fencing].

Generally speaking, a lease-based lock service cannot provide mutual exclusion to processes. This is because such a lease mechanism depends on the physical clock of both the lock service and client processes. Many factors (e.g. stop-the-world GC pause of a language runtime) can cause false expiration of a granted lease as depicted in the below figure: ![unsafe lock][unsafe-lock]

As discussed in [notes on the usage of lock and lease][why.md], such a problem can be solved with a technique called version number validation or fencing tokens. With this technique a shared resource (storage in the figures) needs to validate requests from clients based on their tokens like this: ![fencing tokens][fencing-tokens]

This directory contains two programs: `client` and `storage`. With `etcd`, you can reproduce the expired lease problem of distributed locking and a simple example solution of the validation technique which can avoid incorrect access from a client with an expired lease.

`storage` works as a very simple key value in-memory store which is accessible through HTTP and a custom JSON protocol. `client` works as client processes which tries to write a key/value to `storage` with coordination of etcd locking.

## How to build

For building `client` and `storage`, just execute `go build` in each directory.

## How to try

At first you need to start an etcd cluster, which works as lock service in the figures. On top of the etcd source directory, execute commands like below:
```
$ ./build      # build etcd
$ goreman start
```

Then run `storage` command in `storage` directory:
```
$ ./storage
```

Now client processes ("Client 1" and "Client 2" in the figures) can be started. At first, execute below command for starting a client process which corresponds to "Client 1":
```
$ GODEBUG=gcstoptheworld=2 ./client 1
```
It will show an output like this:
```
client 1 starts
creted etcd client
acquired lock, version: 1029195466614598192
took 6.771998255s for allocation, took 36.217205ms for GC
emulated stop the world GC, make sure the /lock/* key disappeared and hit any key after executing client 2:
```
The process causes stop the world GC pause for making lease expiration intentionally and waits a keyboard input. Now another client process can be started like this:
```
$ ./client 2
client 2 starts
creted etcd client
acquired lock, version: 4703569812595502727
this is client 2, continuing
```
If things go well the second client process invoked as `./client 2` finishes soon. It successfully writes a key to `storage` process. After checking this, please hit any key for `./client 1` and resume the process. It will show an output like below:
```
resuming client 1
failed to write to storage: error: given version (4703569812595502721) differ from the existing version (4703569812595502727)
```

### Notes on the parameters related to stop the world GC pause
`client` program includes two constant values: `nrGarbageObjects` and `sessionTTL`. These parameters are configured for causing lease expiration with stop the world GC pause of go runtime. They heavily rely on resources of a machine for executing the example. If lease expiration doesn't happen on your machine, update these parameters and try again.

[why.md]: ../why.md#Notes-on-the-usage-of-lock-and-lease
[fencing]: https://martin.kleppmann.com/2016/02/08/how-to-do-distributed-locking.html
[unsafe-lock]: https://martin.kleppmann.com/2016/02/unsafe-lock.png
[fencing-tokens]: https://martin.kleppmann.com/2016/02/fencing-tokens.png
Hello World 2

---

## 🚲 Bicycle Advocacy 🚲

We believe that cycling is one of the most powerful tools we have for building healthier communities, reducing carbon emissions, and reclaiming our streets.

### Why Bicycles Matter

- **Climate action** — A bicycle produces zero direct emissions. Replacing even one car trip per day with a bike ride can save hundreds of kilograms of CO₂ per year.
- **Healthier cities** — Cycling reduces traffic congestion, improves air quality, and lowers noise pollution, making urban spaces more liveable for everyone.
- **Personal health** — Regular cycling improves cardiovascular fitness, mental well-being, and longevity.
- **Equity & access** — Bicycles are affordable, require no fuel, and open up mobility to people who cannot drive or afford a car.
- **Economic benefits** — Cyclists spend more at local businesses per kilometre travelled than motorists.

### How You Can Help

1. **Ride when you can.** Every trip made by bike instead of car counts.
2. **Advocate locally.** Attend city council meetings and support protected bike lane proposals.
3. **Be visible.** Use lights, wear bright clothing, and ride predictably.
4. **Welcome newcomers.** Offer to do a group ride with someone new to cycling.
5. **Support bike-friendly businesses.** Patronise shops that provide bike parking and commuter facilities.

### Resources

- [PeopleForBikes](https://www.peopleforbikes.org)
- [European Cyclists Federation](https://ecf.com)
- [Cycling UK](https://www.cyclinguk.org)
- [Bike to Work Day](https://bikeleague.org/bikemonth/)

> *"Life is like riding a bicycle. To keep your balance, you must keep moving."* — Albert Einstein
