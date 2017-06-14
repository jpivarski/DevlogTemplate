---
date: 2017-06-14
category: ROOT I/O Week
---

   * Created Python module with one function, `fill`, that loads TBranch data into a preallocated Numpy array.
   * TBufferFile immediately tries to reallocate my given array, fills it with data that includes a header (79 bytes), and then segmentation faults because it thinks it owns the array.
   * Maintaining ownership would be a struggle and the header might be insurmountable (might have to alter the decompression-handling code, for every compression method). So maybe give up on zero-copy and do a single `memcpy`.
   * Measured the impact of `memcpy`: it's 1.4% of the GetEntriesSerialized time on warmed cache (decompression, mostly), which is 0.08 ns/byte, 11.1 GB/sec on a quiet CMSLPC machine.
