# Task 00 — Initial Setup

Goal:
When the app is opened for the first time, it creates the local cryptographic material needed for BeerFriend pairing, BeerMe signal sending, BeerMe signal receiving, and BLE discovery.

Initial setup should happen locally on the user's device.

During initial setup, the app creates:

* the user's BeerFriend key pair,
* the user's BeerMe / BeerOpener material needed for future BeerBottle decryption,
* any BLE discovery keys or seeds needed for temporary rotating identifiers,
* protocol version,
* message-format version,
* metadata needed for future decoding, decrypting, or verifying messages.

Important constraints:

* Do not use phone numbers to generate keys.
* Do not use usernames, emails, or other stable public identifiers to generate keys.
* Key generation must happen locally on the device.
* Private keys and BeerOpener material must remain on the device unless deliberately shared during BeerFriend pairing.
* The server should not receive the user's private keys.
* The server should not receive enough information to reconstruct the user's BeerFriend graph.
* The user should not need to create a public account identifier in v1.
* The setup process should feel simple and invisible to the user.

User experience:

* The user opens the app.
* The app silently creates the required local keys.
* The user may be shown a short explanation that BeerMe works through private local keys and deliberate BeerFriend pairing.
* The user is asked for only the permissions needed at this stage.
* Location permission should not be requested until the user sends a BeerMe signal or uses a feature that needs location.
* BLE permission should not be requested until the user starts or enables BLE discovery.
* Notification permission may be requested during setup, or just before the app first needs to notify the user.

Open questions:

* What happens if the user changes phone?
* Should keys be stored only in the secure device keystore?
* Should BeerFriend relationships be exportable?
* Should notification permission be requested during setup or later?
* What Python packages should be used for prototyping key generation and secure storage?
* What Kotlin / Android APIs should be used for secure key storage?

Testing expectations:

* Test first-launch key generation.
* Test that setup is idempotent: reopening the app does not create a new identity if one already exists.
* Test that private keys are never exported by default.
* Test that no phone number, username, email, or stable public identifier is required.
* Test that generated keys are valid and usable for later pairing and BeerBottle decryption.
* Test app behaviour if secure storage is unavailable or corrupted.
* Test migration/version handling for protocol and message-format versions.

Possible prototypes:

* Python: generate BeerFriend and BeerMe key pairs, serialize public setup information, and store private material locally.
* Kotlin Android: generate keys using Android Keystore or an appropriate cryptographic library.
* Flutter: first-launch setup flow, local secure storage integration, and permission-request timing.
