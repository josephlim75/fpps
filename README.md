# Fraud Protection and Prevention System (FPPS)

## Architecture

 ![FPPS Architecture](/assets/images/fpps_architecture.drawio.png)


- In order for FPPS to be PCI compliant, FPPS should accepts only tokenized ISO8583 payload from financial institutions.  Financial institutions can selectively choose what PII data should be tokenized.  FPPS will used the tokenized PAN as the key to retrieve history data for near-realtime fraud processing.  Every tokenized message payload will be stored in FPPS backend.

- For all near-realtime authorization messages that require an approval or decline response from FPPS, such messages will be send to `FPPS Brokers` either using REST or gRPC via HTTP/2.  `FPPS Brokers` published all messages to `NATS` while waiting for a response from `FPPS Evaluators` to publish a decision whether to approve or decline a transaction before sending back to the caller.

- For any offline authorizations such as stand-in processing take over, FPPS accepts authorizations that already contained `authorization code`.  All offline messages are stream directly from `FPPS Brokers` to `RedPanda` for further processing and aggregation.

## Technology Stacks
- Rule base RETE Algorithm
- NATS
- RedPanda
- H20 (optional)
