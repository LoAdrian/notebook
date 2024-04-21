# Producing `Iterator`s
- `iter(&self) -> Iter<_, T>` Referential iterator to type
- `iter_mut(&mut self) -> IterMut<_, T>` Mutable referential iterator to type
- `into_iter(self) -> IntoIter` Converting iterator into type

# `Iterator` trait
- `next(&mut self) -> Option<Self::Item>`

# Methods consuming iterators
- `max(self) -> Option<Self::Item>`
- `min(self) -> Option<Self::Item>`
- `all<F>(&mut self, f: F) -> bool where F: FnMut(Self::Item) -> bool` consumes iterator until false value
- `any<F>(&mut self, f: F) -> bool where F: FnMut(Self::Item) -> bool` consumes iterator until true value
- `find<P>(&mut self, predicate: P) -> Option<Self::Item> where P: FnMut(&Self::Item) -> bool`
- `sum<S>(self) -> S where S: Sum<Self::Item>`
- `product<P>(self) -> P where P: Product<Self::Item>`
- `cmp<I>(self, other: I) -> Ordering` Compares elements of self with elements of other lexicographically. Returns `Equal`, `Greater`, `Less`.
    - also exists in partial form
- `cmp_by<I, F>(self, other: I, cmp: F) -> Ordering` Compares elements of self with elements of other by `cmp`. Returns `Equal`, `Greater`, `Less`.
    - also exists in partial form
- `eq<I>(self, other: I) -> bool`
- `ne<I>(self, other: I) -> bool`
- `le<I>(self, other: I) -> bool`
- `lt<I>(self, other: I) -> bool`
- `ge<I>(self, other: I) -> bool`
- `gt<I>(self, other: I) -> bool`
- `eq_by<I, F>(self, other: I, eq: F) -> bool where F: FnMut(Self::Item, I::Item) -> bool`
- `is_sorted(self) -> bool`
- `is_sorted_by(self, compare: F) -> bool`

# Methods converting iterators
- `collect<B>(self) -> B where B: FromIterator<Self::Item>`
- `collect_into<E>(self, collection: &mut E) -> &mut E where E: Extend<Self::Item>`
- `filter<P>(self, predicate: P) -> Filter<Self, P> where P: FnMut(&Self::Item) -> bool`
- `map<B, F>(self, f: F) -> Map<Self, F> where F: FnMut(Self::Item) -> B`
- `flatten(self) -> Flatten<Self> where Self::Item: IntoIterator` Returns an Iterator over T from an Iterator over Iterables over T
- `filter_map<B, F>(self, f: F) -> FilterMap<Self, F> where F: FnMut(Self::Item) -> Option<B>`