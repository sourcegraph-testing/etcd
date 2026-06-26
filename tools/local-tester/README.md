# etcd local-tester

The etcd local-tester runs a fault injected cluster using local processes. It sets up an etcd cluster with unreliable network bridges on its peer and client interfaces. The cluster runs with a constant stream of `Put` requests to simulate client usage. A fault injection script periodically kills cluster members and disrupts bridge connectivity.

# Requirements

local-tester depends on `goreman` to manage its processes and `bash` to run fault injection.

# Building

local-tester needs `etcd`, `benchmark`, and `bridge` binaries. To build these binaries, run the following from the etcd repository root:

```sh
./build
pushd tools/benchmark/ && go build && popd
pushd tools/local-tester/bridge && go build && popd
```

# Running

The fault injected cluster is invoked with `goreman`:

```sh
goreman -f tools/local-tester/Procfile start
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
