# BeerMe

BeerMe is a privacy-preserving app for spontaneous beer meetups.

The app has three core tasks:

1. Become BeerFriends.
2. Send a BeerMe signal.
3. Locate a BeerFriend nearby using BLE.

The project deliberately separates product design from implementation experiments.

- `tasks/` describes what the app must do.
- `python/` contains prototypes, simulations, crypto experiments, and backend experiments.
- `kotlin-android/` contains Android-native prototypes.
- `flutter/` contains cross-platform Android/iOS UI experiments.
- `shared/` contains message formats and test vectors.
- `docs/` contains product, privacy, threat-model, and protocol notes.

Development should follow TDD wherever practical: write tests first, implement the smallest passing behaviour, then refactor.
