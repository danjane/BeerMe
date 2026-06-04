# Task 02 — Send BeerMe Signal

Goal:
A user presses one button to send a BeerMe signal.

Signal types:
- Normal BeerMe: intended for direct BeerFriends.
- Wide BeerMe: intended for BeerFriends and their BeerFriends.

Signal contents:
- Approximate current location or chosen nearby place.
- Timestamp.
- Expiry time.
- No sender name.
- No permanent location tracking.

Important constraints:
- Sender identity should not be visible to recipients.
- Location is captured only when the button is pressed.
- Cooldown period, roughly 1 hour.
- The backend should learn as little as possible.
- Ideally the backend should not know who the intended recipients are.

Testing expectations:
- Test cooldown logic.
- Test signal expiry.
- Test signal encoding/decoding.
- Test that sender name is not included in the public payload.
- Test that stale signals are ignored.

Possible prototypes:
- Python: simulate encrypted BeerMe messages for a list of friends.
- Python: server/mailbox prototype.
- Kotlin Android: send location-triggered message.
- Flutter: one-button UI and cooldown display.
