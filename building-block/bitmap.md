# Bitmap

## Example
Represent 1, 2, 89 as bit map
0 1 1 0 ........ 1 (89th index)

## Tradeoff
## Benefits
* Higher accurancy than bloomfilter
* Lower latency of < 1ms for 99p
* Allow different bit operation like union, intersection and difference

### Challenges
* With sparse data too many zeroes (hence roaring bitmap)

## Traditional bitmap
* For storing a 8 M number, space required is 8 M bits / 8 = 1 MB
* If we had just one number (ideally it would take 4 Byte)
* Difference between 1 MB and 4 B

## Roaring bitmap
* **Sparse Data (Arrays):** For user IDs that are sparsely populated (e.g., only a few thousand users logged in), roaring bitmaps use an array of integers. This array directly contains the IDs of users who logged in.
* **Dense Data (Bitmaps)**: If a large contiguous range of user IDs is present (e.g., most users between 500,000 and 600,000 made a purchase), it switches to using a traditional bitmap for just that range.

### Implementation
* The series of data is divided into chunks of size 4096 [0-4095, 4096-8191, 8192-...]
* For each chunk a threshold based on hueristic is decided.
* Sparse data in each chunk if number of values in a chunk < (4096/8) = 512
* Dense data in each chunk if number of values in a chunk > 512
* Sparse data is implemented as array
* Dense data is implemented as bitmap

#### Dense data
* To represent dense data as bitmap (4096 bits) in a 32-bit integer
* We need 4096/32 = 2^12 / 2^5 = 2^7 = 128 integers (128*4 B)

#### Example
* Total users 1M
* Users subscribing for a newsletter [2, 10, 500] => store as sparse data
* If the same is stored as bitmap it would use 1M bits = 100 KB

## Use case
* membership check
* better accuracy then bloomfilter
* need latency in the order of < 10ms
* avoid large write operation on the same db

## Real world
* **Storing Logged-in Users**: If 3,000 users logged in last week (say users with IDs 5, 22, 102, ...), the roaring bitmap for logins will store an array of these 3,000 integers.
* **Storing Users Who Made a Purchase**: If users from ID 500,000 to 600,000 made a purchase, the roaring bitmap for purchases uses a bitmap for this range.

### Performing Operations:
* **Intersection**: To find users who both logged in and made a purchase, the roaring bitmap efficiently intersects the sparse array (logins) with the dense bitmap (purchases).
* **Union**: To find all users who either logged in or made a purchase, it combines the sparse and dense structures.

### Usage
* Using redis setbit
* setbit key offset value

## References
* [Segmentation Platform](https://engineering.grab.com/streamlining-grabs-segmentation-platform)
* [Roaring bitmaps and how they work](https://vikramoberoi.com/a-primer-on-roaring-bitmaps-what-they-are-and-how-they-work/)
* [Set Bit](https://redis.io/commands/setbit/)
