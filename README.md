# Simple-Cache-Project
This project will focus on the memory system focusing on cache architecture.

Direct-mapped, set-associative, and fully associative cache organizations are supported by this program's simulation of a basic cache memory system.
It replaces fully associative and set-associative caches with LRU (Least Recently Used) caches.


COMPLIATION:
To compile the program, open a terminal or command prompt in the directory where SimpleCache.java is located and run: javac SimpleCache.java

If successful, this will produce a compiled file named: SimpleCache.class


RUN:
After compiling, run the program using: java SimpleCache


You’ll be prompted for:

Number of cache blocks
(e.g., 8)

Set associativity

1 → Direct-mapped

2 → 2-way set associative

4 → 4-way set associative

0 → Fully associative

Block address references
Enter a series of integer block addresses (e.g., 0 1 2 3 0 1 4 0 1 -1)
Use -1 to stop input and calculate the miss rate.



Representation in Cache:

Makes use of a 2D CacheBlock object array.

To keep track of usage order, each set keeps an LRU list.



Policy for Replacement:

When a set is full, LRU (Least Recently Used) is used.

Hit/Miss Identification:

Cache hit: the tag is valid and present in the set.

Cache miss: tag not found; replacement could happen.


TEST:
You can test various cache configurations:

Configuration Type	Associativity Input	Example:
Direct-Mapped	1	8 blocks → 8 sets of 1 block each
2-Way Set Associative	2	8 blocks → 4 sets of 2 blocks each
4-Way Set Associative	4	8 blocks → 2 sets of 4 blocks each
Fully Associative	0	8 blocks → 1 set of 8 blocks


Notes:
Input must be integers only.
The program stops reading addresses when you enter -1.
The final miss rate is printed at the end of execution.
