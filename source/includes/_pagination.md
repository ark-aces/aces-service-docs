
# Pagination

```shell
url="http://localhost/contracts/3985723987423/executions"
url="$url?pageSize=100"
url="$url&page=4"
url="$url&continuation=839729837520395702375023"
curl -X GET "$url"  
```

```java
ServiceClient client = new ServiceClient("http://localhost");

ContractExecutionsRequest request = new ContractExecutionsRequest();
request.setPageSize(100);
request.setPage(4);
request.setContinuation("933502938509238509283059");

Page<ContractExecution> executionsPage = client.getContractExecutionsPage(request);
```

```json
{
  "pageSize": "100",
  "page": "4",
  "continuation": "933502938509238509283059",
  "items": [
    {
      "id": "1",
      "status": "executed",
      "executedAt": "2017-11-01T00:00:00.000Z"
    }
  ]
}
```

A `GET` request that returns a list of unbound items will be paginated
in a way to return only a small number of items per page.

Clients can request additional pages by performing additional `GET`
requests to retrieve the next page.

Page number is zero-offset such that the first page has `page = 0`.

The page response contains a `continuation` property that can be added
to `GET` requests for the next page to stabilize pagination offsets.
Including the continuation parameter ensures that newly added items
will not shift down items in the next page. This allows pages to be
iterated over without encountering repeat elements as elements are
added.

