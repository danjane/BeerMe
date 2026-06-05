# Task 01 — Become BeerFriends

Goal:
Two users become BeerFriends only if they both deliberately add each other.

When two users become BeerFriends, the QR exchange shares:
   * the user's public BeerFriend key,
   * the user's BeerOpener information needed for future BeerBottle decryption,
   * protocol version,
   * message-format version,
   * metadata needed for decoding, decrypting, or verifying messages.

Preferred method:
- In-person QR-code exchange.
- Each user shows a QR code containing their public pairing information.
- Friendship becomes active only after mutual confirmation.

Important constraints:
- Do not use phone numbers to generate public keys.
- Do not allow searching by phone number.
- The server should not know the friendship graph.
- The server should not be able to infer the friendship graph.
- Friendship should feel deliberate and personal.

Open questions:
- Can the exchange be forced to be in person?
- Could we use BLE to check proximity?
- Could we encode the location in the QR code?
- Could we use a series or video of QR code exchanges?
- Would a combination of these ideas be better?
- What Python packages should be used for verification of the technology?

Testing expectations:
- Test export/import of public pairing information.
- Test that malformed QR or BLE data is rejected.
- Test that duplicate BeerFriends are handled safely.
- Test that no phone number is required.

Possible prototypes:
- Python: generate key pair, encode public key as QR code, scan/import another public key.
- Kotlin Android: generate key pair on phone, display QR code, scan QR code.
- Flutter: UI flow for showing and scanning a BeerFriend QR code.
