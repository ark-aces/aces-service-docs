
# Overview

> Get Service Info:

```shell
curl 'https://bitcoin-transfer-service.example.com/info' \
  -X GET
```

```java
AcesServiceClient client = new AcesServiceClient(
  "https://bitcoin-transfer-service.example.com"
);
Info info = client.getInfo();
```

```json
{
  "name": "Bitcoin Transfer Service",
  "description": "Transfer value from Ark to Bitcoin.",
  "instructions": "Enter the Bitcoin amount to transfer and recipient Bitcoin address.",
  "capacities": [
    {
      "value": 10000,
      "unit": "BTC",
      "displayValue": "10000.00 BTC"
    }
  ],
  "flatFee": "1 ARK",
  "percentFee": "1.25%",
  "contractSchema": {
    "type": "object",
    "properties": {
      "btcAmount": {
        "type": "string"
      },
      "recipientBitcoinAddress": {
        "type": "string"
      },
      "returnArkAddress": {
        "type": "string"
      }
    },
    "required": [
      "btcAmount",
      "recipientBitcoinAddress",
      "returnArkAddress"
    ]
  },
  "interfaces": [
    "arkSmartbridgePayable"
  ]
}
```


> Create Service Contract:

```shell
curl 'http://bitcoin-transfer-service.example.com/contracts' \
  -X POST \
  -d '{
    "correlationId": "8bbcbe9c-4955-4fd6-b97b-0bf3e8b5809f",
    "arguments": {
        "btcAmount": "1",
        "recipientBitcoinAddress": "1F1tAaz5x1HUXrCNLbtMDqcw6o5GNn4xqX",
        "returnArkAddres": "ARNJJruY6RcuYCXcwWsu4bx9kyZtntqeA
    }"
  }'
```

```java
AcesServiceClient client = new AcesServiceClient(
  "https://bitcoin-transfer-service.example.com"
);

Map<String, Object> arguments = new HashMap();
arguments.put("btcAmount", "1");
arguments.put("recipientBitcoinAddress", "1F1tAaz5x1HUXrCNLbtMDqcw6o5GNn4xqX");
arguments.put("returnArkAddres", "ARNJJruY6RcuYCXcwWsu4bx9kyZtntqeAx");

ContractRequest request = new ContractRequest();
request.setCorrelationId(UUID.randomUUID());
request.setArguments(arguments);

Contract contract = client.createContract(request);
```

```json
{
  "id" : "abe05cd7-40c2-4fb0-a4a7-8d2f76e74978",
  "correlationId" : "8bbcbe9c-4955-4fd6-b97b-0bf3e8b5809f",
  "status": "new",
  "btcAmount": "1.00",
  "requiredArkAmount" : "2250.00",
  "serviceArkAddress": "AewU1vEmPrtQNjdVo33cX84bfovY3jNAkV", 
  "returnArkAddress" : "ARNJJruY6RcuYCXcwWsu4bx9kyZtntqeAx",
  "actualArkCost": "0.80000000",
  "returnArkAmount": "0.2000000",
  "returnArkTransactionId": "49f55381c5c3c70f96d848df53ab7f9ae9881dbb8eb43e8f91f642018bf1258f",
  "contractTransactionHash": "500224999259823996",
  "contractAddress": "0xdaa24d02bad7e9d6a80106db164bad9399a0423e",
  "createdAt":"2017-07-01T22:35:57.695Z"
}
```

> Get Service Contract:

```shell
curl 'http://bitcoin-transfer-service.example.com/contracts/abe05cd7-40c2-4fb0-a4a7-8d2f76e74978' \
  -X GET
```

```java
AcesServiceClient client = new AcesServiceClient(
  "https://bitcoin-transfer-service.example.com"
);
Contract contract = client.getContract("abe05cd7-40c2-4fb0-a4a7-8d2f76e74978");
```

```json
{
  "id" : "abe05cd7-40c2-4fb0-a4a7-8d2f76e74978",
  "correlationId" : "8bbcbe9c-4955-4fd6-b97b-0bf3e8b5809f",
  "status": "executed",
  "btcAmount": "1.00",
  "requiredArkAmount" : "2250.00",
  "serviceArkAddress": "AewU1vEmPrtQNjdVo33cX84bfovY3jNAkV", 
  "returnArkAddress" : "ARNJJruY6RcuYCXcwWsu4bx9kyZtntqeAx",
  "returnArkAmount": "50.00",
  "arkPaymentTransactionId": "iefa85381c5c3c70f96d848df53ab7ed6135a7fb34b43e8f91f6j42018bf1258",
  "returnArkTransactionId": "49f55381c5c3c70f96d848df53ab7f9ae9881dbb8eb43e8f91f642018bf1258f",
  "bitcoinTransactionId": "2f12236a2684264ff33c7f1ce28ea9ed6135a7fb34af4185d504b6022fba8f77",
  "createdAt": "2017-07-01T22:35:57.695Z",
  "executedAt": "2017-07-01T22:35:58.000Z"
}
```


At the center of the ACES Service API is the Service Contract. Each service defines
and operates on one type of contract. A Service Contract defines a particular service
a provider is able to fulfil, which can be anything from uploading a file to a storage
blockchain, performing value transfers, creating smart contracts, executing code on
blockchain based computing platforms, or interacting with IoT hardware.

<img src="images/figures/service-seq-diagram.png" alt="Service Contract Sequence Diagram" />

Consumers of a service get the contract schema from in the Service Info 
endpoint. This tells you what parameters a contract takes and what data is returned from the contract.
ACES uses the [Json Schema](http://json-schema.org/) format for defining data schemas.
ACES Services may implement any number of [Contract Interfaces]("#contract-interfaces"), 
which semantically define a set of contract arguments and result properties.

ACES Services can react in real-time to blockchain events by registering with an 
[Encoded Listener](https://github.com/ark-aces/aces-encoded-listener-api) instance.
For example, an ACES Service can register with an Ark Encoded Listener and
accept service contract payment via an Ark transaction by listening for specific
`SmartBridge` data associated with the service contract. Services can also create
wallets for a service contract and listen for deposit transactions into that wallet.

<img src="images/figures/service-enc-listener-seq-diagram.png" 
    alt="Service Encoded Listener Sequence Diagram" />
