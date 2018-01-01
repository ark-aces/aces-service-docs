
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
  "name": "BTC to ARK Channel Service",
  "description": "Transfer value from BTC to ARK.",
  "instructions": "Enter your BTC address to create an BTC to ARK transfer channel.",
  "capacities": [
    {
      "value": 10000,
      "unit": "ARK",
      "displayValue": "10000.00 ARK"
    }
  ],
  "flatFee": "0 BTC",
  "percentFee": "1%",
  "inputSchema": {
    "type": "object",
    "properties": {
      "recipientArkAddress": {
        "type": "string"
      }
    },
    "required": [
      "recipientArkAddress"
    ]
  },
  "outputSchema": {
    "type": "object",
    "properties": {
      "depositBtcAddress": {
        "type": "string"
      },
      "recipientArkAddress": {
        "type": "string"
      },
      "transfers": {
        "type": "array",
        "properties": {
          "btcAmount": {
            "type": "string"
          },
          "btcToArkRate": {
            "type": "string"
          },
          "btcFlatFee": {
            "type": "string"
          },
          "btcPercentFee": {
            "type": "string"
          },
          "btcTotalFee": {
            "type": "string"
          },
          "arkSendAmount": {
            "type": "string"
          },
          "arkTransactionId": {
            "type": "string"
          },
          "createdAt": {
            "type": "string"
          }
        }
      }
    }
  }
}
```


> Create Service Contract:

```shell
curl 'http:/btc-ark-channel-service.example.com/contracts' \
  -X POST \
  -d '{
    "correlationId": "8bbcbe9c-4955-4fd6-b97b-0bf3e8b5809f",
    "arguments": {
        "recipientArkAddress": "ARNJJruY6RcuYCXcwWsu4bx9kyZtntqeA"
    }"
  }'
```

```java
AcesServiceClient client = new AcesServiceClient(
  "https://bitcoin-transfer-service.example.com"
);

Map<String, Object> arguments = new HashMap();
arguments.put("recipientArkAddress", "ARNJJruY6RcuYCXcwWsu4bx9kyZtntqeAx");

ContractRequest request = new ContractRequest();
request.setCorrelationId(UUID.randomUUID());
request.setArguments(arguments);

Contract contract = client.createContract(request);
```

```json
{
  "id" : "abe05cd7-40c2-4fb0-a4a7-8d2f76e74978",
  "correlationId" : "8bbcbe9c-4955-4fd6-b97b-0bf3e8b5809f",
  "createdAt":"2017-07-01T22:35:57.695Z",
  "status": "executed",
  "results": {
    "depositBtcAddress": "1F1tAaz5x1HUXrCNLbtMDqcw6o5GNn4xqX",
    "recipientArkAddress": "ARNJJruY6RcuYCXcwWsu4bx9kyZtntqeAx",
    "transfers": [
      {
        "createdAt": "2017-07-01T22:36:57.005Z",
        "btcAmount": "1.000",
        "btcToArkRate": "0.040",
        "btcFlatFee": "0",
        "btcPercentFee": "1",
        "btcTotalFee": "0.001",
        "arkSendAmount": "40.000",
        "arkTransactionId": "49f55381c5c3c70f96d848df53ab7f9ae9881dbb8eb43e8f91f642018bf1258f"
      }
    ]
  }
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
  "createdAt":"2017-07-01T22:35:57.695Z",
  "status": "executed",
  "results": {
    "depositBtcAddress": "1F1tAaz5x1HUXrCNLbtMDqcw6o5GNn4xqX",
    "recipientArkAddress": "ARNJJruY6RcuYCXcwWsu4bx9kyZtntqeAx",
    "transfers": [
      {
        "createdAt": "2017-07-01T22:36:57.005Z",
        "btcAmount": "1.000",
        "btcToArkRate": "0.040",
        "btcFlatFee": "0",
        "btcPercentFee": "1",
        "btcTotalFee": "0.001",
        "arkSendAmount": "40.000",
        "arkTransactionId": "49f55381c5c3c70f96d848df53ab7f9ae9881dbb8eb43e8f91f642018bf1258f"
      }
    ]
  }
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
