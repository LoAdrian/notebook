# Literal Types
Can be used in `constexpr` functions
```C++
template <typename T> //possible
struct C {
	constexpr C([T xy]){} //at least 1
	constexpr void myFn() {} //possible
	void myFn2() {} //possible
	~C() = default; //trivial
};
```
- Captures (**lambda**-types) are literal-types aswell
## Standard Literal suffixes
- `using namespace std::string_literals`
	- `s` for `std::string`
	- `i`, `il`, `if` for `std::complex`
- `using namespace std::chrono_literals`
	- `ns`, `us`, `ms`, `s`, `min`, `h` for `std::chrono::duration`
