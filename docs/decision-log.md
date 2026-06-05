# Decision log

## Friendship

- BeerFriends are mutual relationships.
- Pairing happens by scanning QR codes in person.
- No phone-number-based discovery.

## Notifications

- A normal BeerMe call reaches BeerFriends.
- A wide BeerMe call reaches BeerFriends and their BeerFriends.
- The sender's identity is not shown.
- Location is recorded only at send time.

## Privacy

- Backend should not know the friendship graph.
- Backend should not know the number of friends of a user.
- Backend should not know the number of users.
- Server should mainly act as a relay.
- Explore designs inspired by COVID proximity/contact-tracing protocols.

## Open questions

- Can recipients privately check whether a BeerMe event is intended for them without being able to identify the sender?
- How can the server rate-limit spam without knowing the friendship graph?
- Other than TTL, are there are possible solutions to the friends-of-friends wide call which don't reveal the graph structure?
