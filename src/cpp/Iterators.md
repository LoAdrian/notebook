# Iterators
## Capabilities of categories
- `ìnput_iterator_tag`: Write results
	- `i!=j`, `i==j`, const: `*i`, `i->m`, `i++`, `++i`, `*i++`
- `output_iterator_tag`: Read once
	- non-const: `*r=o`, `r++`, `++r`, `*r++=o`
- `forward_iterator_tag`: Read/write multipass
	- input_iterator + default_constructible + (optional) output_iterator
- `bidirectional_iterator_tag`: Read/write, back-forth
	- forward_iterator + `--a`, `a--`, `*a--`
- `random_access_iterator_tag`: read/write/ indexed
	- bidirectional_iterator + `i+=n`, `i+n`, `n+i`, `i-=n`, `i-n`, `i1-i2`, `i[n]` `i1<i2`, `i1>i2`, `i1<=i2`, `i1>=i2`

## Member Types`<iterator>`
```C++
template <typename T>
struct CustomIterator {
	using iterator_category = std::<?_iterator_tag>;
	using value_type = T;
	using difference_type = ptrdiff_t;
	using pointer = T *;
	using reference = T &;
//...
```

## Input Iterator example 
```C++
struct IntIterator {
		//member types
	explicit IntIterator(int const start = 0) : value{start} {}
	bool operator ==(IntIterator const & r) const {
		return value == r.value;
	}
	bool operator !=(IntIterator const & r) const {
		return !(*this == r);
	}
	value_type operator *() {
		return value;
	}
	IntIterator & operator ++() {
		++value;
		return *this;
	}
	IntIterator operator ++(int) {
		auto old = *this;
		++(*this);
		return old;
	}
private:
	int value;
};
```

## Boost Iterator Library
- `<boost/iterator/counting_iterator.hpp>`
- `<boost/iterator/filter_iterator.hpp>`
- `<boost/iterator/transform_iterator.hpp>`

```C++
struct odd {
    bool operator()(int n) const {
        return n % 2;
    }
}
std::ostream_iterator<int> out{std::cout, ", "};
//counting
using counter = boost::counting_iterator<int>;  
std::vector<int> v(counter{0}, counter{11});
copy(v.begin(), v.end(), out); std::cout << "\n";
//filtering
copy(boost::make_filter_iterator(odd{}, v.begin(), v.end()),
	boost::make_filter_iterator(odd{}, v.end(), v.end()), out);
std::cout << "\n";
//transforming
auto square = [] (auto i) {return i * i;};
copy(boost::make_transform_iterator(v.begin(), square),
	boost::make_transform_iterator(v.end(), square), out);
```

## Boost Iterator Helper `<boost/operators.hpp>`
```C++
struct CusotmIterator : boost::<type>_iterator_helper<CustomIterator, MyValueType>
```
- `input_iterator_helper<T,V>`
- `forward_iterator_helper<T,V>`
- `bidirectional_iterator_helper<T,V>`
- `random_access_iterator_helper<T,V>`
- `output_iterator_helper<T,V>`
- Providing member-types and operators
