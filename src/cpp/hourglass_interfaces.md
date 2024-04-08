# Hourglass Interfaces 
## Library (C + C++)
```C++
//WizardHidden.h / WizardHidden.cpp
#include <stdexcept>
#include <string>
#include <iostream>
namespace unseen {
struct Wizard {
    Wizard(std::string name) : name{name}{}
    char const * doMagic(std::string const & spell) {
        if(spell == std::string{"avada kedavra"}) {
            throw std::runtime_error{"Illegal spell"};
        }
        current_spell = spell;
        return current_spell.c_str();
    }
    char const * getName() const {
        return name.c_str();
    }
private:
    std::string name;
    std::string current_spell;
};
```

```C
//Wizard.h (C)
extern "C" {
typedef struct Wizard * wizard;
typedef struct Wizard const * cwizard;
typedef struct Error * error;

char const *error_message(error error);
void error_dispose(error error);

wizard createWizard(char const *name, error *out_error);
void disposeWizard(wizard toDispose);

char const *doMagic(wizard w, char const *spell, error *out_error);
char const *wizardName(cwizard w);

```

```C++
//Wizard.cpp
#include <cassert>
#include <stdexcept>
#include "Wizard.h"
#include "WizardHidden.h"
#include <iostream>
extern "C" {
struct Error {
    std::string message;
};
const char * error_message(error error) {
    return error->message.c_str();
}
void error_dispose(error toDispose) {
    delete toDispose;
}
}

template <typename Fn>
bool translateExceptions(error* out_error, Fn&& fn) {
    try {
        fn();
        return true;
    } catch (const std::exception& e) {
        *out_error = new Error{e.what()};
        return false;
    } catch(...) {
        *out_error = new Error {"Unknown internal error"};
        return false;
    }
}

extern "C" {
struct Wizard {
    Wizard(char const *name) :wizard{name} {}
    unseen::Wizard wizard;
};
wizard createWizard(const char* name, error *out_error) {
   wizard result = nullptr;
   translateExceptions(out_error, [&] {
       result = new Wizard{name};
   }); 
   return result;
}
void disposeWizard(wizard const toDispose) {
    delete toDispose;
}
char const* doMagic(wizard w, char const * spell, error *out_error) {
    char const * result = "asdf";
    translateExceptions(out_error, [&] {
        if(!w) throw std::logic_error{"null wizard"};
        result = w->wizard.doMagic(spell);
    });
    return result;
}
char const* wizardName(cwizard const w) {
    assert(w);
    return w->wizard.getName();
}

```
## C++ Clients
```C++
#include "Wizard.h"
#include <string>
#include <stdexcept>
namespace wizard_client {
struct ErrorRAII {
    ErrorRAII(error error) : opaque {error} {}
    ~ErrorRAII() {
        if(opaque)
            error_dispose(opaque);
    }
   error opaque;
};

struct ThrowOnError {
    ThrowOnError() = default;
    ~ThrowOnError() noexcept(false) {
        if(err.opaque) {
            throw std::runtime_error{error_message(err.opaque)};
        }
    }
    operator error*() {
        return &err.opaque;
    }
private:
    ErrorRAII err{nullptr};
};

struct Wizard {
    Wizard(std::string const &who) : wiz{createWizard(who.c_str(), ThrowOnError {})} {}
    ~Wizard() {
        disposeWizard(wiz);
    }
    std::string doMagic(std::string const &spell) {
        return ::doMagic(wiz, spell.c_str(), ThrowOnError{});
    }
    char const * getName() const {
        return wizardName(wiz);
    }
private:
    Wizard(Wizard const &) = delete;
    Wizard& operator=(Wizard const & ) = delete;
    wizard wiz;
};
}
```

## Java Clients
```Java
public interface WizardLib extends Library {
	WizardLib INSTANCE = (WizardLib) Native.load("wizard", WizardLib.class);
	public static class Wizard extends Pointer {
		Wizard(String name) {
			super(Pointer.nativeValue(INSTANCE.createWizard(name)));
		}
		void dispose() {
			INSTANCE.disposeWizard(this);
		}
		String doMagic(String spell) {
			return doMagic(this, spell);
		}
	}
	Pointer createWizard(String name);
	void disposeWizard(Wizard instance);
	String doMagic(Wizard wiz, String spell);
}
```
- this example does not care about errors!

### For non-pointer data structures (non opaque / by value)
```C
extern "C" {
	struct Point {
		int x; int y;
	}
	void printPoint(Point point);
}
```
```Java
//public interface ...
//CPLLib INSTANCE ...
public static class Point extends Structure implements Structure.ByValue {
	public int x,y;
	Point(int x, int y) {
		this.x = x; this.y = y;
	}
	@Override
	protected List<String> getFieldOrder() {
		return List.of("x""y");
	}
	void printPoint(Point point);
}
```