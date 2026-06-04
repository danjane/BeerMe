# BeerMe project instructions for Codex

BeerMe is a privacy-preserving app for spontaneous beer meetups.

The project is split into product tasks and technology areas.

## Product tasks

The three main BeerMe tasks are:

1. Become BeerFriends
   - Two users become BeerFriends only after mutual, deliberate pairing.
   - Preferred pairing method: in-person QR-code exchange.
   - Do not use phone numbers for discovery or key generation.

2. Send BeerMe Signal
   - The user presses one button to send a BeerMe signal.
   - Normal signal: BeerFriends.
   - Wide signal: BeerFriends and their BeerFriends.
   - The sender's identity should not be revealed.
   - The signal may include current location, captured only when the button is pressed.
   - There should be a cooldown, roughly 1 hour.

3. Locate BeerFriend with BLE
   - After a BeerMe signal, BLE may help nearby BeerFriends find each other.
   - BLE should use temporary rotating identifiers.
   - BLE should not reveal stable user identity.
   - BLE should run only during a temporary BeerMe discovery session.

## Technology areas

Use these folders:

- `python/`
  - For prototypes, simulations, cryptography experiments, backend/mailbox experiments, and test-vector generation.
  - Python code should be simple, readable, and heavily commented.

- `kotlin-android/`
  - For Android-native work.
  - Use this especially for BLE, secure local storage, QR scanning, notifications, and Android permissions.

- `flutter/`
  - For cross-platform UI experiments targeting Android and iOS.
  - Flutter may call native Kotlin/Swift code where BLE or cryptographic storage requires platform-specific behaviour.

- `shared/`
  - For message formats, protocol notes, sample keys, and test vectors shared across implementations.

## Privacy principles

- The backend should know as little as possible.
- The backend should not know the friendship graph if avoidable.
- The backend should not know how many BeerFriends a user has if avoidable.
- Sender identity should not be shown in BeerMe notifications.
- Location is shared only at the moment of pressing the BeerMe button.
- Avoid permanent tracking.
- Avoid phone-number-based discovery.
- Prefer QR-code pairing.

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

- Python prototypes should use `pytest`.
- Kotlin Android code should have unit tests for pure logic and instrumented tests only when platform behaviour requires it.
- Flutter code should use widget tests for UI behaviour and unit tests for protocol-independent logic.
- Shared protocol formats should have test vectors in `shared/test-vectors/`.
- Security-sensitive behaviour must have explicit tests, especially for:
  - QR pairing data format,
  - key generation/import/export,
  - message encoding/decoding,
  - cooldown rules,
  - temporary BLE identifiers,
  - prevention of stable identity leakage.

Do not add large untested features.

When Codex is asked to implement a task, it should first identify:
- the relevant BeerMe task,
- the relevant technology area,
- the expected tests,
- the smallest testable behaviour to implement.

## How to work

When asked to implement something:

1. First identify which product task it belongs to.
2. Then identify the correct technology area.
3. Identify the tests that should exist before writing implementation code.
4. Prefer small prototypes before large architecture changes.
5. Do not mix Python, Kotlin, and Flutter code in the same folder.
6. Keep protocol assumptions documented in `docs/` or `shared/`.
7. If changing behaviour, update the relevant file in `tasks/`.
