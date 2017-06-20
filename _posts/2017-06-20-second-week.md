---
date: 2017-06-20
category: Second Week
---

   * Rewrote the entire project (it's better organized now!) and it worked right away for all my test cases.

   * Getting good performance results.

<img src="data/chart.png" style="width: 100%">

   * The first four groups all use the new BulkIO --> Numpy interface, some merely viewing the data (ROOT owns the buffer) and some actually copying (Numpy owns the buffer); the difference is about 20% in the smallest bars (uncompressed data), but hard to see on the scale of everything else.

   * The next four are "traditional" ways of reading the same file: root_numpy, SetBranchAddress, TTreeReader, and TTree::Draw.

   * The four colors are uncompressed/compressed, and a calculation of p versus a calculation of E. This file has a special feature: its "px", "py", "pz" are cluster-aligned (every basket is exactly the same size, we can iterate over baskets without stashing temporary data) but its "mass" branch doesn't line up anywhere (lots of buffer slicing).

   * It also has poor compression (a ratio of 1.07), so the compression is more of an impediment to reading than anything else.

   * That's why there's a big gap between compressed/deflate-1 for the four BulkIO --> Numpy groups: BulkIO benefits the most from being able to literally copy a data block without decompression.

   * The difference between "Numpy" and "Numba" is whether we perform a calculation using Numpy's canned functions:

```python
for i in xrange(reps):
    total = 0
    for start, end, px, py, pz in iterator:
        total = numpy.sqrt(px**2 + py**2 + pz**2).sum()
```

or Numba's JIT-compilation:

```python
@numba.jit(nopython=True)
def momentumsum(px, py, pz):
    total = 0.0
    for i in range(len(px)):
        total += math.sqrt(px[i]**2 + py[i]**2 + pz[i]**2)
    return total

for i in xrange(reps):
    total = 0
    for start, end, px, py, pz in iterator:
        total = momentumsum(px, py, pz)
```

   * Numpy allocates intermediate arrays with each step and does multiple passes over the data. Numba does basically what a C routine would do (no intermediate arrays, one pass over the data). There's a difference— Numba bars are about 20% smaller for the uncompressed case.

   * Generally speaking, the concern over allocating intermediate memory or doing extra copies makes 20% differences when the data do not need to be uncompressed. Otherwise, you're dominated by decompression times and don't see it. Which is basically what we'd expect.

   * C++: [test_performance.cxx](https://github.com/jpivarski/root/blob/root-bulkapi-fastread-v2/develop_tbranchnumpy/test_performance.cxx)

   * Python: [test_performance.py](https://github.com/jpivarski/root/blob/root-bulkapi-fastread-v2/develop_tbranchnumpy/test_performance.py)
