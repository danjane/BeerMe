# Privacy principles

- Collect the minimum data needed.
- Avoid permanent location tracking.
- Avoid phone-number-based discovery.
- Prefer in-person QR pairing.
- Do not reveal stable BLE identifiers.
- Prefer temporary, rotating identifiers.

The backend should not be able to:
   * identify the sender,
   * identify recipients,
   * read the location inside a BeerBottle,
   * infer the friendship graph,
   * infer the number of BeerFriends a user has,
   * permanently track user location,
   * link multiple BeerMe signals to the same stable identity unless unavoidable and documented.
