
# OSI model
The OSI ( Open Systems Interconnection ) model is a conceptual framework that standardizes how different network system communicate. 

### The 7 Layers ( Top - Bottom )

```
7  Application   ── user-facing protocols (HTTP, DNS, SMTP)
6  Presentation  ── encoding, encryption, compression (TLS, JPEG)
5  Session       ── session management (RPC, sockets)
4  Transport     ── end-to-end delivery (TCP, UDP)
3  Network       ── routing, addressing (IP, ICMP)
2  Data Link     ── framing, MAC addressing (Ethernet, Wi-Fi)
1  Physical      ── bits over wire/air (cables, radio)
```

---

- In Backend Example is that User send data to Frontend and frontend in JavaScript or TypeScript  send data to Backend and Backend in ( Rust or Jo ) there are use to serializationn or deserialization. 

## Serialization 
Converting Data into a byte stem or text format ( JSON, XML, Message-Pack )  so it can be stored or sent over a network.

## Deserialization
The reverse process -- parsing a byte steam/ text bock into a useble data object. 

```ts
[ Object ] --serialize--> [ Bytes/Text ] --deserialize--> [ Object ]
```

|Use Case|Serialization Role|
|---|---|
|REST/gRPC APIs|Request/response bodies (JSON, Protobuf)|
|Databases|Persisting rows/documents (BSON, Avro)|
|Message queues|Kafka/RabbitMQ payloads|
|Caching|Redis/Memcached values|
|Inter-service calls|Contract-enforced data exchange|
|Logging/telemetry|Structured log events|

---
