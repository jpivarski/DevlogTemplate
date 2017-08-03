---
date: 2017-08-02
category: Back from PLUR
---

   * Found `TBranch::DropBaskets()` to be a more stable way to reset a reader than `TTree::Reset()`.
   * Got counters to be readable [by removing the guards](https://github.com/jpivarski/root/commit/4b18c22aab94693923e530b81ad636b62b096042) on `TLeaf::DeserializeType` in Brian's code.
