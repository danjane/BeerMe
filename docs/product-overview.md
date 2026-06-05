# Product overview

BeerMe is a one-button app for spontaneous beer meetups.

The main BeerMe workflows are:

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

5. Receive BeerMe Signal
   * The app periodically checks for new BeerBottles on the server.
   * A received BeerMe signal is only displayed if the app can successfully open the BeerBottle using a locally held BeerOpener.
   * Non-BeerFriends may be able to download or see the existence of an encrypted BeerMe signal in a BeerBottle, but must not be able to read the sender's location.
   * The sender's identity should not be revealed in the app UI.
   * The app will display “BeerMe” together with the decrypted location.
   * The user may choose to ignore the signal or go to the location.
   * To locate a friend the user must switch to DrinkBeer mode by themselves sending a BeerMe signal.
   * If the signal is a wide signal the app will automatically republish it according to the TTL design.
