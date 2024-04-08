# Futures & Promises `<future>`
```C++
using namespace std::chrono_literals;
std::promise<int> promise{};
auto result = promise.get_future();
auto thread = std::thread{[&] {
	std::this_thread::sleep_for(2s);
	promise.set_value(42);
	if(1 == 2) {
		promise.set_exception(<exception_ptr>);
	}
}};
std::this_thread::sleep_for(1s);
std::cout << "The answer: " << result.get() << "\n";
thread.join();
```

## std::future
- `wait()` Block until result is available
- `wait_for(<timeout>)`
- `wait_until(<timepoint>)`
- `get()` Block until result is available and return it
- Dtor waits for results to be available

## std::async `<future>`
```C++
auto the_answer = std::async(std::launch::async | std::launch::deferred,
	[] {
   return 42;
});
std::cout << "The answer: " << the_answer.get() << "\n"
```
- std::launch argument (optional)
	- `std::launch::async` (start new thread)
	- `std::launch::deferred` run when .get() is called on the future returned (`the_answer`)
	- default: `std::launch::async | std::launch:deffered`
