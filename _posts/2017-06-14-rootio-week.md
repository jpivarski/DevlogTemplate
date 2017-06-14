---
date: 2017-06-14
category: ROOT I/O Week
---

   * Created Python module with one function, `fill`, that loads TBranch data into a preallocated Numpy array.
   
   * TBufferFile immediately tries to reallocate my given array, fills it with data that includes a header (79 bytes), and then segmentation faults because it thinks it owns the array.
   
   * Maintaining ownership would be a struggle and the header might be insurmountable (might have to alter the decompression-handling code, for every compression method). So maybe give up on zero-copy and do a single `memcpy`.
   
   * Measured the impact of `memcpy`: it's 1.4% of the `GetEntriesSerialized` time on warmed cache (decompression, mostly), which is 0.08 ns/byte, 11.1 GB/sec on a quiet CMSLPC machine.
   
   * Re-measured on an uncompressed file. The `memcpy` is still 0.08 ns/byte, 11.1 GB/sec, but now that adds up to 41% of the total because the `GetEntriesSerialized` is 44.7 times faster (it doesn't have to decompress). In fact, if my `memcpy` is 41% of the total, it's highly likely that a `GetEntriesSerialized` for uncompressed data is just a memory copy, too.

   * Raw data in [memcpy_timing.txt](data/memcpy_timing.txt).

   * Merged with Brian's API changes and recompiled.

```bash
git checkout root-bulkapi-fastread-v2
git pull https://github.com/bbockelm/root.git root-bulkapi-fastread-v2
git push origin root-bulkapi-fastread-v2
```

   * My sample code compiles again.

   * Wrote new interfaces on the Google doc ("book" icon at the top of this page).
