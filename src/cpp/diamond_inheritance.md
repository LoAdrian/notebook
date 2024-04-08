# Diamond Inheritance (virtual)
```C++
struct Tip {
    Tip() { std::cout << "Tip(); \n";}
    explicit Tip(int x) {std::cout << "Tip(int x); \n";}
    ~Tip() {std::cout << "~Tip();\n";}
};
struct A : virtual Tip {
    explicit A(int x) : Tip(x)
        {std::cout << "A(int x);\n";}
    ~A() {std::cout << "~A();\n";}
};
struct B : virtual Tip {
    explicit B(int x) : Tip(x)
        {std::cout << "B(int x);\n";}
    ~B() {std::cout << "~B();\n";}
};
struct Child : A, B {
    explicit Child(int x) : A(x), B(x)
        {std::cout << "Child(int x);\n";}
    ~Child() {std::cout << "~Child();\n";}
};
int main() {
    Child c{5};
}
/* Tip();
 * A(int x);
 * B(int x);
 * Child(int x);
 * ~Child();
 * ~B();
 * ~A();
 * ~Tip();
*
```
- Note that the default constructor of Tip is called by the Child struct
