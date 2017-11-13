# Service Interfaces

The ACES Service API defines a highly abstract blueprint for services that can
perform many different types of functions across many different blockchains. 
As such, ACES contracts are able to accept a flexible range of input values
and return an equally flexible range of result data for contract executions.

To simplify integration of services into a common Marketplace, the ACES Service
API defines several Service Interfaces to control presentational aspects 
of different types of contracts.

These interfaces tell consumers the semantic meaning of certain sets of contract 
request and response parameters, so they can be rendered or controlled differently
in the Marketplace or other UI applications.

We expect to add more Services Interfaces as more blockchains are integrated
and different types of contracts are defined. Some examples may include interfaces
such as `fileUpload`, `multiFileUpload`, `expirable`, etc...


### Ark SmartBridge Payable

The `arkSmartbrigePayable` interface specifies that the contract can be paid for
using an Ark SmartBridge transaction.

Response Parameters:

| Parameter | Type | Description |
| --------- | ---- | ----------- |
| arkServiceAddress | String | The service Ark address to send payment transaction to. |



### Ark Returnable

The `arkReturnable` interface specifies that the contract can return refundable
ARK to the given return address. 

Request Parameters:

| Parameter | Type | Description |
| --------- | ---- | ----------- |
| arkReturnAddress | String | The return Ark address to send refund transaction to. |


### Expirable

The `expirable` interface specifies that the contract will expire after the given
date if not executed.

Response Parameters:

| Parameter | Type | Description |
| --------- | ---- | ----------- |
| expirationDate | String | The ISO 8601 formatted date time string specifying when the contract will expire.
 |

