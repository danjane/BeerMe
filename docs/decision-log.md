# Decision log

## Friendship

- BeerFriends are mutual relationships.
- Pairing happens by scanning QR codes in person.
- Avoid phone-number-based discovery.

## Notifications

- A normal BeerMe call reaches BeerFriends.
- A wide BeerMe call reaches BeerFriends and their BeerFriends.
- The sender's identity is not shown.
- Location is shared only at send time.

## Privacy

- Backend should not know the friendship graph if avoidable.
- Backend should not know number of friends if avoidable.
- Server should mainly act as a mailbox or relay.
- Explore designs inspired by COVID proximity/contact-tracing protocols.

## Open questions

- How can recipients privately check whether a BeerMe event is intended for them?
- How can the server rate-limit spam without knowing the friendship graph?
- How should friends-of-friends work without revealing graph structure?
- Should the first prototype ignore friends-of-friends and implement direct BeerFriends only?
