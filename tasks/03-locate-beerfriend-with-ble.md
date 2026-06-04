# Task 03 — Locate BeerFriend with BLE

Goal:
After a BeerMe signal, help nearby BeerFriends find each other using Bluetooth Low Energy.

Possible behaviour:
- After pressing BeerMe, the phone advertises/scans using BLE for a limited time.
- Nearby BeerFriends can detect proximity.
- The app may show simple proximity hints, not exact identity.
- BLE should not be permanently active unless the user explicitly starts a BeerMe session.

Important constraints:
- BLE should not reveal stable identifiers.
- Broadcast identifiers should rotate.
- Detection should be temporary.
- The server should not be involved in local BLE discovery if avoidable.

Testing expectations:
- Test temporary BLE identifier generation.
- Test identifier rotation.
- Test that stable user identifiers are not broadcast.
- Test discovery-session expiry.
- Test that BLE logic can be reasoned about without permanent tracking.

Possible prototypes:
- Python: BLE scanner experiment on Mac, if technically possible.
- Kotlin Android: BLE advertiser/scanner prototype.
- Flutter: UI wrapper, probably with native Android/iOS BLE plugins or platform channels.
