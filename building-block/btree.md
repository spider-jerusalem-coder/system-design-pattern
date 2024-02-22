# BTree

## Why do BTree needs vaccuming
Fragmentation due to 
* **Big value updates** - updating a value into a larger value might overwrite data of the next node, so the tree relocates the item to a different location, leaving a "hole" in the original page.
* **Small value updates** - updating a value to a smaller value leaves a "hole" at the end.
* **Deletes** - deletion causes a "hole" right where the deleted value used to reside.

> The process that takes care of space reclamation and page rewrites can sometimes be called vacuum, compaction, page defragmentation, and maintenance. It is usually done in the background to not interfere and cause latency spikes to user requests.

## Real world
* PostgresQL
