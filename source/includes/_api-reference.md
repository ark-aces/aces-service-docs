
# API Reference


# Health

## Get Health
 
Get application health information.

> Example request:

```shell
curl 'http://localhost:8080/status'
```

```java
ServiceClient client = new ServiceClient("http://localhost");
Health health = client.getHealth();
```

> Returned response:

```json
{
	"status": "up"
}
```

### HTTP Request

`GET http://localhost:8080/status`


# Service Info

## Get Service Info

> Example request:

```shell
curl 'http://localhost/info'
```

```java
ServiceClient client = new ServiceClient("http://localhost");
ServiceInfo serviceInfo = client.getServiceInfo();
```
> Returned response:

```json
{
	"name":"ACES",
	"description":"an ACES service",
	"instructions":"Service usage instructions in markdown format."
	"capacities":[1, "BTC", "1.00 BTC"],
	"flatFee":"0.01",
	"percentageFee":"0.01",
	"contractSchema":{},
	"interfaces":["arkSmartbridgePayable", "arkReturnable"]
}
```

Gets Service Info object.

### HTTP Request

`GET http://localhost/info`


# Service Contract

## Create Service Contract

> Example request:

```shell
curl 'http://localhost/contracts' \
  -X POST \
  -d '{
    "correlationId": "8bbcbe9c-4955-4fd6-b97b-0bf3e8b5809f",
    "arguments": {
      "btcAmount": "1.00",
      "recipientBitcoinAddress": "1F1tAaz5x1HUXrCNLbtMDqcw6o5GNn4xqX",
      "returnArkAddres": "ARNJJruY6RcuYCXcwWsu4bx9kyZtntqeAx"
    }
  }'
```

```java
ServiceClient client = new ServiceClient("http://localhost");

Map<String, Object> arguments = new HashMap();
arguments.put("btcAmount", "1.00");
arguments.put("recipientBitcoinAddress", "1F1tAaz5x1HUXrCNLbtMDqcw6o5GNn4xqX");
arguments.put("returnArkAddres", "ARNJJruY6RcuYCXcwWsu4bx9kyZtntqeAx");

ContractRequest request = new ContractRequest();
request.setCorrelationId(UUID.randomUUID());
request.setArguments(arguments);

Contract contract = client.createContract(request);
```

> Returned response:

```json
{
  "id" : "f2c0244d-8473-417a-98ce-f30a979e925a",
  "correlationId" : "8bbcbe9c-4955-4fd6-b97b-0bf3e8b5809f",
  "status": "new",
  "btcAmount": "1.00",
  "requiredArkAmount" : "2250.00",
  "serviceArkAddress": "AewU1vEmPrtQNjdVo33cX84bfovY3jNAkV", 
  "returnArkAddress" : "ARNJJruY6RcuYCXcwWsu4bx9kyZtntqeAx",
  "createdAt": "2017-07-01T22:35:57.695Z"
}
``` 

Creates a new service contract.

### Parameters

Parameter       | In      | Type    | Required      | Description             
--------------- | ----    | ------- | ----          | ----------------------- 
ContractRequest | body    | string  | correlationId | The request to create a new contract.

### Schema

Properties      | Type    | Description             
--------------- | ------- | ----------------------- 
correlationId   | string  | Requestor generated globally unique identifier for correleating requests.
arguments 	  | array   |


### HTTP Request

`POST http://localhost/contracts`


## Get Service Contract

> Example request:

```shell
curl 'http://localhost/contracts/f2c0244d-8473-417a-98ce-f30a979e925a'
```

```javascript
ServiceClient client = new ServiceClient("http://localhost");
Contract contract = client.getContract("f2c0244d-8473-417a-98ce-f30a979e925a");
```

> Returned response:

```json
{
  "id" : "f2c0244d-8473-417a-98ce-f30a979e925a",
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

Gets service contract info for a contract.

### Parameters

Parameter       | In      | Type    | Required      | Description             
--------------- | ----    | ------- | ----          | ----------------------- 
id              | path    | string  | true          | Contract identifier.

### HTTP Request

`GET http://localhost/contracts/{id}`