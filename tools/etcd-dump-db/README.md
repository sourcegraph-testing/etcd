### etcd-dump-db

etcd-dump-db inspects etcd db files.

```
Usage:
  etcd-dump-db [command]

Available Commands:
  list-bucket    bucket lists all buckets.
  iterate-bucket iterate-bucket lists key-value pairs in reverse order.
  hash           hash computes the hash of db file.

Flags:
  -h, --help[=false]: help for etcd-dump-db

Use "etcd-dump-db [command] --help" for more information about a command.
```


#### list-bucket [data dir or db file path]

Lists all buckets.

```
$ etcd-dump-db list-bucket agent01/agent.etcd

alarm
auth
authRoles
authUsers
cluster
key
lease
members
members_removed
meta
```


#### hash [data dir or db file path]

Computes the hash of db file.

```
$ etcd-dump-db hash agent01/agent.etcd
db path: agent01/agent.etcd/member/snap/db
Hash: 3700260467


$ etcd-dump-db hash agent02/agent.etcd

db path: agent02/agent.etcd/member/snap/db
Hash: 3700260467


$ etcd-dump-db hash agent03/agent.etcd

db path: agent03/agent.etcd/member/snap/db
Hash: 3700260467
```


#### iterate-bucket [data dir or db file path]

Lists key-value pairs in reverse order.

```
$ etcd-dump-db iterate-bucket agent03/agent.etcd key --limit 3

key="\x00\x00\x00\x00\x005@x_\x00\x00\x00\x00\x00\x00\x00\tt", value="\n\x153640412599896088633_9"
key="\x00\x00\x00\x00\x005@x_\x00\x00\x00\x00\x00\x00\x00\bt", value="\n\x153640412599896088633_8"
key="\x00\x00\x00\x00\x005@x_\x00\x00\x00\x00\x00\x00\x00\at", value="\n\x153640412599896088633_7"
```
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
