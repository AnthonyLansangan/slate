---
title: API Reference

language_tabs: # must be one of https://git.io/vQNgJ
  - shell

toc_footers:
  - <a href='#'>Sign Up for a Developer Key</a>
  - <a href='https://github.com/lord/slate'>Documentation Powered by Slate</a>

includes:
  - errors

search: true
---

# Introduction


## Post Order

```shell
Request url -- ws://127.0.0.1/ws/order
Request body 
{
  "Event": "orders",
  "Payload": {
    "ClOrID": 1,
    "Symbol": "BTC/USD",
    "OrderSideStr": "1",
    "OrderTypeStr": "C",
    "OrderQty": "1",
    "Price": "10",
    "StopPrice": "20"
  }
}
```


```
> The above endpoint returns JSON structured like this:

```json
{
  "OrderType": "C",
  "OrderID": "203277",
  "ClOrdID": "12",
  "ExecID": "1522616700795.20761",
  "ExecTransType": "0",
  "ExecType": "0",
  "OrdStatus": "0",
  "Symbol": "BTC/USD",
  "Side": "1",
  "OrderQty": "1",
  "LeavesQty": "1",
  "CumQty": "0",
  "Message": ""
}
```

This posts order

### Websocket

`ws://127.0.0.1/ws/order`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
Event |  | Identifies request is an order request
Payload |  | Contains order request params

### Payload parameters

Parameter |  Description
--------- | -----------
ClOrID | A unique identifier assigned by the client. Uniqueness must be guaranteed within a single trading day.
Symbol | The symbol for the base and variable currencies of the currency pair in the following format: baseCCY/variableCCY
OrderSideStr | 1=Buy 2=Sell
OrderTypeStr | 3=Stop Loss, 4=Stop Limit, C=Forex Market, F=Forex Limit, P=Pegged, V=Trailing Stop, W=One Cancels Other, X=If Done, Y=If Done OCO, Z=Iceberg
OrderQty | The amount of the dealt currency (using the Symbol field) to be either bought or sold (as determined by the OrderSideStr field).
Price | If the OrdType is Stop Limit, Forex Limit or Iceberg, then this is set to the limit price. For If Done and If Done OCO, this field represents the If Leg Limit Price. Value should be greater than zero.
StopPrice | The stop rate at which the market order will be placed into the market if OrdType is Stop (3). Or The stop rate at which the limit order will be placed into the market if OrdType is Stop Limit (4). Value should be greater than zero.

### Response parameters
Parameter |  Description
--------- | -----------
OrderType | 3 = Stop 4 = Stop Limit C = Forex Market F = Forex Limit Z = Iceberg
OrderID | Unique identifier for Order as assigned by Shift Forex. Uniqueness must be guaranteed within a single trading day.
ClOrdID | Unique identifier for Order as assigned by client. Uniqueness must be guaranteed within a single trading day.
ExecID | Unique identifier of execution message as assigned by Shift Forex.
ExecTransType | Execution transaction type: 0 = New 3 = Status
ExecType | Describes the specific ExecutionRpt (i.e. Pending Cancel) while OrdStatus will always identify the current order status (i.e. Partially Filled). Note: please refer to OrdStatus for partial fills as ExecType will always return fill for any execution. 0 = New 2 = Fill 4 = Canceled 5 = Replace 8 = Rejected C = Expired
OrdStatus | Identifies current status of order. 0 = New 1 = Partially filled 2 = Filled 4 = Canceled 5 = Replaced 8 = Rejected C = Expired
Symbol | The symbol for the base and variable currencies of the currency pair in the following format: baseCCY/variableCCY
Side | 1=Buy 2=Sell
OrderQty | Quantity ordered
LeavesQty | Quantity open for further execution. If the OrdStatus is Canceled, Expired, or Rejected (in which case the order is no longer active) then LeavesQty could be 0, otherwise LeavesQty = OrderQty - CumQty.
CumQty | Total quantity (e.g. number of shares) filled. Not sent if ExecType = 8, Rejected, or if the entire order is canceled, ExecType = 4, Canceled.



## Get a Specific Kitten

```ruby
require 'kittn'

api = Kittn::APIClient.authorize!('meowmeowmeow')
api.kittens.get(2)
```

```python
import kittn

api = kittn.authorize('meowmeowmeow')
api.kittens.get(2)
```

```shell
curl "http://example.com/api/kittens/2"
  -H "Authorization: meowmeowmeow"
```

```javascript
const kittn = require('kittn');

let api = kittn.authorize('meowmeowmeow');
let max = api.kittens.get(2);
```

> The above command returns JSON structured like this:

```json
{
  "id": 2,
  "name": "Max",
  "breed": "unknown",
  "fluffiness": 5,
  "cuteness": 10
}
```

This endpoint retrieves a specific kitten.

<aside class="warning">Inside HTML code blocks like this one, you can't use Markdown, so use <code>&lt;code&gt;</code> blocks to denote code.</aside>

### HTTP Request

`GET http://example.com/kittens/<ID>`

### URL Parameters

Parameter | Description
--------- | -----------
ID | The ID of the kitten to retrieve

## Delete a Specific Kitten

```ruby
require 'kittn'

api = Kittn::APIClient.authorize!('meowmeowmeow')
api.kittens.delete(2)
```

```python
import kittn

api = kittn.authorize('meowmeowmeow')
api.kittens.delete(2)
```

```shell
curl "http://example.com/api/kittens/2"
  -X DELETE
  -H "Authorization: meowmeowmeow"
```

```javascript
const kittn = require('kittn');

let api = kittn.authorize('meowmeowmeow');
let max = api.kittens.delete(2);
```

> The above command returns JSON structured like this:

```json
{
  "id": 2,
  "deleted" : ":("
}
```

This endpoint deletes a specific kitten.

### HTTP Request

`DELETE http://example.com/kittens/<ID>`

### URL Parameters

Parameter | Description
--------- | -----------
ID | The ID of the kitten to delete

