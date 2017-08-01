---
date: 2017-08-01
category: Back from PLUR
---

   * Fully working flat branch reading, and also correctly reads in the counted branches, but doesn't expose counter.
   * The counter is some "destructively serialized" type that Brian doesn't handle.
   * Wrote a `SetBranchAddress`-based (old school) leaf reader, intended primarily for counters, with the thinking that counters are a minority of the data and can suffer a slower reading method. I _am_ using MakeClass mode, though.
