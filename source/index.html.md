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

# PDAX FIX


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
Message | Descriptive text message



## Cancel Order


```shell
Request url -- ws://127.0.0.1/ws/order/cancel
Request body 
{
  "Event": "orders",
  "Payload": {
    "ClOrID": "12",
    "OrigClOrdID": "11",
    "Symbol": "BTC/USD",
    "OrderSideStr": "1"
  }
}
```

```

> The above endpoint returns JSON structured like this:

```json
{
  "OrderType": "C",
  "OrderID": "210515",
  "ClOrdID": "19",
  "ExecID": "1522703914261.8989",
  "ExecTransType": "0",
  "ExecType": "4",
  "OrdStatus": "4",
  "Symbol": "BTC/USD",
  "Side": "1",
  "OrderQty": "1",
  "LeavesQty": "0",
  "CumQty": "0",
  "Message": "system cancel"
}
```

This endpoint cancels order.

### Websocket

`ws://127.0.0.1/ws/order/cancel`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
Event |  | Identifies request is an order request
Payload |  | Contains order cancel request params

### Payload parameters

Parameter |  Description
--------- | -----------
ClOrID | Unique ID of cancel request as assigned by the institution.
OrigClOrdID | ClOrdID of the previous order as assigned by the institution, id of order to be canceled.
Symbol | The symbol of the original order. Must match the current order's symbol
OrderSideStr | 1=Buy 2=Sell. Must match the current order's side

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
Message | Descriptive text message

## Market Data Request


```shell
Request url -- ws://127.0.0.1/ws/market
Request body 
{
  "Event": "marketData",
  "Payload": {
    "MarketDataReqID": "1",
    "SubscriptionReqType": "1",
    "Symbol": "BTC/USD"
  }
}
```

