# Task 04 — Receive BeerMe Signal

Goal:
The app checks for BeerMe signals and displays only those that the user is allowed to open as a BeerFriend or friend-of-friend receiver.

A received BeerMe signal should only become visible to the user if the app can successfully open the BeerBottle using locally held BeerFriend / BeerOpener material.

Receiving a BeerMe signal involves:

* fetching candidate encrypted BeerMe signals,
* attempting to identify which signals can be opened locally,
* opening the BeerBottle locally on the device,
* verifying protocol version and message-format version,
* decrypting the location or meeting-point information,
* deciding whether the signal should be displayed,
* optionally participating in TTL-style forwarding for Wide signals.

Important constraints:

* The backend must not decide who is a BeerFriend.
* The backend must not know which users successfully opened a BeerBottle.
* The backend must not learn the sender's location.
* The backend must not learn the receiver's BeerFriend list.
* The app UI must not reveal the sender's identity.
* Non-BeerFriends may see or download encrypted candidate signals, but must not be able to read the location or event details.
* The app must not display new BeerMe signals during DrinkBeer mode.
* Receiving a BeerMe signal does not automatically put the receiver into DrinkBeer mode.
* The receiver should be able to ignore the signal.

User experience:

* The user receives a notification or sees an in-app alert.
* The app does not say who sent the signal.
* The app may say something like: “Someone nearby wants a beer.”
* The app displays the decrypted location or meeting point only after successful BeerBottle opening.
* The user may choose to ignore the signal, go to the location, or send their own BeerMe signal if they are not in DrinkBeer mode.

Wide signal behaviour:

* A Wide signal may be intended for BeerFriends and BeerFriends-of-BeerFriends.
* Wide signal delivery should use a TTL-style forwarding model, initially TTL=2.
* Any forwarding prototype must document what metadata the backend can observe.
* Any forwarding prototype must document whether forwarding leaks friendship relationships.
* The receiver should not post one separate message per BeerFriend if that reveals valency or graph structure.
* Forwarding must not reveal the receiver's BeerFriend list to the backend.

Open questions:

* How does the app efficiently find candidate BeerMe signals without revealing the friendship graph?
* Should all users download a shared pool of recent encrypted signals?
* Should candidate signals be bucketed geographically, temporally, or by anonymous cryptographic tags?
* How much metadata leakage is acceptable for v1?
* Should Wide signal forwarding be automatic or require user/device policy checks?
* How should the app avoid replayed or expired BeerMe signals?
* Should the receiver be able to see the signal age?
* What Python packages should be used to prototype BeerBottle detection and decryption?
* What Kotlin / Android APIs should be used for background fetching and notification handling?

Testing expectations:

* Test that a valid BeerFriend can open a BeerBottle.
* Test that a non-BeerFriend cannot open a BeerBottle.
* Test that malformed signals are rejected safely.
* Test that expired signals are ignored.
* Test that replayed signals are ignored or handled safely.
* Test that the sender's identity is not displayed.
* Test that the backend does not need to know which user opened the signal.
* Test that receiving is disabled during DrinkBeer mode.
* Test that receiving a signal does not automatically trigger DrinkBeer mode.
* Test Wide signal TTL decrementing and stopping at TTL=0.
* Test that forwarding does not reveal the receiver's full BeerFriend list.

Possible prototypes:

* Python: create encrypted BeerBottles, publish candidate messages to a local test pool, and test which local BeerOpeners can decrypt them.
* Python: simulate Wide signal TTL forwarding and document graph-leakage risks.
* Kotlin Android: background fetch of candidate encrypted signals, local BeerBottle opening, and notification display.
* Flutter: UI flow for anonymous received BeerMe signal, including ignore/go/respond choices.
