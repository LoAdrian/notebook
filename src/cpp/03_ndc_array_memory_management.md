# Memory Management: NDC Arrays `<memory>`
```C++
struct Resource {
    Resource(int x) : x {x} {}
    int x;
};
Resource & elementAt(std::byte * memory, size_t index) {
    return reinterpret_cast<Resource *>(memory)[index];
}
int main() {
    auto memory = std::make_unique<std::byte[]>(sizeof(Resource) * 5);
    for(int i = 0; i < 5; i++) {
        new (&elementAt(memory.get(),i)) Resource{i};
    }
    for(int i = 0; i < 5; i++) {
        std::cout << elementAt(memory.get(), i).x;
    }
    for(int i = 0; i < 5; i++) {
        std::destroy_at(&elementAt(memory.get(), i));
    }
} //memory destroys itself
```