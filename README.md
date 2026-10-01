# Messaging System mocking the architecture behind WhatsApp
* C++
* SQLite
* Asio
---
## System Design
A central server to handle the first registration of the clients, 
its job is to "give" the socket connection to two clients so that they can chat without the need of the server handling all of the requests.
If a client disconnects then the message is sent to the server and stores it, so the next time the target of the messages connects it receives the queued messages.