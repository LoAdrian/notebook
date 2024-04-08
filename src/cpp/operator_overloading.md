# Operator Overloads
Overloadable Operators:
$$
\begin{matrix}
+ & - & * & / & \% & \hat{} & \& & | & \tilde{} \\
! & , & = & < & > & <= & >= & ++ & -- & \\
<< & >> & == & != & \&\& & || &+= & -= & /= \\
\%= & \hat{}= & \&= & |= & *= & <<= & >>= & [] & () \\
-> & ->* & \text{new} & \text{new}[] & \text{delete} & \text{delete}[]
\end{matrix}
$$
## Member-Operator
```C++
template <typename T>
T operator * (T const & factor) const {
	return factor * this->property;
}
```
## Friend-Operator
```C++
template <typename T>
friend T operator * (T const & facotr,
					 MyClass const & my_instance) {
	return factor * my_instance.property;
}
```
## Cast-Operator
```C++
//explicit for explicit cast only
[explicit] constexpr operator T () const {
	return this->property_in_T;
}
```
## User-defined literal suffix operator
```C++
constexpr inline TYPE operator"" _suffix(T value);
constexpr inline TYPE operator"" _suffix(char const *s, std::size_t len);
constexpr inline TYPE operator"" _suffix(char const *s);
// raw (last) takes int and float not char const *
template <char ...Digits>
constexpr inline TYPE operator"" _suffix(){
	return helper<Digits...>;
}
// raw variadic
```
T might be: `unsigned long long`, `long double`