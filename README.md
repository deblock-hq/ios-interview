# iOS Home Task for iOS positions

1. Create a new private repo and invite: @mario-deblock.
2. Start or upload your project.
3. We take into account:
  - Xcode SwiftUI full-lifecycle project/workspace (Bonus for workspace setup)
  - Bonus for iOS workspace with multiple targets to separate Core (platform and framework agnostic) module from SwiftUI (views and components) and Main modules
  - SOLID principles hands-on.
  - Most important thing, flexibility to understand this: "Don’t over-engineering! 
  - Model-View (MV) should be the natural way in SwiftUI.
  - Unit testing (core module).
  - Bonus: understanding of Composition Root concept.

## Requirements

### Design
![Screenshot 2023-09-01 at 14 34 54](https://github.com/deblock-hq/ios-interview/assets/115789674/2fc24d37-9fd3-4725-bde3-d55cbe6aa9ad)

[Figma file to follow colors, spacing, icons, etc here](https://www.figma.com/file/SaT9UfRVHwrcpCN8p1kVKl/Test-project?type=design&node-id=0%3A1&mode=design&t=gS6wgz3FuWBBJfqV-1)

### Features
A Deblock user has a crypto wallet. This crypto wallet contains ETH.
Assume the current user has a wallet balance of 10 ETH.

In this screen:
- As a user I can send ETH and select the amount without exceeding the wallet balance.
- As a user I can select a currency to display the equivalent amount in EUR, USD or GBP from a modal with these fixed options.
- As a user I can also switch the primary label of the component to be ETH or the selected currency (this will change the label displayed on the Send button too). As I type in this label an amount, it should convert the equivalent ETH<>FIAT
- As a user I can see the estimated network fees in ETH.

You will need to consume 2 data points:
1. Exchange rate (ETH to eur/gbp/usd)
2. Estimated network fees

#### 1. Obtaining Exchange rate

For the exchange rate, you can consume any free API, but here you have a working example you can use:

**Request**
```
https://api.coingecko.com/api/v3/simple/price?ids=ethereum&vs_currencies=usd
```

**Response**
```
{
  "ethereum": {
    "usd": 1644.1
  }
}
```

`vs_currencies` accepts `usd`, `eur` and `gbp` so you will be covered for this task :)

#### 2. Estimated Network Fees

For the ETH Network fees, you can use the following formula + API to obtain the data

**Formula**
```
Est Network fees = 21000 * {gasPrice} / 100000000
```

To obtain the current `{gasPrice}`:

**Request**
```
https://api.etherscan.io/api?module=gastracker&action=gasoracle&apikey=YourApiKeyToken
```
_If you dont have an apiToken you have a rate limit of 1/5 seconds._

**Response**
```
{"status":"1","message":"OK-Missing/Invalid API Key, rate limit of 1/5sec applied","result":{"LastBlock":"18041185","SafeGasPrice":"13","ProposeGasPrice":"14","FastGasPrice":"14","suggestBaseFee":"12.086368893","gasUsedRatio":"0.505199894363513,0.45067015395881,0.364857033333333,0.700552333333333,0.461159333333333"}}
```

Extract and use the `FastGasPrice` param.

So the estimated gas fee to be displayed is equal to
```
21000 * {FastGasPrice} / 100000000
```


## What we look for
- Be pixel perfect
- Animate the modal
- Clean code (networking, UI, simplicity)
- BE FAST EXECUTING.