```

> The above endpoint returns JSON structured like this:

```json
{
  "Symbol": "BTC/USD",
  "MDReqID": "1",
  "NoMDEntries": "30",
  "MDEntries": [
    {
      "EntryType": "0",
      "EntryPX": "7166.76",
      "Currency": "BTC",
      "EntrySize": "5.896313",
      "QuoteCondition": "A",
      "EntryOriginator": "",
      "MinQty": "",
      "QuoteEntryID": "104u6hq6x",
      "NumberOfOrders": "1",
      "EntryPositionNo": "1"
    },
    {
      "EntryType": "0",
      "EntryPX": "7166.29",
      "Currency": "BTC",
      "EntrySize": "6.767895",
      "QuoteCondition": "A",
      "EntryOriginator": "",
      "MinQty": "",
      "QuoteEntryID": "18de9y14rfd",
      "NumberOfOrders": "1",
      "EntryPositionNo": "2"
    },
    {
      "EntryType": "0",
      "EntryPX": "7166.22",
      "Currency": "BTC",
      "EntrySize": "6.916496",
      "QuoteCondition": "A",
      "EntryOriginator": "",
      "MinQty": "",
      "QuoteEntryID": "2gpsf1vrsnt",
      "NumberOfOrders": "1",
      "EntryPositionNo": "3"
    },
    {
      "EntryType": "0",
      "EntryPX": "7166.13",
      "Currency": "BTC",
      "EntrySize": "7.105057",
      "QuoteCondition": "A",
      "EntryOriginator": "",
      "MinQty": "",
      "QuoteEntryID": "3p26k5qetw9",
      "NumberOfOrders": "1",
      "EntryPositionNo": "4"
    },
    {
      "EntryType": "0",
      "EntryPX": "7165.99",
      "Currency": "BTC",
      "EntrySize": "7.453448",
      "QuoteCondition": "A",
      "EntryOriginator": "",
      "MinQty": "",
      "QuoteEntryID": "4xekp9l1v4p",
      "NumberOfOrders": "1",
      "EntryPositionNo": "5"
    },
    {
      "EntryType": "0",
      "EntryPX": "7160.49",
      "Currency": "BTC",
      "EntrySize": "14.953448",
      "QuoteCondition": "A",
      "EntryOriginator": "",
      "MinQty": "",
      "QuoteEntryID": "9c2vkvbuf",
      "NumberOfOrders": "1",
      "EntryPositionNo": "6"
    },
    {
      "EntryType": "0",
      "EntryPX": "7157.01",
      "Currency": "BTC",
      "EntrySize": "22.453448",
      "QuoteCondition": "A",
      "EntryOriginator": "",
      "MinQty": "",
      "QuoteEntryID": "18lq7zfid2v",
      "NumberOfOrders": "1",
      "EntryPositionNo": "7"
    },
    {
      "EntryType": "0",
      "EntryPX": "7154.03",
      "Currency": "BTC",
      "EntrySize": "29.953448",
      "QuoteCondition": "A",
      "EntryOriginator": "",
      "MinQty": "",
      "QuoteEntryID": "2gy4d3a5ebb",
      "NumberOfOrders": "1",
      "EntryPositionNo": "8"
    },
    {
      "EntryType": "0",
      "EntryPX": "7151.24",
      "Currency": "BTC",
      "EntrySize": "37.453448",
      "QuoteCondition": "A",
      "EntryOriginator": "",
      "MinQty": "",
      "QuoteEntryID": "3paii74sfjr",
      "NumberOfOrders": "1",
      "EntryPositionNo": "9"
    },
    {
      "EntryType": "0",
      "EntryPX": "7148.55",
      "Currency": "BTC",
      "EntrySize": "44.953448",
      "QuoteCondition": "A",
      "EntryOriginator": "",
      "MinQty": "",
      "QuoteEntryID": "4xmwnazfgs7",
      "NumberOfOrders": "1",
      "EntryPositionNo": "10"
    },
    {
      "EntryType": "0",
      "EntryPX": "7145.92",
      "Currency": "BTC",
      "EntrySize": "52.453448",
      "QuoteCondition": "A",
      "EntryOriginator": "",
      "MinQty": "",
      "QuoteEntryID": "65zaseu2i0n",
      "NumberOfOrders": "1",
      "EntryPositionNo": "11"
    },
    {
      "EntryType": "0",
      "EntryPX": "7143.32",
      "Currency": "BTC",
      "EntrySize": "59.953448",
      "QuoteCondition": "A",
      "EntryOriginator": "",
      "MinQty": "",
      "QuoteEntryID": "7eboxiopj93",
      "NumberOfOrders": "1",
      "EntryPositionNo": "12"
    },
    {
      "EntryType": "0",
      "EntryPX": "7140.74",
      "Currency": "BTC",
      "EntrySize": "67.453448",
      "QuoteCondition": "A",
      "EntryOriginator": "",
      "MinQty": "",
      "QuoteEntryID": "8mo32mjckhj",
      "NumberOfOrders": "1",
      "EntryPositionNo": "13"
    },
    {
      "EntryType": "0",
      "EntryPX": "7138.19",
      "Currency": "BTC",
      "EntrySize": "74.953448",
      "QuoteCondition": "A",
      "EntryOriginator": "",
      "MinQty": "",
      "QuoteEntryID": "9v0h7qdzlpz",
      "NumberOfOrders": "1",
      "EntryPositionNo": "14"
    },
    {
      "EntryType": "0",
      "EntryPX": "7135.62",
      "Currency": "BTC",
      "EntrySize": "82.453448",
      "QuoteCondition": "A",
      "EntryOriginator": "",
      "MinQty": "",
      "QuoteEntryID": "b3cvcu8mmyf",
      "NumberOfOrders": "1",
      "EntryPositionNo": "15"
    },
    {
      "EntryType": "1",
      "EntryPX": "7181.19",
      "Currency": "BTC",
      "EntrySize": "0.047815",
      "QuoteCondition": "A",
      "EntryOriginator": "",
      "MinQty": "",
      "QuoteEntryID": "65qyudfowd5",
      "NumberOfOrders": "1",
      "EntryPositionNo": "1"
    },
    {
      "EntryType": "1",
      "EntryPX": "7181.54",
      "Currency": "BTC",
      "EntrySize": "0.07321",
      "QuoteCondition": "A",
      "EntryOriginator": "",
      "MinQty": "",
      "QuoteEntryID": "7e3czhabxll",
      "NumberOfOrders": "1",
      "EntryPositionNo": "2"
    },
    {
      "EntryType": "1",
      "EntryPX": "7182.14",
      "Currency": "BTC",
      "EntrySize": "0.830926",
      "QuoteCondition": "A",
      "EntryOriginator": "",
      "MinQty": "",
      "QuoteEntryID": "8mfr4l4yyu1",
      "NumberOfOrders": "1",
      "EntryPositionNo": "3"
    },
    {
      "EntryType": "1",
      "EntryPX": "7183.9",
      "Currency": "BTC",
      "EntrySize": "1.830926",
      "QuoteCondition": "A",
      "EntryOriginator": "",
      "MinQty": "",
      "QuoteEntryID": "9us59ozm02h",
      "NumberOfOrders": "1",
      "EntryPositionNo": "4"
    },
    {
      "EntryType": "1",
      "EntryPX": "7183.93",
      "Currency": "BTC",
      "EntrySize": "1.855185",
      "QuoteCondition": "A",
      "EntryOriginator": "",
      "MinQty": "",
      "QuoteEntryID": "b34jesu91ax",
      "NumberOfOrders": "1",
      "EntryPositionNo": "5"
    },
    {
      "EntryType": "1",
      "EntryPX": "7192.8",
      "Currency": "BTC",
      "EntrySize": "9.355185",
      "QuoteCondition": "A",
      "EntryOriginator": "",
      "MinQty": "",
      "QuoteEntryID": "cbp9hy39o6v",
      "NumberOfOrders": "1",
      "EntryPositionNo": "6"
    },
    {
      "EntryType": "1",
      "EntryPX": "7196",
      "Currency": "BTC",
      "EntrySize": "16.855185",
      "QuoteCondition": "A",
      "EntryOriginator": "",
      "MinQty": "",
      "QuoteEntryID": "dk1nn1xwpfb",
      "NumberOfOrders": "1",
      "EntryPositionNo": "7"
    },
    {
      "EntryType": "1",
      "EntryPX": "7198.76",
      "Currency": "BTC",
      "EntrySize": "24.355185",
      "QuoteCondition": "A",
      "EntryOriginator": "",
      "MinQty": "",
      "QuoteEntryID": "ese1s5sjqnr",
      "NumberOfOrders": "1",
      "EntryPositionNo": "8"
    },
    {
      "EntryType": "1",
      "EntryPX": "7201.4",
      "Currency": "BTC",
      "EntrySize": "31.855185",
      "QuoteCondition": "A",
      "EntryOriginator": "",
      "MinQty": "",
      "QuoteEntryID": "g0qfx9n6rw7",
      "NumberOfOrders": "1",
      "EntryPositionNo": "9"
    },
    {
      "EntryType": "1",
      "EntryPX": "7203.99",
      "Currency": "BTC",
      "EntrySize": "39.355185",
      "QuoteCondition": "A",
      "EntryOriginator": "",
      "MinQty": "",
      "QuoteEntryID": "h92u2dhtt4n",
      "NumberOfOrders": "1",
      "EntryPositionNo": "10"
    },
    {
      "EntryType": "1",
      "EntryPX": "7206.56",
      "Currency": "BTC",
      "EntrySize": "46.855185",
      "QuoteCondition": "A",
      "EntryOriginator": "",
      "MinQty": "",
      "QuoteEntryID": "ihf87hcgud3",
      "NumberOfOrders": "1",
      "EntryPositionNo": "11"
    },
    {
      "EntryType": "1",
      "EntryPX": "7209.1",
      "Currency": "BTC",
      "EntrySize": "54.355185",
      "QuoteCondition": "A",
      "EntryOriginator": "",
      "MinQty": "",
      "QuoteEntryID": "jprmcl73vlj",
      "NumberOfOrders": "1",
      "EntryPositionNo": "12"
    },
    {
      "EntryType": "1",
      "EntryPX": "7211.66",
      "Currency": "BTC",
      "EntrySize": "61.855185",
      "QuoteCondition": "A",
      "EntryOriginator": "",
      "MinQty": "",
      "QuoteEntryID": "ky40hp1qwtz",
      "NumberOfOrders": "1",
      "EntryPositionNo": "13"
    },
    {
      "EntryType": "1",
      "EntryPX": "7214.2",
      "Currency": "BTC",
      "EntrySize": "69.355185",
      "QuoteCondition": "A",
      "EntryOriginator": "",
      "MinQty": "",
      "QuoteEntryID": "m6gemswdy2f",
      "NumberOfOrders": "1",
      "EntryPositionNo": "14"
    },
    {
      "EntryType": "1",
      "EntryPX": "7216.74",
      "Currency": "BTC",
      "EntrySize": "76.855185",
      "QuoteCondition": "A",
      "EntryOriginator": "",
      "MinQty": "",
      "QuoteEntryID": "nessrwr0zav",
      "NumberOfOrders": "1",
      "EntryPositionNo": "15"
    }
  ]
}
```

This endpoint subcribes to Market Data.

### Websocket

`ws://127.0.0.1/ws/market`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
Event |  | Identifies request is an order request
Payload |  | Contains market data request params

