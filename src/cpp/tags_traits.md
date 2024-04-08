# Tags and Traits
```C++
//Classes with different API doing the same thing
//but with different efficiencies
struct MPCV {
    void travelThroughSpace(Galaxy destination);
};
struct MilleniumFalcon {
    void travelThroughSpace(Galaxy destination);
    void travelThroughHyperspace(Galaxy destination);
};
struct HeartOfGold {
    void travelThroughSpace(Galaxy destination);
    Galaxy travelImprobably();
};

//Tags to communicaate functionality
struct SpaceDriveTag{};
struct HyperspaceDriveTag : SpaceDriveTag{};
struct InfiniteProbabilityDriveTag : SpaceDriveTag {};

//Traits to get tags
template <typename>
struct SpaceshipTraits {
    using Drive = SpaceDriveTag;
};
template <>
struct SpaceshipTraits<MilleniumFalcon> {
    using Drive = HyperspaceDriveTag;
};
template <>
struct SpaceshipTraits<HeartOfGold> {
    using Drive = InfiniteProbabilityDriveTag;
};

//Dispatched functions
template <typename Ship>
void travelToDispatched(Galaxy destination, Ship & ship, SpaceDriveTag) {
    ship.travelThroughSpace(destination);
}
template <typename Ship>
void travelToDispatched(Galaxy destination, Ship & ship, InfiniteProbabilityDriveTag) {
    while(ship.travelImprobably() != destination);
}
//Dispatching
template <typename Ship>
void travelTo(Galaxy destination, Ship & ship) {
    typename SpaceshipTraits<Ship>::Drive drive{};
    travelToDispatched(destination, ship, drive);
}
```