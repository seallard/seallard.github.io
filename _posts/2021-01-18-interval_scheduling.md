---
layout: post
title: Interval scheduling
subtitle: Understanding an algorithm for solving the interval scheduling maximization problem with multiple resources
tags: [algorithm]
comments: false
---
>The interval scheduling maximization problem (ISMP) is to find a largest compatible set - a set of non-overlapping intervals of maximum size. The goal here is to execute as many tasks as possible.
>[[wiki](https://en.wikipedia.org/wiki/Interval_scheduling)]

In this post, a variation of the ISMP is explored in which k resources are available. For example, the set of intervals might be computational tasks which have to run during specific intervals but can be executed on any of k available processing units.

## The greedy approach
I won't present a formal proof. But lets think of the problem if there are n tasks and one resource. It is certain that the task which ends first is part of an optimal solution. So, it is selected. Now a set of n-1 tasks remain and the same argument holds, with the addition that the selected task can't overlap with any previously selected task. So the task with the earliest finish time that is compatible is selected, and so on.

It becomes a bit more tricky if each task can be assigned to one of many available resources. To find an optimal solution, it is necessary to make a second greedy choice beyond picking the earliest finishing compatible task. Each selected task has to be assigned to the worst compatible resource, meaning the resource with the latest finishing task. The intuition is that you keep the "best" resources unoccupied for as long as possible and minimise wasted resource time. That way, tasks which finish later will be easier to assign to a resource.

```python
def scheduling(available_resources, jobs):
    """
    Find the size of the largest set of mutually compatible jobs scheduled
    across the available resources.
    """
    sort_by_earliest_finish_time(jobs)
    resources = [[] for x in range(available_resources)]

    for job in jobs:
        assign_to_latest_compatible_resource(job, resources)

    return sum([len(scheduling) for scheduling in resources])
```

## A coloring algorithm
ISMP with k resources can be thought of as finding the maximal coloring of n intervals with k colors where intersecting intervals must recieve distinct colors. An O(k+n) algorithm of that type is described in a paper called *[On the k-coloring of intervals](https://www.martincarlisle.com/publications/kcoloring.pdf)*. The algorithm indirectly constructs an interval graph, a graph in which nodes are intervals and edges represent intersections between the intervals. The interval graph that is constructed is always k-colorable. It can always be colored with k colors such that no adjacent nodes have the same color, meaning that a k+1 clique of nodes never is allowed to form.


### Resources
- [Slides by Kevin Wayne](https://www.cs.princeton.edu/~wayne/kleinberg-tardos/pdf/04GreedyAlgorithmsI.pdf)
- [On the k-coloring of intervals](https://www.martincarlisle.com/publications/kcoloring.pdf)
- [Interval scheduling wiki](https://en.wikipedia.org/wiki/Interval_scheduling)
- [A better blog post on interval scheduling](https://stumash.github.io/Algorithm_Notes/greedy/intervals/intervals.html)