### Payload parameters

Parameter |  Description
--------- | -----------
MarketDataReqID | Must be unique, or the ID of previous Market Data Request to disable if SubscriptionRequestType = Disable previous Snapshot + Updates Request (2).
SubscriptionReqType | Indicates to the other party what type of response is expected. A subscribe request asks for updates as the status changes. Unsubscribe will cancel any future update messages from the counter party. 1 = Snapshot + Updates 2 = Unsubscribe
Symbol | The symbol for the base and variable currencies of the currency pair in the following format: baseCCY/variableCCY

### Response parameters
Parameter |  Description
--------- | -----------
Symbol | The symbol for the base and variable currencies of the currency pair in the following format: baseCCY/variableCCY
MDReqID | Unique identifier for Market Data Request
NoMDEntries | Number of entries in market data message.
MDEntries | Array of market data entries
EntryType | The side of the rate. Part of the repeating group of fields for each rate in the update. 0= Bid 1= Offer
EntryPX | Price of the Market Data Entry.
Currency | The value of this field represents the denomination of the quantity fields (for example, BTC represents a quantity of BTC). This may be the base or term currency of a currency pair.
EntrySize | Quantity represented by the Market Data Entry.
QuoteCondition | Indicates whether the rate is tradable or only indicative. A=Open/Active B=Closed/Inactive
EntryOriginator | Originator of a Market Data Entry. Present on full non-aggregated book attributed market data updates. A full book, non-aggregated price can be attributed to a sender if MarketDepth (264) = 0, AggregatedBook (266) = N, AttibutedPrices (7560) = Y are sent on the Market Data request.
MinQty | MinQty of the market data entry. Will only be available if FIX Server's "Include MD MinQty" option is enabled and prices from running counterparties also have MinQty values set.
QuoteEntryID | Unique id for the prices.
NumberOfOrders | Number of orders for the aggregate. Usually shows 1, except when aggregated and attributed are both set to true where real number of orders for aggregated is specified.
MDEntryPositionNo | Display position of a bid or offer, numbered from most competitive to least competitive, per market side, beginning with 1.
