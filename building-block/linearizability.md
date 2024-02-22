# Linearizability

## Linearizability vs Serializibility
### Linearizability
* Linearizability is a guarantee about single operations on single objects. 
* It provides a real-time (i.e., wall-clock) guarantee on the behavior of a set of single operations (often reads and writes) on a single object (e.g., distributed register or data item).
* Writes should appear to be instantaneous.
* Once a write completes, all later reads should return the value of that write or the value of a later write
* Synonymous to atomic consistency
* It is composable (if operations on each object in a system are linearizable, then all operations in the system are linearizable)

### Serializibility
* Serializability is a guarantee about transactions, or groups of one or more operations over one or more objects.
* It guarantees that the execution of a set of transactions over multiple items is equivalent to some serial execution (total ordering) of the transactions.
* A serializable execution also preserves correctness.
* serializability does not—by itself—impose any real-time constraints on the ordering of transactions.
* Serializability is also not composable.

## References
* [inearizability-versus-serializability](http://www.bailis.org/blog/linearizability-versus-serializability/)
