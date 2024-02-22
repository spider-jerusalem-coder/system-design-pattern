# Disk

> Explain the functioning of how data is read/ write in a Spinning hard drive

Tywo types of latency
* Seek latency
  * Moving physically the disk read head (`actuator arm`) at the correct track
* Rotational latency
  * Waiting for the disk to rate and alig with the targe sector.


 ## Sector 
 * Smallest addressable disk unit
  
### Full Sector Write
* The disk controller receives the data.
* The controller writes the data directly to the appropriate sectors on the disk.
* This process is efficient because it involves a single action: writing the data to the disk.

### Partial Sector Write
* If you're writing less than a full sector's worth of data (let's say half a sector), the process is more complex and less efficient:
* The disk controller reads the entire sector from the disk into a buffer in the drive's memory (even the part of the sector that isn't being changed).
* This is necessary because you cannot directly write to just a portion of a sector on the disk; you can only write entire sectors at a time.
* The controller then merges the new data with the existing data in the buffer so that the new data occupies the correct portion of the sector, with the unchanged data filling out the rest of the sector.
* Finally, the controller writes the entire sector back to the disk.

> This read-modify-write cycle is necessary for partial sector writes due to the way hard drives are designed to operate at the sector level. It takes more time because the sector must be read first (even though part of it isnt changing), modified, and then written back. This additional overhead can slow down disk operations, especially if there are many partial sector writes happening

### Disk vs RAM
* Sequential access on HDD may be hundreds of thousands of times faster than random access. [Source](https://queue.acm.org/detail.cfm?id=1563874)
* It may be faster to read sequentially from the disk than randomly from the memory.
* This research shows that reading fully sequentially from HDD may be faster than SSD

### BTree
* Values from a single node make a page. In the example above, each page consists of three values. A page is a set of values placed on a disk next to each other, so the database may reach the whole page at once with one sequential read.
* Postgres page size is 8KB.
* Once a page is loaded then all values in the node are already available in the memory or CPU caches.
* B+ Tree nodes only contain the keys, thereby allowing more keys per page.

### Raid
* `Raid 0`: implies that no redundancy is available



### References
* [BTree makes your queries](https://blog.allegro.tech/2023/11/how-does-btree-make-your-queries-fast.html)
* [Big data analysis](https://queue.acm.org/detail.cfm?id=1563874)
