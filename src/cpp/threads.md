# Threads `<thread>`
```C++
std::thread t{[]() {
using namespace std::chrono_literals;
std::this_thread::sleep_for(500ms);
	//std::this_thread::sleep_until(...);
	//std::this_thread::yield();
	std::cout << std::this_thread::get_id() << "\n";
}};
t.join();
//t.detach()
```

## Synchronization `<[shared_]mutex>` `<condition_variable>`
```C++
#include <mutex>
#include <queue>
#include <condition_variable>
template <typename T, typename MUTEX = std::mutex>
struct threadsafe_queue {
    using guard = std::lock_guard<MUTEX>;
    using lock = std::unique_lock<MUTEX>;
    void push(T const & t) {
        guard lk{mx};
        q.push(t);
        notEmpty.notify_one(); //or _all();
    }
    T pop() {
        lock lk{mx};
        notEmpty.wait(lk, [this] {
            return !q.empty();
        });
        T value = q.front();
        q.pop();
        return value;
    }
private:
    mutable MUTEX mx{};
    std::condition_variable notEmpty{};
    std::queue<T> q{};
};
```
### Mutex Types
- `std::[recursive_]mutex`
	- `lock()` blocking
	- `try_lock()` non-blocking
	- `unlock()` non-blocking
- `std::[recursive_]timed_mutex`
	- `try_lock_for(<dur>)`
	- `try_lock_until(<time>)`
- `std::shared_[timed_]mutex`
	- `[try_]lock_shared()` (readonly)
	- `try_lock_shared_[for/until](<time>)`
	- `unlock_shared()`
### Lock-Types
- `std::lock_guard`: RAII
	- locks immediately
	- unlocks when destructed
- `std::scoped_lock{mx1, mx2}`: RAII for multiple mutexes
	- locks immediately
	- unlocks when destructed
- `std::unique_lock`:
	- deffered and timed locking
	- explicit locking/unlocking
	- unlocks when destructed
- `std::shared_lock` Wrapper for shared mutexes
	- Allows explicit locking/unlocking
