# TanglePay-DeepLink
This repo contains spec, tools for work with TanglePay deep link features in mobile, browser extension environment.

Demo [here](https://tanglepay.github.io/TanglePay-DeepLink/). 

## Supported environments
1. Invoke deep link on Android/iOS
2. Scan QR code contains TanglePay deep link

## API */send*
1. API signature
    >tanglepay://send/{iota_address}?value={value}&merchant={merchant}&item_desc={item_desc}&return_url={return_url}&network={network}
2. Behavior
    - Redirect users to Send page, auto fill the target IOTA address, amount, merchant name, item descriptions, etc.
    - After transfer completes, present user the transaction receipt and allow users to be redirected back tho the return uri specified by the caller, with a reference to message id for ease of verification.
3. Parameters

|Parameter   |Type    |Description|
|---------   |--------|-----------|
|iota_address|Required|Valid IOTA address in [Bech32 format](https://github.com/Wollac/protocol-rfcs/blob/master/text/0020-bech32-address-format/0020-bech32-address-format.md).|
|value       |Required|Numbers of IOTA tokens to be transfered, e.g. 2000000.
|unit        |Optional|Unit to use for value transfer, e.g. Mi=1,000Ki=1,000,000i. For more details please refer to [unit-converter](https://www.npmjs.com/package/@iota/unit-converter).
|merchant    |Optional|Merchant name, to be displayed on TanglePay send page.
|item_desc   |Optional|Item description, to be displayed on TanglePay send page.
|return_url  |Optional|Encoded URI string. When this is provided, the link will be displayed on TanglePay after send completes to allow redirection of users.
|message_id  |Optional|If return_url is provided, message_id would be appended to the return url for ease of caller verification.
|network     |Optional|Netowrk to use. Allowed values are *mainnet* and *devnet*, with *mainnet* as the default value.
4. Restrictions
    - Send amount constraint. Due to limitations in IOTA Chrysalis 1.5, send may fail if 
        - The value to be transferred is less than 1MIOTA.
        - The remaining balance after value transfer is between 0 (exclusive) and 1 (exclusive) MIOTA. 

## API */dapp*
1. API signature
    >tanglepay://dapp/{dapp_name}?asset_id={asset_id}&network={network}
2. Behavior
    - Redirect user to Asset page, focus on the assets details specified by asset_id
3. Prerequisites
    - The dapp_name/asset_id should be pre-registered in TanglePay to enable integration of asset details display.
4. Parameters

|Parameter   |Type    |Description|
|---------   |--------|-----------|
|dapp_name   |Required|Unique identifier of dapp, e.g. IOTA, Shimmer.
|asset_id    |Optional|Asset_id to support multi-asset network.
|network     |Optional|Netowrk to use. Allowed values are *mainnet* and *devnet*, with *mainnet* as the default value.

## API */Sign*
1. API signature
    > tanglepay://sign?content={content}&return_url={return_url}&merchant={merchant_name}&network={network}
2. Behavior
    - Parse the sign content and present to users on TanglePay sign page.
    - After user confirms with password, send a signed message to the chain, which is similar as Stake/Vote operation on IOTA.
    - Upon success of sign, present user the sign receipt and allow users to be redirected back tho the return uri specified by the caller, with a reference to message id for ease of verification.
3. Sign payload on chain
    - Index: TanglePay.Sign
    - Content: Decoded content specified in API.
    - Example message: https://explorer.iota.org/devnet/message/d9bb6f4e6eb0e3ab01940a76535b984ab48fef3cb29158f98fbfe1af0536665b
3. Parameters

|Parameter   |Type    |Description|
|---------   |--------|-----------|
|content     |Required|Content to be sent as payload of signed message.
|merchant    |Optional|Merchant name, to be displayed on TanglePay send page.
|return_url  |Optional|Encoded URI string. When this is provided, the link will be displayed on TanglePay after send completes to allow redirection of users.
|message_id  |Optional|If return_url is provided, message_id would be appended to the return url for ease of caller verification.
|network     |Optional|Netowrk to use. Allowed values are *mainnet* and *devnet*, with *mainnet* as the default value.