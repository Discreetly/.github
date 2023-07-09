# Discreetly
## About Discreetly
Discreetly is an anonymous and federated chat application. Under the hood it uses a zero-knowledge protocol called RLN (Rate-Limiting Nullifier) to rate limit messages per user.
Read more about [RLN](https://rate-limiting-nullifier.github.io/rln-docs/).

## Features 

* Truly anonymous
  * It uses RLN to prove you are a member of a room, but not who you are.
  * If you don't trust the server, run your own
* Federated
  * Run your own server, and use the same frontend as everyone else, or run your own frontend too.
* Spammers are banned
  * If someone tries to send messages faster than the rate limit allowed, they are automatically banned, and there is no unban.
