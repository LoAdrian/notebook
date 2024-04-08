# pImpl Idiom (Pointer to Implementation)
```C++
//Wizard.h
struct Wizard {
    Wizard(std::string name);
    Wizard(Wizard && other);
    Wizard & operator = (Wizard && other);
    ~Wizard(); //needed when using unqiue_ptr
    void doMagic(std::string spell);
private:
    std::unique_ptr<class WizardImpl> pImpl; //pointer to impl
};

//WizardImpl.cpp
//#include <Wizard.h>
class WizardImpl {
    std::string name;
public:
    WizardImpl(std::string name) :name{name}{}
    void doMagic(std::string spell) {std::cout << spell << std::endl;}
};
Wizard::Wizard(std::string name) : pImpl{std::make_unique<WizardImpl>(name)} {}
Wizard::Wizard(Wizard && other) = default;
Wizard & Wizard::operator = (Wizard && other) = default;
Wizard::~Wizard() = default; //needed when using unique_ptr
void Wizard::doMagic(std::string spell){
    pImpl->doMagic(spell);
}
```
How should objects be copied:
- No Copying, only moving => `std::unique_ptr<class Impl>`
	- Destructor = default
	- Movie Operation = default
- Shallow Copying => `std::shaared_ptr<class Impl>`
- Deep Copying => `std::unique_ptr<class Impl>`
	- Additional DIY copy operations (using copy ctor)
