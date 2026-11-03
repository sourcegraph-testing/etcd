# ./hack/patch/cherrypick.sh

Handles cherry-picks of PR(s) from etcd master to a stable etcd release branch automatically.

## Setup

Set the `UPSTREAM_REMOTE` and `FORK_REMOTE` environment variables.
`UPSTREAM_REMOTE` should be set to git remote name of `github.com/etcd-io/etcd`,
and `FORK_REMOTE` should be set to the git remote name of the forked etcd
repo (`github.com/${github-username}/etcd`). Use `git remote -v` to
look up the git remote names. If etcd has not been forked, create
one on github.com and register it locally with `git remote add ...`.


```
export UPSTREAM_REMOTE=upstream
export FORK_REMOTE=origin
export GITHUB_USER=${github-username}
```

Next, install hub from https://github.com/github/hub

## Usage

To cherry pick PR 12345 onto release-3.2 and propose is as a PR, run:

```sh
./hack/patch/cherrypick.sh ${UPSTREAM_REMOTE}/release-3.2 12345
```

To cherry pick 12345 then 56789 and propose them togther as a single PR, run:

```
./hack/patch/cherrypick.sh ${UPSTREAM_REMOTE}/release-3.2 12345 56789
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
