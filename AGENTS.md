# BeerMe project instructions for Codex

BeerMe is a privacy-preserving app for spontaneous beer meetups.



## Product tasks

The four main BeerMe tasks are:

1. Become BeerFriends

   * Two users become BeerFriends only after mutual, deliberate pairing.
   * Preferred pairing method: QR-code exchange in person.
   * Open design question: should in-person pairing be strengthened using BLE proximity, coarse GPS, short-lived QR codes, or a combination?
   * Do not use phone numbers for discovery or key generation.
   * Do not use usernames, emails, or other stable public identifiers for discovery.
   * A user cannot silently remove a BeerFriend in v1.
   * Future revocation/blocking is out of scope for now.

2. Send BeerMe Signal

   * The user presses one button to send a BeerMe signal.
   * Normal signal: BeerFriends notified.
   * Wide signal: BeerFriends and their BeerFriends.
   * Avoid implementations where Alice posts one separate message per BeerFriend.
   * A Wide signal should use a TTL-style forwarding model, initially TTL=2, so that the sender does not post one separate message per BeerFriend.
   * The TTL design is not a complete protocol. Any TTL forwarding prototype must document what metadata the backend can observe and whether forwarding leaks friendship relationships.
   * The sender's identity should not be revealed in the app UI.
   * The signal includes the location captured when the button was pressed.
   * The location must be encrypted in a BeerBottle so that the backend and non-BeerFriends cannot read it.

3. DrinkBeer mode

   * After sending a BeerMe signal, the app enters DrinkBeer mode for 1 hour.
   * During this hour, the user cannot send another BeerMe signal.
   * During this hour, the app does not receive or display new BeerMe signals.
   * During this hour, BLE discovery may run if the user has granted permission.


4. Locate BeerFriend with BLE

   * BLE technology can help nearby BeerFriends find each other during a temporary discovery session.
   * BLE should use temporary rotating identifiers.
   * BLE should not reveal stable user identity.
   * BLE detection should be able to run during the DrinkBeer hour.

## Key and message model

Use the following project vocabulary consistently.

### BeerFriend key pair

On first app launch, each device creates a BeerFriend key pair.
   * Used for becoming BeerFriends and for BLE-related friendship checks.
   * The public BeerFriend key may be shared during QR pairing.
   * The private BeerFriend key never leaves the device.

### BeerBottle

A BeerBottle is an encrypted BeerMe signal packet.
   * It is posted to the backend.
   * It may be distributed to many users.
   * It contains encrypted private event information, including the location captured when the BeerMe button was pressed.
   * The backend may see the BeerBottle, but must not be able to open it.
   * Non-BeerFriends may see the BeerBottle, but must not be able to open it.

### BeerOpener

A BeerOpener is the secret or key material that lets a BeerFriend open/decrypt a BeerBottle.
   * BeerOpener material is exchanged during BeerFriend pairing.
   * Only BeerFriends should have enough BeerOpener material to open each other's future BeerBottles.
   * Treat BeerOpener material as sensitive and store it securely.

### QR pairing exchange

When two users become BeerFriends, the QR exchange shares:
   * the user's public BeerFriend key,
   * the user's BeerOpener information needed for future BeerBottle decryption,
   * protocol version,
   * message-format version,
   * metadata needed for decoding, decrypting, or verifying messages.

Do not use phone numbers, usernames, emails, or stable public identifiers for discovery.

## BeerMe signal flow

When Alice sends a BeerMe signal:
1. Alice creates a private BeerMe payload containing the event location and other event information.
2. Alice seals this payload inside an encrypted BeerBottle.
3. Alice posts the BeerBottle to the backend.
4. The backend can distribute the BeerBottle but cannot open it or read the location.
5. Alice's BeerFriends use their BeerOpener material to try to open the BeerBottle.
6. Other BeerMe users cannot open the BeerBottle or read the location.
7. The app notification should not reveal Alice's identity in the UI.

Known v1 limitation:
   * If each BeerFriend has sender-specific BeerOpener material, the receiving device may be able to infer which BeerFriend sent the BeerBottle.
   * The app UI should still avoid showing the sender's identity.
   * Any stronger sender-anonymity design must be documented separately before implementation.

## Backend assumptions

The backend is a bulletin board for BeerMe signal packets.

The backend should be able to:
   * receive BeerBottle packets,
   * rate-limit or reject obvious spam where possible,
   * distribute packets to clients.

The backend should not be able to:
   * identify the sender,
   * identify recipients,
   * read the location inside a BeerBottle,
   * infer the friendship graph,
   * infer the number of BeerFriends a user has,
   * permanently track user location,
   * link multiple BeerMe signals to the same stable identity unless unavoidable and documented.

## Development method: TDD

Use test-driven development wherever practical.

For any new feature or protocol behaviour:

1. Write or update tests first.
2. Run the relevant test suite and confirm that the new test fails for the expected reason.
3. Implement the smallest amount of code needed to pass the test.
4. Refactor only after tests pass.
5. Keep tests close to the behaviour being developed.
6. Maintain a strong, dedicated, and pertinent test suite.

Testing expectations:

* Python prototypes should use `pytest`.
* Kotlin Android code should have unit tests for pure logic and instrumented tests only when platform behaviour requires it.
* Flutter code should use widget tests for UI behaviour and unit tests for protocol-independent logic.
* Shared protocol formats should have test vectors in `shared/test-vectors/`.
* Security-sensitive behaviour must have explicit tests, especially for:


  * QR pairing data format,
  * key generation/import/export,
  * BeerBottle creation,
  * BeerBottle opening/decryption,
  * message encoding/decoding,
  * cooldown rules,
  * temporary BLE identifiers,
  * prevention of stable identity leakage.

Do not add large untested features.

## Security and privacy discipline

Codex must not invent new protocol assumptions silently.

When implementing privacy-sensitive features, Codex should:

1. State the assumption being used.
2. Add or update tests for the assumption.
3. Add test vectors where messages, keys, or encodings are involved.
4. Document privacy leaks or limitations explicitly.
5. Prefer small simulations in `python/` before mobile implementation.

For protocol or security-sensitive changes, first update or create a short note in `docs/` describing:

* what the attacker can observe,
* what the backend can observe,
* what a malicious user can attempt,
* what privacy properties the design is trying to preserve,
* known limitations.

Do not implement custom cryptographic primitives.

Use established libraries and keep cryptographic code small, explicit, and tested.

S