
[![Binance Community API](https://cdn.discordapp.com/attachments/569865969120575519/718166995354255370/binance-api-black.png)](https://dev.binance.vision/)


# Updated Node Binance API

!!! BE CAREFULL !!!

This project is a duplicate of **node-binance-api** library, I just added some new features that I needed for my personal project.

See ==> Node binance api project and docs : (https://npmjs.org/package/node-binance-api)

New features:

 - Get Fiat deposit history (credit card / Cash balance)
 - Get Stacking history
 - Get Stacking position
 - Get Flexible Stacking position
 - Get NFT transaction history


This project is designed to help you make your own projects that interact with the [Binance API](https://github.com/binance-exchange/binance-official-api-docs). You can stream candlestick chart data, market depth, or use other advanced features such as setting stop losses and iceberg orders. This project seeks to have complete API coverage including WebSockets and then you can get the fiat deposit history and also the stacking history.


#### Installation: **`npm install -s updated-node-binance-api`**

#### Getting started
```javascript
const Binance = require('updated-node-binance-api');
const binance = new Binance().options({
  APIKEY: '<key>',
  APISECRET: '<secret>'
});
```

# Updated Node Binance API (new feature)

#### Getting Fiat deposit history
```js

const params = { "transactionType": 0, "beginTime": 1514761200000, "endTime": 1675401553952 };
// transactionType: number (0 = deposit || transactionType: 1 = withdraw);
// beginTime: timestamp;
// endTime: timestamp;

// For all params please refere to : https://binance-docs.github.io/apidocs/spot/en/#get-fiat-deposit-withdraw-history-user_data


binance.fiatDepositHistory((error, data) => {
    console.info(`My deposit history from ${params.beginTime} to ${params.endTime}: `, data);
}, params);

```
<details>
 <summary>View Response</summary>

```js
{
    "code": "000000",
    "message": "success",
    "data": [
        {
            "orderNo": "11954f0208cf8987937da46ece761f6a",
            "sourceAmount": "500.0",
            "fiatCurrency": "EUR",
            "obtainAmount": "0.43522778",
            "cryptoCurrency": "ETH",
            "totalFee": "0.0",
            "price": "1146.53",
            "status": "Completed",
            "paymentMethod": "Cash Balance",
            "createTime": 1672146583000,
            "updateTime":1672146593000
        },{
            "orderNo": "f130a09ee0ef4e60b2692edf3c819dff",
            "sourceAmount": "500.0",
            "fiatCurrency": "USD",
            "obtainAmount": "2.17998657",
            "cryptoCurrency": "BNB",
            "totalFee": "0.0",
            "price": "229.359",
            "status": "Completed",
            "paymentMethod": "Credit Card",
            "createTime": 1672146521000,
            "updateTime": 1672146532000
        },{
            "orderNo": "N01285495125038081024110506",
            "sourceAmount": "200.0",
            "fiatCurrency": "EUR",
            "obtainAmount": "188.61631353",
            "cryptoCurrency": "BUSD",
            "totalFee": "3.91",
            "price": "1.01311491",
            "status": "Failed",
            "paymentMethod": "Credit Card",
            "createTime": 1667681091000,
            "updateTime": 1667681156386
        }
    ],
    "total": 3,
    "success": true
}
```
</details>


#### Getting Stacking history
```js

const params = { "product": "STACKING", "txnType": "INTEREST", "startTime": 1514761200000, "endTime": 1675401553952 };
// product: enum ('STAKING' -> Locked Staking; 'F_DEFI' -> flexible DeFi Staking, 'L_DEFI' -> locked DeFi Staking);
// txnType: enum ()"SUBSCRIPTION", "REDEMPTION", "INTEREST");
// startTime: timestamp;
// endTime: timestamp;
// current: number (page to get)
// the diff between "startTime" / "endTime" shouldn't exceed 3 month;

// For all params please refere to : https://binance-docs.github.io/apidocs/spot/en/#get-staking-history-user_data

binance.stackingHistory((error, data) => {
    console.info(`My stacking history (${params.product} and ${params.txnType}) from ${params.startTime} to ${params.endTime}: `, data);
}, params);

```
<details>
 <summary>View Response</summary>

```js
[
    {
        amount: "0.0005223",
        asset: "QTUM",
        positionId: 137781268,
        status: "SUCCESS",
        time: 1675385706000
    },{
        amount: "0.001043",
        asset: "BNB",
        positionId: 137581268,
        status: "SUCCESS",
        time: 1675385706000
    },{
        amount: "0.0000112",
        asset: "BTC",
        positionId: 297781268,
        status: "SUCCESS",
        time: 1675385706000,
    }
]

// Could have different return depend of the product and txnType :
// "project":"BSC", // DeFi Staking’s project
// "lockPeriod":"30",
// "deliverDate":"1575018510000",  // Redemption date
// "type":"AUTO", // display only for subscription
```
</details>




#### Getting Stacking position
```js

const params = { "product": "STACKING", "txnType": "INTEREST", "startTime": 1514761200000, "endTime": 1675401553952 };
// product: enum ('STAKING' -> Locked Staking; 'F_DEFI' -> flexible DeFi Staking, 'L_DEFI' -> locked DeFi Staking);
// startTime: timestamp;
// endTime: timestamp;
// current: number (page to get)

// For all params please refere to : https://binance-docs.github.io/apidocs/spot/en/#get-staking-product-position-user_data

binance.stackingPosition((error, data) => {
    console.info(`My stacking postion (${params.product} from ${params.startTime} to ${params.endTime}: `, data);
}, params);

```
<details>
 <summary>View Response</summary>

```js
[
  {
    "positionId":"123123",  //Staking position ID
    "projectId": "Axs*90",  //Staking project ID
    "asset":"AXS",          //Locked asset
    "amount":"122.09202928",  //Locked Amount
    "purchaseTime": "1646182276000",  //Subscription time
    "duration": "60",    //Lock period(days)
    "accrualDays": "4",  //Accrue days
    "rewardAsset":"AXS", //Earned asset
    "APY":"0.2032",
    "rewardAmt": "5.17181528",  //Earned amount
    "extraRewardAsset":"BNB",   //Rewards assets of extra staking type
    "extraRewardAPY":"0.0203",  //APY of extra staking type
    "estExtraRewardAmt": "5.17181528", //Rewards of extra staking type, distribute when order expires
    "nextInterestPay": "1.29295383",   //Next estimated interest payment
    "nextInterestPayDate": "1646697600000", //Next interest payment date
    "payInterestPeriod": "1",  //Interest cycle
    "redeemAmountEarly": "2802.24068892", //Early redemption amount
    "interestEndDate": "1651449600000",   //Interest accrual end date
    "deliverDate": "1651536000000",       //Redemption arrival time
    "redeemPeriod": "1",           //Redemption interval
    "redeemingAmt":"232.2323",     //Amount under redemption
    "partialAmtDeliverDate":"1651536000000", //Arrival time of partial redemption amount of order
    "canRedeemEarly": true,  //When it is true, early redemption can be operated
    "renewable"：true,  //When it is true, auto staking can be operated
    "type":"AUTO",   //Order type is auto-staking or normal
    "status": "HOLDING"
  }
]

// Could have different return depend of the product !
```
</details>




#### Getting Flexible Stacking position
```js

const params = { "startTime": 1514761200000, "endTime": 1675401553952 };
// startTime: timestamp;
// endTime: timestamp;

// For all params please refere to : https://binance-docs.github.io/apidocs/spot/en/#get-flexible-product-position-user_data

binance.flexibleStackingPosition((error, data) => {
    console.info(`My flexible stacking postion (from ${params.startTime} to ${params.endTime}: `, data);
}, params);

```
<details>
 <summary>View Response</summary>

```js
[
  {
    "tierAnnualInterestRate": {
      "0-5BTC": 0.05,
      "5-10BTC": 0.03,
      ">10BTC": 0.01
    },
    "annualInterestRate":"0.02599895",
    "asset": "USDT",
    "avgAnnualInterestRate": "0.02599895",
    "canRedeem": true,
    "dailyInterestRate": "0.00007123",
    "freeAmount": "75.46000000",
    "freezeAmount": "0.00000000", // abandoned
    "lockedAmount": "0.00000000", // abandoned
    "productId": "USDT001",
    "productName": "USDT",
    "redeemingAmount": "0.00000000",
    "todayPurchasedAmount": "0.00000000",
    "totalAmount": "75.46000000",
    "totalInterest": "0.22759183"
  }
]

```
</details>




#### Getting Nft Transaction history
```js

const params = { "orderType": 3, "startTime": 1514761200000, "endTime": 1675401553952, page: 1 };
// orderType: number (0: purchase order, 1: sell order, 2: royalty income, 3: primary market order, 4: mint fee);
// startTime: timestamp;
// endTime: timestamp;
// page: number; default = 1
// the diff between "startTime" / "endTime" shouldn't exceed 3 month (90 days);

// For all params please refere to : https://binance-docs.github.io/apidocs/spot/en/#get-nft-transaction-history-user_data

binance.nftTransactionHistory((error, data) => {
    console.info(`My Nft Transaction history (for market order from ${params.startTime} to ${params.endTime}: `, data);
}, params);

```
<details>
 <summary>View Response</summary>

```js
{
  "total": 2,  //total records
  "list": [
    {
      "orderNo": "1_470502070600699904", // 0: purchase order, 1: sell order, 2: royalty income, 3: primary market order, 4: mint fee
      "tokens": [
        {
          "network": "BSC",  // NFT Network
          "tokenId": "216000000496",  // NFT Token ID
          "contractAddress": "MYSTERY_BOX0000087"  // NFT Contract Address
        }
      ],
      "tradeTime": 1626941236000,
      "tradeAmount": "19.60000000",
      "tradeCurrency": "BNB"
    },
    {
      "orderNo": "1_488306442479116288",
      "tokens": [
        {
          "network": "BSC",
          "tokenId": "132900000007",
          "contractAddress": "0xAf12111a592e408DAbC740849fcd5e68629D9fb6"
        }
      ],
      "tradeTime": 1631186130000,
      "tradeAmount": "192.00000000",
      "tradeCurrency": "BNB"
    }
  ]
}
```
</details>




## Contribution
- Give us a star :star:
- Fork and Clone! Awesome
