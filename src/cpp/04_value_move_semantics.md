# Value & Move Semantics
## Swap
```C++
struct S {
	void swap(S & other) noexcept {
		using std::swap;
		swap(member1,other.member1);
		//...
	}
}
void swap(S & lhs, S & rhs) noexcept {
	lhs.swap(rhs);
}
```
## Copy Assignment
```c++
struct S {
	S & operator=(S const & s) {
		if(&s != this) {
			S copy = s;
			swap(copy);
		}
		return *this;
	}
}
```
## Move Assignment
```C++
struct S {
	S & operator=(S && s) {
		if(&s != this) {
			swap(s);
		}
		return *this;
	}
}
```
## Mandatory Elision
```C++
S create() {
	return S{};
}

S s = S{S{}}; //calls S() once
S new_sw{create()}; //calls S() once
S * sp = new S{create()}; //calls S() once
```
## Lifetime Extension
Ok:
```C++
struct Demon{};
Demon summon() {return Demon{};}
void countEyes(Demon const &) {/*...*/}
//main
summon(); //dies immediately;
countEyes(Demon{}); //lifetime extended until end of fn
Demon const & flaaghun = summon(); //lifetime extended until end of block
Demon const & vecna = summon(); //lifetime extended until end of blck
```
Undefined Behavior:
```C++
Demon const & bloodMagic() {
	Demon demon{};
	return demon;
} //demon dies here
Demon const & animate(Demon const & demon) {return demon;}
//main
Demon const & vecna = blood_magic(); //already dead
Demon const & one = animate(Demon{}); //cannot be kept alive
```