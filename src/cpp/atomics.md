# Atomics `<atomic>`
## std::atomic_flag{bool}
```C++
std::atomic_flag flag{true};
std::thread t{[&flag] {
	using namespace std::chrono_literals;
	std::this_thread::sleep_for(5s);
	std::cout << "flag about to be cleared\n";
	flag.clear();
}};
while(flag.test_and_set());
std::cout << "flag cleared\n";
t.join();
```

## std::atomic\<T\>{T}
- `void store(T)`
- `T load()`
- `T exchaange(T)`
- `bool compare_exchange_weak(T & expexted, T desired)`
	- compare expected with current
	- replace if equal
	- may spuriously fail
- `bool compare_exchange_strong(T & expexted, T desired)`
	- no spurious failures (slower)

## Memory order for atomics
- All atomics-operations take additional argument for memory-order
- `std::memory_order_seq_cst` 
	- Intuitive default behavior
- `std::memory_order_acquire`
	- No reads/writes in current thread can be reordered before this load
	- All writes in other threads that release this atomic, are visible in the current thread
- `std::memory_order_release`
	- No reads/writes in current thread can be reordered after this store
	- All writes in the current thread are visible in other threads that acquire this atomic
- `std::memory_order_acq_rel`
	- Work on the latest value
	- No reorderings in the current thread
- `std::memory_order_relaxed`
	- No sequencing promises
- `std::memory_order_consume`
	- Bad
