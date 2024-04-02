# Templates
## Variadics
```C++
template <typename First, typename...Types>
void printAll(First const & first, Types const &...rest) {
	std::cout << first;
	if(sizeof...(Types)) {
		std::cout << ", ";
	}
	printAll(rest...);
}
void printAll(){} //base case
//alternative (without base case)
if constexpr(sizeof...(Types)) {
	std::cout << ", ";
	printAll(rest...);
}
```
## SFINAE
``` C++
template<typename T,
	typename = std::enable_if_t<CHECK<T>::value, void>>
std::enable_if_t<CHECK<T>::value, T> FUNCTION (
std::enable_if_t<CHECK<T>::value, T> value);
```
- Only one of three enable_if_t checks need to be applied (usually)
- `CHECK` might be: `std::is_class`, `std::is_arithmetic`, `std::is_same<T1, T2>`, ...

## Deduction Guides
Example: Iterator Constructor
```C++
template <typename T>
class S {
	template <typename Iter>
	S(Iter begin, Iter end) : internal_list(begin,end) {}
};
template <typename Iter>
S(Iter begin, Iter end) ->
	S<typanem std::iterator_traits<Iter>::value_type>;
```

## Concepts
```C++
template<TYPE T>
concept bool MyConcept = my_boolean_constexpr_fn(T);

template <MyConcept T>
/*...*/
```