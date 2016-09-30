# High Performance C++ Queues
High performance C++ implementation of producer/consumer queue architecture

## Fundamentals to this implementation
* the group of consumers and the group of producers do not synchronize between each other.  Contention takes place only when the queue is empty to notify the readers when the queue becomes non-empty.  Otherwise, the readers and writers operate with no contention.
* choose either single or multi-threaded producers
* choose either single or multi-threaded consumers
* memory allocations are minimized or avoided entirely: addition to the queues is done by assignment copying (especially useful for stack allocated objects and especially useful to avoid reference counting or other memory tracking).  Removals from the queue are non-copy: they simply use a reference to the entry in the queue itself, and increment a queue counter when done to free up the queue location.
* The queues are circular but resize as necessary if consumers are not keeping pace with producers, up until a max size limit upon which the producers will block as necessary
* designed to be fast
* easy to customize to enqueue any data types.  See the sample folder for an implementation using two data types that can be enqueued (SampleQueueEntry1 and 2), and some test code which starts up writer threads, a queue consumer engine with multiple readers, a single consumer class SampleQueueEntryConsumer, and adds numerous entries to the queue for consumption in testQueues.

### Platforms
* Written in portable code for all platforms
* In a couple of spots it uses g++ atomic built-ins __sync_lock_test_and_set and __sync_fetch_and_add, see [atomic built-ins documentation](https://gcc.gnu.org/onlinedocs/gcc-4.4.5/gcc/Atomic-Builtins.html)
* Developed in Eclipse Mars 2 on Linux using Eclipse CDT and compiling with g++, it can be easily imported into an Eclipse workspace
