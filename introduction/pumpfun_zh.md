# pumpfun 快速入门

pumpfun 是一个token发射平台，你每天都可以从pumpfun从中发现大的机会。我们目前提供所有事件的消息流，包括:Token Create、Token Swap、Token Withdraw、Token Migrate。
简单流程：

- 用户订阅blocksmith实时行情流
- 用户发现交易机会，请求blocksmith构建交易体
- 然后用户通过自己的私钥给返回的交易体签名
- 最后用户发送签名后的交易给blocksmith进行上链

## Token Create

API

```proto
rpc SubscribePumpFunCreateStream(SubscribePumpFunCreateStreamRequest) returns (stream SubscribePumpFunCreateStreamResponse);

message SubscribePumpFunCreateStreamRequest {}

message EventMeta {
  string txHash = 1;
  int64 slot = 2;
  int64 blockTime = 3;
  int64 fee = 4;
  int64 tip = 5;
  string tag = 6;
  int64 balance = 7;
}

message SubscribePumpFunCreateStreamResponse {
  EventMeta meta = 1;
  CreateInfo createInfo = 2;
}
message CreateInfo{
  string name = 1;
  string symbol = 2;
  string uri = 3;
  string mint = 4;
  string bondingCurve = 5;
  string creator = 6;
}
```

resposne JSON序列化

```json
{
  "meta": {
    "txHash": "4TaXHCHaxadNA9fenpzCuAt67zdtrMgrd1H9UUsF4mJzK9CbDkVoYhAtc6HRJGuNrj2tq78EtjhLoArHfWsnJj6D",
    "slot": 310321231,
    "blockTime": 1735378353800,
    "fee": 476392,
    "balance": 26154831
  },
  "createInfo": {
    "name": "noiut",
    "symbol": "noiut",
    "uri": "https://ipfs.io/ipfs/QmQPVYVfgXD1dRh2azypVUZsKjZWPkRzWzy6FTxQknUBCm",
    "mint": "8NhnduXvbVaLZHYCuEn8aLxkhaJC6wiHenK18x1Hpump",
    "bondingCurve": "F1xEPbaNk1DzwpJfyyBumTSRJDqLfwfsvLersLPvpwqd",
    "creator": "8g7nUMg7VN4fgfZDoXpwb6aNXjjsKf2rRzdieWT62nNj"
  }
}
```

## Token Withdraw

API

```proto
rpc SubscribePumpFunWithdrawStream(SubscribePumpFunWithdrawStreamRequest) returns (stream SubscribePumpFunWithdrawStreamResponse);

message SubscribePumpFunWithdrawStreamRequest {}

message SubscribePumpFunWithdrawStreamResponse {
  EventMeta meta = 1;
  WithdrawInfo withdrawInfo = 2;
}
message WithdrawInfo{
  string mint = 1;
  string bondingCurve = 2;
}
```

resposne JSON序列化

```json
{
  "meta": {
    "txHash": "4TaXHCHaxadNA9fenpzCuAt67zdtrMgrd1H9UUsF4mJzK9CbDkVoYhAtc6HRJGuNrj2tq78EtjhLoArHfWsnJj6D",
    "slot": 310321231,
    "blockTime": 1735378353800,
    "fee": 476392,
    "balance": 26154831
  },
  "withdrawInfo": {
    "mint": "8NhnduXvbVaLZHYCuEn8aLxkhaJC6wiHenK18x1Hpump",
    "bondingCurve": "F1xEPbaNk1DzwpJfyyBumTSRJDqLfwfsvLersLPvpwqd"
  }
}
```

## Token Swap

API

```proto
rpc SubscribePumpFunSwapStream(SubscribePumpFunSwapStreamRequest) returns (stream SubscribePumpFunSwapStreamResponse);

message SubscribePumpFunSwapStreamRequest {}

message SubscribePumpFunSwapStreamResponse {
  EventMeta meta = 1;
  PumpFunSwapInfo swapInfo = 2;
  SwapExtra swapExtra = 3;
  BalanceExtra balanceExtra = 4;
}
message PumpFunSwapInfo{
  string mint = 1;
  string bondingCurve = 2;
  string user = 3;
  uint64 solAmount = 4;
  uint64 tokenAmount = 5;
  bool isBuy = 6;
  int64 timestamp = 7;
  uint64 virtualSolReserves = 8;
  uint64 virtualTokenReserves = 9;
}
message SwapExtra{
  string side = 1;
  double baseQty = 2;
  double quoteQty = 3;
  double price = 4;//priceUSD=price*quoteUSDPrice
  double liquidity = 5;//liquidityUSD=liquidity*quoteUSDPrice
  double cap = 6;//capUSD=cap*quoteUSDPrice
  double quoteUSDPrice = 7;
}
message BalanceExtra{
  uint64 userSolBalance = 1;
  uint64 userBaseBalance = 2;
  uint64 poolQuoteBalance = 3;
  uint64 poolBaseBalance = 4;
}
```

resposne JSON序列化

```json
{
  "meta": {
    "txHash": "3X9zNME1rU9pDpJks2FxGEcBjdXkQrAENEUF8gd8BoCgbaLRgZJ9RJ97SGErFV8cZhPiZW7hAaqgi1SC5ygkwXQW",
    "slot": 310322590,
    "blockTime": 1735378896800,
    "fee": 86874,
    "balance": 323197297
  },
  "swapInfo": {
    "mint": "Ag3rduERjxVShq3jCHy7nxTRXAu9M7EhNKZQMdZhpump",
    "bondingCurve": "GQv1SHACHWLs37oBEVbxCdMkXsBw5SzVyRNQNDUqgwFE",
    "user": "6bxpX2eFhuDjy7jjuLJZUrciJY1iFqLBdp9QumWm66s2",
    "solAmount": 8851140,
    "tokenAmount": 314061000000,
    "timestamp": 1735378897,
    "virtualSolReserves": 30115445820,
    "virtualTokenReserves": 1068886716653914
  },
  "swapExtra": {
    "side": "SELL",
    "baseQty": 314061,
    "quoteQty": 0.00885114,
    "price": 2.81828689e-8,
    "liquidity": 60.23089164,
    "cap": 28.1745908,
    "quoteUSDPrice": 185.99812225
  },
  "balanceExtra": {
    "userSolBalance": 323197297,
    "userBaseBalance": 465736,
    "poolQuoteBalance": 30115445820,
    "poolBaseBalance": 1068886716653914
  }
}
```

## Build swap tx

API

```proto
rpc PumpFunSwap(PumpFunSwapRequest) returns (PumpFunSwapResponse);

message PumpFunSwapRequest {
  string user = 1;
  string bondingCurve = 2;
  string tokenIn = 3;
  string tokenOut = 4;
  string amountIn = 5;
  string amountOut = 6;
  bool checkAta = 7;
  optional Fee fee = 8;
}

message PumpFunSwapResponse{
  string transaction = 1;
}
```

request JSON序列化

```json
{
  "user": "8g7nUMg7VN4fgfZDoXpwb6aNXjjsKf2rRzdieWT62nNj",
  "bondingCurve": "F1xEPbaNk1DzwpJfyyBumTSRJDqLfwfsvLersLPvpwqd",
  "tokenIn": "So11111111111111111111111111111111111111112",
  "tokenOut": "8NhnduXvbVaLZHYCuEn8aLxkhaJC6wiHenK18x1Hpump",
  "amountIn": "1000000000",
  "amountOut": "1"
}
```

resposne JSON序列化

```json
{
  "transaction":"base64tx"
}
```
