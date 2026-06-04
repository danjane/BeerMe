# Task 01 — Become BeerFriends

Goal:
Two users become BeerFriends only if they both deliberately add each other.

Preferred method:
- In-person QR-code exchange.
- Each user shows a QR code containing their public pairing information.
- The other user scans it.
- Friendship becomes active only after mutual confirmation.

Important constraints:
- Do not use phone numbers to generate public keys.
- Do not allow searching by phone number.
- The server should not need to know the friendship graph.
- Friendship should feel deliberate and personal.

Testing expectations:
- Test key generation.
- Test export/import of public pairing information.
- Test that malformed QR data is rejected.
- Test that duplicate BeerFriends are handled safely.
- Test that no phone number is required.

Possible prototypes:
- Python: generate key pair, encode public key as QR code, scan/import another public key.
- Kotlin Android: generate key pair on phone, display QR code, scan QR code.
- Flutter: UI flow for showing and scanning a BeerFriend QR code.
