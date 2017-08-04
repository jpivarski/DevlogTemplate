---
date: 2017-08-03
category: Back from PLUR
---

   * Cleaned up the interface. It's PR-quality now.
   * Prepared [some notebooks](https://github.com/jpivarski/jupyter-talks/tree/a76c9afbc1a21ba016daa7b48a39d327cf44b778/2017-08-03-rootio-numpy) to present it at the ROOT I/O meeting.
      * This involved solving the call-back-to-ROOT-within-Numba problem, though not in a clean way, integrated into PyROOT. It was a four-line hack that I think illustrates what's going on better.
