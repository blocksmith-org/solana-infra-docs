# RaydiumAmm 快速入门

raydium amm 类似 uniswap v2 的 dex 协议，价格计算的逻辑和 uniswap v2 类似。目前从pumpfun发射成功的token，都是添加流动性到这个协议中。目前我们提供raydium amm 的行情流，包括:Token Create(pump migrate会触发这个事件)、swap、Build Swap Tx。

简单流程：

- 用户订阅blocksmith实时行情流
- 用户发现交易机会，请求blocksmith构建交易体
- 然后用户通过自己的私钥给返回的交易体签名
- 最后用户发送签名后的交易给blocksmith进行上链

## Token Create

API

```proto
rpc SubscribeRaydiumCreateStream(SubscribeRaydiumCreateStreamRequest) returns (stream SubscribeRaydiumCreateStreamResponse);

message SubscribeRaydiumCreateStreamRequest {}
message SubscribeRaydiumCreateStreamResponse {
  EventMeta meta = 1;
  RaydiumCreateInfo createInfo = 2;
}
message RaydiumCreateInfo{
  string amm = 1;
  string coinMint = 2;
  string pcMint = 3;
  string poolCoinTokenAccount = 4;
  string poolPcTokenAccount = 5;
  string user = 6;
  string baseMint = 7;
  string quoteMint = 8;
  int64 openTime = 9;
  uint64 initPcAmount = 10;
  uint64 initCoinAmount = 11;
}
```

resposne JSON

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
    "amm": "8NhnduXvbVaLZHYCuEn8aLxkhaJC6wiHenK18x1Hpump",
    "coinMint": "8NhnduXvbVaLZHYCuEn8aLxkhaJC6wiHenK18x1Hpump",
    "pcMint": "8NhnduXvbVaLZHYCuEn8aLxkhaJC6wiHenK18x1Hpump",
    "poolCoinTokenAccount": "8NhnduXvbVaLZHYCuEn8aLxkhaJC6wiHenK18x1Hpump",
    "poolPcTokenAccount": "8NhnduXvbVaLZHYCuEn8aLxkhaJC6wiHenK18x1Hpump",
    "user": "8NhnduXvbVaLZHYCuEn8aLxkhaJC6wiHenK18x1Hpump",
    "baseMint": "8NhnduXvbVaLZHYCuEn8aLxkhaJC6wiHenK18x1Hpump",
    "quoteMint": "8NhnduXvbVaLZHYCuEn8aLxkhaJC6wiHenK18x1Hpump",
    "openTime": 1735378353800,
    "initPcAmount": 10000000000000,
    "initCoinAmount": 100000000000
  }
}
```

## Token Swap

API

```proto
rpc SubscribeRaydiumSwapStream(SubscribeRaydiumSwapStreamRequest) returns (stream SubscribeRaydiumSwapStreamResponse);

message SubscribeRaydiumSwapStreamRequest {}
message SubscribeRaydiumSwapStreamResponse {
  EventMeta meta = 1;
  RaydiumSwapInfo swapInfo = 2;
  SwapExtra swapExtra = 3;
  BalanceExtra balanceExtra = 4;
}
message RaydiumSwapInfo{
  string amm = 1;
  string poolCoinTokenAccount = 2;
  string poolPcTokenAccount = 3;
  string user = 4;
  string baseMint = 5;
  string quoteMint = 6;
  string tokenInMint = 7;
  int32 tokenInDecimals = 8;
  string tokenOutMint = 9;
  int32 tokenOutDecimals = 10;
  string amountIn = 11;
  string amountOut = 12;
}
```

response JSON

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
    "amm": "8NhnduXvbVaLZHYCuEn8aLxkhaJC6wiHenK18x1Hpump",
    "poolCoinTokenAccount": "8NhnduXvbVaLZHYCuEn8aLxkhaJC6wiHenK18x1Hpump",
    "poolPcTokenAccount": "8NhnduXvbVaLZHYCuEn8aLxkhaJC6wiHenK18x1Hpump",
    "user": "8NhnduXvbVaLZHYCuEn8aLxkhaJC6wiHenK18x1Hpump",
    "baseMint": "8NhnduXvbVaLZHYCuEn8aLxkhaJC6wiHenK18x1Hpump",
    "quoteMint": "8NhnduXvbVaLZHYCuEn8aLxkhaJC6wiHenK18x1Hpump",
    "tokenInMint": "8NhnduXvbVaLZHYCuEn8aLxkhaJC6wiHenK18x1Hpump",
    "tokenInDecimals": 6,
    "tokenOutMint": "8NhnduXvbVaLZHYCuEn8aLxkhaJC6wiHenK18x1Hpump",
    "tokenOutDecimals": 6,
    "amountIn": "100000000000",
    "amountOut": "99999999999"
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
rpc RaydiumSwap(RaydiumSwapRequest) returns (RaydiumSwapResponse);

message RaydiumSwapRequest {
  string user = 1;
  string amm = 2;
  string poolCoinTokenAccount = 3;
  string poolPcTokenAccount = 4;
  string tokenIn = 5;
  string tokenOut = 6;
  string amountIn = 7;
  string amountOut = 8;
  bool checkAta = 9;
  optional Fee fee = 10;
}
message Fee{
  optional uint32 computeLimit = 1 ;
  optional uint64 computePrice = 2 ;
  optional string tipAccount = 3 ;//jito|bloxroute|nextblock|temporal|(address)
  optional uint64 tipAmount = 4 ;
}
message RaydiumSwapResponse{
  string transaction = 1;
}
```

resposne JSON

```json
{
  "transaction":"base64tx"
}
```
