
## Red-Black Tree

*"Introduction to Algorithms" (Cormen et al, 3rd ed.), Chapter 13*

1. Every node is either red or black.
2. The root is black.
3. Every leaf (NIL) is black.
4. If a node is red, then both its children are black.
5. For each node, all simple paths from the node to descendant leaves contain the
same number of black nodes.

For example,

```go
import (
    "fmt"

    "go.etcd.io/etcd/v3/pkg/adt"
)

func main() {
    ivt := adt.NewIntervalTree()
    ivt.Insert(NewInt64Interval(510, 511), 0)
    ivt.Insert(NewInt64Interval(82, 83), 0)
    ivt.Insert(NewInt64Interval(830, 831), 0)
    ...
```

After inserting the values `510`, `82`, `830`, `11`, `383`, `647`, `899`, `261`, `410`, `514`, `815`, `888`, `972`, `238`, `292`, `953`.

![red-black-tree-01-insertion.png](img/red-black-tree-01-insertion.png)

Deleting the node `514` should not trigger any rebalancing:

![red-black-tree-02-delete-514.png](img/red-black-tree-02-delete-514.png)

Deleting the node `11` triggers multiple rotates for rebalancing:

![red-black-tree-03-delete-11.png](img/red-black-tree-03-delete-11.png)
![red-black-tree-04-delete-11.png](img/red-black-tree-04-delete-11.png)
![red-black-tree-05-delete-11.png](img/red-black-tree-05-delete-11.png)
![red-black-tree-06-delete-11.png](img/red-black-tree-06-delete-11.png)
![red-black-tree-07-delete-11.png](img/red-black-tree-07-delete-11.png)
![red-black-tree-08-delete-11.png](img/red-black-tree-08-delete-11.png)
![red-black-tree-09-delete-11.png](img/red-black-tree-09-delete-11.png)

Try yourself at https://www.cs.usfca.edu/~galles/visualization/RedBlack.html.
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
