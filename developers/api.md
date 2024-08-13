# BRC 985 API

You can manage your BRC-985 assets using the API described in this document.

All APIs in the document use the unified address: http://119.147.213.61:38080.

## 1. Login

The API interface under the Log in module is used to manage user information and authorized application to access API. Users need to use the BTC Taproot address to log in.

### 1.1 Get Challenge Message

Get the challenge information that requires the user's signature.

**Request Address**

```
/v1/btc/challenge
```

**Request Method**

`GET`

**Header Parameters**

| parameter | required? | type   | description         |
| --------- | --------- | ------ | ------------------- |
| Origin    | yes       | string | website domain name |

**Path Parameters**

null

**Query Parameters**

| parameter | required? | type   | description          |
| --------- | --------- | ------ | -------------------- |
| address   | yes       | string | BTC address(Taproot) |

**Body**

null

**Response**

| parameter | type   | description       |
| --------- | ------ | ----------------- |
| message   | string | challenge message |

**Request Example**



**Response Example**



### 1.2 Login

Log in to the server to obtain access to all POST interfaces in the document.

**Request Address**

```
/v1/btc/login
```

**Request Method**

POST

**Header Parameters**

null

**Path Parameters**

null

**Query Parameters**

null

**Body**

| parameter | type   | description                                                  |
| --------- | ------ | ------------------------------------------------------------ |
| message   | string | challenge message                                            |
| signature | string | signature for challenge messages with the user's private key |

Signature's generation method can be referred to the [document](https://github.com/fivepiece/sign-verify-message/blob/master/signverifymessage.md). It can also be signed using BTC's signmessage interface.

**Response**

| parameter    | type   | description                          |
| ------------ | ------ | ------------------------------------ |
| accessToken  | string | token for accessing the API          |
| refreshToken | string | token for refreshing the accessToken |

**Request Example**



**Response Example**



### 1.3 Refresh Access Token

Refresh the access token.

**Request Address**

```
/v1/btc/refresh
```

**Request Method**

POST

**Header Parameters**

| parameter     | Value                     |
| ------------- | ------------------------- |
| Authorization | Bearer YOUR_REFRESH_TOKEN |

**Path Parameters**

null

**Query Parameters**

null

**Body**

null

**Response**

| paramenter  | type   | description                 |
| ----------- | ------ | --------------------------- |
| accessToken | string | token for accessing the API |

**Request Example**



**Response Example**



## 2. Inscription

The API interface under the Inscriptions module is used to query the inscription information.

### 2.1 Get Inscription Info

Get inscription info by inscriptionId.

**Request Address**

```
/v1/inscription/{inscriptionId}/info
```

**Request Method**

GET

**Header parameters**

null

**Path parameters**

| parameter     | type   | description           |
| ------------- | ------ | --------------------- |
| inscriptionId | string | inscription unique id |

**Query parameters**

null

**Body**

null

**Response**

| parameter       | type   | description      |
| --------------- | ------ | ---------------- |
| utxoInfo        | Object | UTXO info        |
| inscriptionInfo | Object | inscription info |

**structure of utxoInfo**

| parameter | type    | description                   |
| --------- | ------- | ----------------------------- |
| txid      | string  | transaction id                |
| vout      | integer | offset in transaction outputs |
| satoshi   | integer | output's balance              |
| scriptPk  | string  | lock script                   |
| address   | string  | owner's address               |

**structure of inscriptionInfo**

| parameter             | type    | description               |
| --------------------- | ------- | ------------------------- |
| inscriptionId         | string  | inscription's unique id   |
| commitTransactionHash | string  | commit transaction's hash |
| revealTransactionHash | string  | reveal transaction's hash |
| blockHeight           | integer | block height              |
| owner                 | integer | inscription's owner       |

**Request Example**



**Response Example**



### 2.2 Get Inscription Content

Get inscription content by inscriptionId.

**Request Address**

```
/v1/inscription/{inscriptionId}/content
```

**Request Method**

GET

**Header parameters**

null

**Path parameters**

| parameter     | type   | description           |
| ------------- | ------ | --------------------- |
| inscriptionId | string | inscription unique id |

**Query parameters**

null

**Body**

null

**Response(string)**

| parameter | type   | description         |
| --------- | ------ | ------------------- |
| content   | string | inscription content |

**Request Example**



**Response Example**



### 2.3 Get Address Inscription Info

Get list of inscription info by address.

**Request Address**

```
/v1/address/{address}/inscription/info
```

**Request Method**

GET

**Header parameters**

null

**Path parameters**

| parameter | type   | description          |
| --------- | ------ | -------------------- |
| address   | string | btc address(taproot) |

**Query parameters**

| parameter | required? | type    | description              |
| --------- | --------- | ------- | ------------------------ |
| start     | no        | integer | start offset(default: 0) |
| end       | no        | integer | end offset(default: 10)  |

**Body**

null

**Response**

| parameter | type           | description                      |
| --------- | -------------- | -------------------------------- |
| infos     | Arrary<Object> | a list of inscription info       |
| total     | integer        | total number of inscription info |

**structure of infos**

| parameter       | type   | description      |
| --------------- | ------ | ---------------- |
| utxoInfo        | Object | UTXO info        |
| inscriptionInfo | Object | inscription info |

**structure of utxoInfo**

| parameter | type    | description                   |
| --------- | ------- | ----------------------------- |
| txid      | string  | transactio id                 |
| vout      | integer | offset in transaction outputs |
| satoshi   | integer | output's balance              |
| scriptPk  | string  | lock script                   |
| address   | string  | owner's address               |

**structure of inscriptionInfo**

| parameter             | type    | description              |
| --------------------- | ------- | ------------------------ |
| inscriptionId         | string  | inscription's unique id  |
| commitTransactionHash | string  | commit transation's hash |
| revealTransactionHash | string  | reveal transation's hash |
| blockHeight           | integer | block height             |
| owner                 | integer | inscription's owner      |

**Request Example**



**Response Example**



### 2.4 Get UTXO Inscription Info

Get inscription info by utxo.

**Request Address**

```
/v1/output/{output}/inscription/info
```

**Request Method**

GET

**Header parameters**

null

**Path parameters**

| parameter | type   | description                                                  |
| --------- | ------ | ------------------------------------------------------------ |
| output    | string | transaction output, e.g. {cd8e555a9d988224f5de1a52ad17f988c65b15086b11ff5ebdf12bf5ccaa3246:0} |

**Query parameters**

null

**Body**

null

**Response**

| parameter       | type   | description      |
| --------------- | ------ | ---------------- |
| utxoInfo        | Object | UTXO info        |
| inscriptionInfo | Object | inscription info |

**structure of utxoInfo**

| parameter | type    | description                   |
| --------- | ------- | ----------------------------- |
| txid      | string  | transactio id                 |
| vout      | integer | offset in transaction outputs |
| satoshi   | integer | output's balance              |
| scriptPk  | string  | lock script                   |
| address   | string  | owner's address               |

**structure of inscriptionInfo**

| parameter             | type    | description              |
| --------------------- | ------- | ------------------------ |
| inscriptionId         | string  | inscription's unique id  |
| commitTransactionHash | string  | commit transation's hash |
| revealTransactionHash | string  | reveal transation's hash |
| blockHeight           | integer | block height             |
| owner                 | integer | inscription's owner      |

**Request Example**



**Response Example**



## 3. Fungible Token

The API interface under the Fungible Token module is used to manage the tokens held by users. 

### 3.1 Get FT List

Get the tiker list of FT.

**Request Address**

```
/v1/ft/list
```

**Request Method**

GET

**Header parameters**

null

**Path parameters**

null

**Query parameters**

| parameter | required? | type    | description              |
| --------- | --------- | ------- | ------------------------ |
| start     | no        | integer | start offset(Default: 0) |
| end       | no        | integer | end offset(Default: 10)  |

**Body**

null

**Response**

| parameter | type          | description             |
| --------- | ------------- | ----------------------- |
| ftInfos   | Array<Object> | list of FT info         |
| total     | integer       | total number of FT info |

**structure of ftInfos**

| parameter | type    | description                           |
| --------- | ------- | ------------------------------------- |
| tick      | string  | FT's ticker                           |
| max       | integer | total supply                          |
| limit     | integer | maximum mint amount for a single mint |
| minted    | integer | total minted amount                   |

**Request Example**



**Response Example**



### 3.2 Get Address FT Balance

Obtain FT balance by address, including available balance, transferable balance.

**Request Address**

```
/v1/address/{address}/ft/{tick}/balance
```

**Request Method**

GET

**Header parameters**

null

**Path parameters**

| parameter | type   | description          |
| --------- | ------ | -------------------- |
| address   | string | btc address(taproot) |
| tick      | string | FT ticker            |

**Query parameters**

null

**Body**

null

**Response**

| parameter           | type    | description          |
| ------------------- | ------- | -------------------- |
| tick                | string  | FT's ticker          |
| address             | string  | btc address(taproot) |
| overallBalance      | integer | overall balance      |
| transferableBalance | integer | transferable balance |
| availableBalance    | integer | available balance    |

**Request Example**



**Response Example**



### 3.3 Create FT Deploy Inscription

Create an order to inscribe Deploy Inscription.

**Request Address**

```
/v1/ft/deploy
```

**Request Method**

POST

**Header parameters**

| parameter     | Value                    |
| ------------- | ------------------------ |
| Content-Type  | application/json         |
| Authorization | Bearer YOUR_ACCESS_TOKEN |

**Path parameters**

null

**Query parameters**

null

**Body**

| parameter | required? | type    | description                           |
| --------- | --------- | ------- | ------------------------------------- |
| tick      | yes       | string  | FT ticker                             |
| max       | yes       | integer | total supply                          |
| limit     | yes       | integer | maximum mint amount for a single mint |

**Response**

| parameter             | type    | description                    |
| --------------------- | ------- | ------------------------------ |
| inscriptionId         | string  | deploy inscription's Unique id |
| commitTransactionHash | string  | commit transation's hash       |
| revealTransactionHash | string  | reveal transation's hash       |
| blockHeight           | integer | block height                   |
| owner                 | string  | deploy inscription's owner     |

**Request Example**



**Response Example**



### 3.4 Create FT Mint Inscription

Create an order to inscribe Mint Inscription.

**Request Address**

```
/v1/ft/mint
```

**Request Method**

POST

**Header parameters**

| parameter     | Value                    |
| ------------- | ------------------------ |
| Content-Type  | application/json         |
| Authorization | Bearer YOUR_ACCESS_TOKEN |

**Path parameters**

null

**Query parameters**

null

**Body**

| parameter | required? | type    | description |
| --------- | --------- | ------- | ----------- |
| tick      | yes       | string  | FT ticker   |
| amount    | yes       | integer | mint amount |

**Response**

| parameter             | type    | description                    |
| --------------------- | ------- | ------------------------------ |
| inscriptionId         | string  | deploy inscription's Unique id |
| commitTransactionHash | string  | commit transation's hash       |
| revealTransactionHash | string  | reveal transation's hash       |
| blockHeight           | integer | block height                   |
| owner                 | integer | deploy inscription's owner     |

**Request Example**



**Response Example**



### 3.5 Create FT Transfer Inscription

Create an order to inscribe Transfer Inscription.

**Request Address**

```
/v1/ft/transfer
```

**Request Method**

POST

**Header parameters**

| parameter     | Value                    |
| ------------- | ------------------------ |
| Content-Type  | application/json         |
| Authorization | Bearer YOUR_ACCESS_TOKEN |

**Path parameters**

null

**Query parameters**

null

**Body**

| parameter | required? | type    | description |
| --------- | --------- | ------- | ----------- |
| tick      | yes       | string  | FT ticker   |
| amount    | yes       | integer | mint amount |

**Response**

| parameter             | type    | description                    |
| --------------------- | ------- | ------------------------------ |
| inscriptionId         | string  | deploy inscription's Unique id |
| commitTransactionHash | string  | commit transation's hash       |
| revealTransactionHash | string  | reveal transation's hash       |
| blockHeight           | integer | block height                   |
| owner                 | integer | deploy inscription's owner     |

**Request Example**



**Response Example**



## 4. DA

### 4.1 Get DA List

Get the tiker list of Data Access System.

**Request Address**

```
/v1/da/list
```

**Request Method**

GET

**Header parameters**

null

**Path parameters**

null

**Query parameters**

| parameter | required? | type    | description              |
| --------- | --------- | ------- | ------------------------ |
| start     | no        | integer | start offset(Default: 0) |
| end       | no        | integer | end offset(Default: 10)  |

**Body**

null

**Response**

| parameter | type          | description             |
| --------- | ------------- | ----------------------- |
| daInfos   | Array<Object> | list of DA info         |
| total     | integer       | total number of DA info |

**structure of daInfos**

| parameter  | type    | description                                          |
| ---------- | ------- | ---------------------------------------------------- |
| tick       | string  | Data Access system's ticker                          |
| storage    | string  | storage address(who publish proof of data access)    |
| foundation | string  | foundation address(collect profit when proof failed) |
| interval   | integer | Cycle time to submit proof (sec)                     |
| token      | string  | accept FT                                            |
| price      | integer | Fees to be paid for a single upload                  |

**Request Example**



**Response Example**



### 4.2 Get Address Data Hold

Get data(store in DA) hold by address.

**Request Address**

```
/v1/address/{address}/da/{tick}/hold
```

**Request Method**

GET

**Header parameters**

null

**Path parameters**

| parameter | type   | description          |
| --------- | ------ | -------------------- |
| address   | string | btc address(taproot) |
| tick      | string | DA ticker            |

**Query parameters**

| parameter | required? | type    | description              |
| --------- | --------- | ------- | ------------------------ |
| start     | no        | integer | start offset(default: 0) |
| end       | no        | integer | end offset(default: 10)  |

**Body**

null

**Response**

| parameter | type          | description               |
| --------- | ------------- | ------------------------- |
| dataInfos | Array<Object> | list of data info         |
| total     | integer       | total number of data info |

**structure of dataInfos**

| parameter     | type   | description                    |
| ------------- | ------ | ------------------------------ |
| tick          | string | DA's ticker                    |
| id            | string | data commitment                |
| inscriptionId | string | Upload inscription's unique id |

**Request Example**



**Response Example**



### 4.3 Create Upload Request

Create a request to uploading data to da.

**Request Address**

```
/v1/da/upload/create
```

**Request Method**

POST

**Header parameters**

| parameter     | Value                    |
| ------------- | ------------------------ |
| Content-Type  | application/json         |
| Authorization | Bearer YOUR_ACCESS_TOKEN |

**Path parameters**

null

**Query parameters**

null

**Body**

| parameter | required? | type   | description         |
| --------- | --------- | ------ | ------------------- |
| data      | yes       | string | base64 encoded data |

**Response**

| parameter | type   | description     |
| --------- | ------ | --------------- |
| id        | string | data commitment |

**Request Example**



**Response Example**



### 4.4 Confirm Upload Request

Comfirm the upload request to upload the data to the DA.

**Request Address**

```
/v1/da/upload/create
```

**Request Method**

POST

**Header parameters**

| parameter     | Value                    |
| ------------- | ------------------------ |
| Content-Type  | application/json         |
| Authorization | Bearer YOUR_ACCESS_TOKEN |

**Path parameters**

null

**Query parameters**

null

**Body**

| parameter | required? | type   | description                                  |
| --------- | --------- | ------ | -------------------------------------------- |
| id        | yes       | string | data commitment                              |
| signature | yes       | string | signature for id with the user's private key |

the signature method is as follows:

- Original message: sort the `id` in dictionary order and concatenate in the form of key=value. For example: `id=xxx`.
- The original message is hashed twice using SHA256 to generate the message digest `h`.
- `signature` is generated by signing the message digest `h` with the private key of Creator using the ECDSA secp256k1 algorithm, and encoded in base64.

**Response**

| parameter             | type    | description                    |
| --------------------- | ------- | ------------------------------ |
| inscriptionId         | string  | upload inscription's unique id |
| commitTransactionHash | string  | commit transation's hash       |
| revealTransactionHash | string  | reveal transation's hash       |
| blockHeight           | integer | block height                   |
| owner                 | string  | upload inscription's owner     |

**Request Example**



**Response Example**



### 4.5 **Download From DA**

Download data from Data Access System.

**Request Address**

```
/v1/da/download
```

**Request Method**

GET

**Header parameters**

null

**Path parameters**

null

**Query parameters**

| parameter | required? | type   | description     |
| --------- | --------- | ------ | --------------- |
| id        | yes       | string | data commitment |

**Body**

null

**Response**

| parameter     | type   | description                    |
| ------------- | ------ | ------------------------------ |
| tick          | string | DA's ticker                    |
| id            | string | data commitment                |
| inscriptionId | string | Upload inscription's unique id |

**Request Example**



**Response Example**



## 5. NFT

### 5.1 Get On-chain NFT Conlection List

Get the list of On-chain NFT collection.

**Request Address**

```
/v1/nft/list
```

**Request Method**

GET

**Header parameters**

null

**Path parameters**

null

**Query parameters**

| parameter | required? | type    | description              |
| --------- | --------- | ------- | ------------------------ |
| start     | no        | integer | start offset(Default: 0) |
| end       | no        | integer | end offset(Default: 10)  |

**Body**

null

**Response**

| parameter | type          | description                             |
| --------- | ------------- | --------------------------------------- |
| nftInfos  | Array<Object> | list of on-chain NFT collection info    |
| total     | integer       | total number of on-chain NFT collection |

**structure of nftInfos**

| parameter   | type   | description                                         |
| ----------- | ------ | --------------------------------------------------- |
| tick        | string | NFT's ticker                                        |
| da          | string | Data Access system's ticker                         |
| description | string | a description of the NFT collection                 |
| id          | string | the commitment of the picture of the NFT collection |
| deployer    | string | deployer address                                    |

**Request Example**



**Response Example**



### 5.2 Get Off-chain NFT Collection List

Get the list of off-chain NFT collection which create Deploy request but not submit all files.

**Request Address**

```
/v1/nft/offchain/list
```

**Request Method**

GET

**Header parameters**

null

**Path parameters**

null

**Query parameters**

| parameter | required? | type    | description              |
| --------- | --------- | ------- | ------------------------ |
| start     | no        | integer | start offset(Default: 0) |
| end       | no        | integer | end offset(Default: 10)  |

**Body**

null

**Response**

| parameter | type          | description                              |
| --------- | ------------- | ---------------------------------------- |
| nftInfos  | Array<Object> | list of off-chain NFT collection info    |
| total     | integer       | total number of off-chain NFT collection |

**structure of nftInfos**

| parameter   | type    | description                                         |
| ----------- | ------- | --------------------------------------------------- |
| tick        | string  | NFT's ticker                                        |
| da          | string  | Data Access system's ticker                         |
| description | string  | a description of the NFT collection                 |
| id          | string  | the commitment of the picture of the NFT collection |
| deployer    | string  | deployer address                                    |
| max         | integer | total supply nft                                    |
| uploaded    | integer | number of uploaded file                             |
| createTime  | integer | Deploy request creation time                        |

**Request Example**



**Response Example**



### 5.3 Get Address NFT Hold

Get NFT hold by address.

**Request Address**

```
/v1/address/{address}/nft/{tick}/hold
```

**Request Method**

GET

**Header parameters**

null

**Path parameters**

| parameter | type   | description          |
| --------- | ------ | -------------------- |
| address   | string | btc address(taproot) |
| tick      | string | NFT ticker           |

**Query parameters**

| parameter | required? | type    | description              |
| --------- | --------- | ------- | ------------------------ |
| start     | no        | integer | start offset(default: 0) |
| end       | no        | integer | end offset(default: 10)  |

**Body**

null

**Response**

| parameter | type          | description                       |
| --------- | ------------- | --------------------------------- |
| nfts      | Array<Object> | list of NFT hold by user          |
| total     | integer       | total number of NFTs hold by user |

**structure of nfts**

| parameter     | type   | description                      |
| ------------- | ------ | -------------------------------- |
| tick          | string | NFT's ticker                     |
| id            | string | data commitment for NFT content  |
| inscriptionId | string | NFT Mint inscription's unique id |

**Request Example**



**Response Example**



### 5.4 Get Off-chain NFT Content List

Get a list of Off-chain NFT content.

**Request Address**

```
/v1/nft/offchain/{tick}/content/list
```

**Request Method**

GET

**Header parameters**

null

**Path parameters**

| parameter | type   | description |
| --------- | ------ | ----------- |
| tick      | string | NFT ticker  |

**Query parameters**

| parameter | required? | type    | description              |
| --------- | --------- | ------- | ------------------------ |
| start     | no        | integer | start offset(default: 0) |
| end       | no        | integer | end offset(default: 10)  |

**Body**

null

**Response**

| parameter | type          | description                              |
| --------- | ------------- | ---------------------------------------- |
| dataIds   | Array<string> | list of off-chain NFT content commitment |
| total     | integer       | total number of off-chain NFT content    |

**Request Example**



**Response Example**



### 5.5 Create NFT Deploy Inscription

Creating a deploy inscription is divided into 3 steps:

1. **Creating a deploy request**: uploading relevant information about the deploy inscription, such as information about the da used and the total number of NFTs issued;
2. **Batch upload data:** deployer batch uploads audited data (due to subsequent mint nft);
3. **Confirm The deploy request:** after the batch upload is complete, the deployer confirms the creation of the deploy inscription.

#### 5.5.1 Create Deploy Request

Create a request for creating deploy inscription.

**Request Address**

```
/v1/nft/deploy/create
```

**Request Method**

POST

**Header parameters**

| parameter     | Value                    |
| ------------- | ------------------------ |
| Content-Type  | application/json         |
| Authorization | Bearer YOUR_ACCESS_TOKEN |

**Path parameters**

null

**Query parameters**

null

**Body**

| parameter   | required? | type    | description                         |
| ----------- | --------- | ------- | ----------------------------------- |
| tick        | yes       | string  | NFT ticker                          |
| da          | yes       | integer | Specifies the NFT content location  |
| description | yes       | string  | a description of the NFT collection |
| data        | yes       | string  | a picture of the NFT collection     |
| max         | yes       | integer | total supply NFT                    |

**Response**

null

**Request Example**



**Response Example**



#### 5.5.2 Batch Upload NFT Content

Batch upload NFT content to server. Batch uploaded content will be needed for subsequent minting of NFTs.

**Request Address**

```
/v1/nft/deploy/batch_upload
```

**Request Method**

POST

**Header parameters**

| parameter     | Value                    |
| ------------- | ------------------------ |
| Content-Type  | multipart/form-data      |
| Authorization | Bearer YOUR_ACCESS_TOKEN |

**Path parameters**

null

**Query parameters**

null

**Body(form-data)**

| parameter | required? | type         | description      |
| --------- | --------- | ------------ | ---------------- |
| tick      | yes       | string       | NFT ticker       |
| files     | yes       | Arrary<file> | list of nft data |

**Response**

| parameter | type           | description                    |
| --------- | -------------- | ------------------------------ |
| dataIds   | Arrary<string> | collection of data commitments |

**Request Example**



**Response Example**



#### 5.5.3 Confirm Deploy Request

Confirm the deploy request when the deployer has uploaded all the NFT data..

**Request Address**

```
/v1/nft/deploy/confirm
```

**Request Method**

POST

**Header parameters**

| parameter     | Value                    |
| ------------- | ------------------------ |
| Content-Type  | application/json         |
| Authorization | Bearer YOUR_ACCESS_TOKEN |

**Path parameters**

null

**Query parameters**

null

**Body**

| parameter | required? | type   | description                                  |
| --------- | --------- | ------ | -------------------------------------------- |
| tick      | yes       | string | NFT ticker                                   |
| id        | yes       | string | NFT collection's content commitment          |
| signature | yes       | string | signature for id with the user's private key |

the signature method is as follows:

- Original message: sort the `id` in dictionary order and concatenate in the form of key=value. For example: `id=xxx`.
- The original message is hashed twice using SHA256 to generate the message digest `h`.
- `signature` is generated by signing the message digest `h` with the private key of Creator using the ECDSA secp256k1 algorithm, and encoded in base64.

**Response**

| parameter             | type    | description                    |
| --------------------- | ------- | ------------------------------ |
| inscriptionId         | integer | deploy inscription's unique id |
| commitTransactionHash | string  | commit transaction's hash      |
| revealTransactionHash | string  | reveal transaction's hash      |
| blockHeight           | integer | block height                   |
| owner                 | integer | deploy inscription's owner     |

**Request Example**



**Response Example**



### 5.6 Create NFT Mint Inscription

Create an order to inscribe Mint Inscription.

**Request Address**

```
/v1/nft/mint
```

**Request Method**

POST

**Header parameters**

| parameter     | Value                    |
| ------------- | ------------------------ |
| Content-Type  | application/json         |
| Authorization | Bearer YOUR_ACCESS_TOKEN |

**Path parameters**

null

**Query parameters**

null

**Body**

| parameter | required? | type    | description                                  |
| --------- | --------- | ------- | -------------------------------------------- |
| tick      | yes       | string  | NFT ticker                                   |
| id        | yes       | integer | NFT content commitment                       |
| signature | yes       | string  | signature for id with the user's private key |

the signature method is as follows:

- Original message: sort the `id` in dictionary order and concatenate in the form of key=value. For example: `id=xxx`.
- The original message is hashed twice using SHA256 to generate the message digest `h`.
- `signature` is generated by signing the message digest `h` with the private key of Creator using the ECDSA secp256k1 algorithm, and encoded in base64.

**Response**

| parameter             | type    | description                  |
| --------------------- | ------- | ---------------------------- |
| inscriptionId         | string  | mint inscription's Unique id |
| commitTransactionHash | string  | commit transaction's hash    |
| revealTransactionHash | string  | reveal transaction's hash    |
| blockHeight           | integer | block height                 |
| owner                 | integer | deploy inscription's owner   |

**Request Example**



**Response Example**



## 6. Market

### 6.1 FT Market

#### 6.1.1 Get FT List

Get the list of FT information.

**Request Address**

```
/v1/market/ft/list
```

**Request Method**

GET

**Path parameters**

null

**Query parameters**

| parameter | required? | type    | description                                                  |
| --------- | --------- | ------- | ------------------------------------------------------------ |
| start     | no        | integer | start offset(Default: 0)                                     |
| end       | no        | integer | end offset(Default: 10)                                      |
| order     | no        | string  | Sorting rules (the default is volum_sec, sorting by total transaction value from high to low) |

**Optional order:**

1. volum_sec: sorting by total transaction value from high to low
2. volum_dsec: sorting by total transaction value from low to high
3. listed_sec: sorting by number of orders from high to low
4. listed_dsec: sorting by number of orders from low to high
5. price_sec: sorting by floor price from high to low
6. price_dsec: sorting by floor price from low to high

**Body**

null

**Response**

| parameter | type          | description     |
| --------- | ------------- | --------------- |
| ftInfos   | Array<Object> | list of FT info |

**structure of ftInfos**

| parameter      | type    | description                            |
| -------------- | ------- | -------------------------------------- |
| tick           | string  | FT's ticker                            |
| max            | integer | total supply                           |
| limit          | integer | minimum mint amount for a single mint  |
| minted         | integer | total minted amount                    |
| listed         | integer | the number of FT orders                |
| transactions   | integer | the number of transactions             |
| holders        | integer | the number of addresses holding the FT |
| volume         | integer | total volume                           |
| capitalization | integer | market capitalization                  |
| floorPrice     | integer | floor price（sats/token）              |

**Request Example**



**Response Example**



#### 6.1.2 Get FT Sale Orders

Get a list of on sale ft orders.

**Request Address**

```
/v1/market/ft/{tick}/order/list
```

**Request Method**

GET

**Path parameters**

| parameter | type   | description |
| --------- | ------ | ----------- |
| tick      | string | FT ticker   |

**Query parameters**

| parameter | required? | type    | description                                                  |
| --------- | --------- | ------- | ------------------------------------------------------------ |
| start     | no        | integer | start offset(Default: 0)                                     |
| end       | no        | integer | end offset(Default: 10)                                      |
| order     | no        | string  | Sorting rules (the default is price_dsec, sorting by  average price of pre token from low to high) |

**Optional order:**

1. price_sec: sorting by average price of pre token from high to low
2. price_dsec: sorting by  average price of pre token from low to high
3. amount_sec: sorting by number of tokens from high to low
4. amount_dsec: sorting by number of tokens from low to high
5. recent: sorting by order of listing time

**Body**

null

**Response**

| parameter  | type           | description        |
| ---------- | -------------- | ------------------ |
| orderInfos | Arrary<Object> | list of order info |

**structure of orderInfos**

| parameter     | type    | description                       |
| ------------- | ------- | --------------------------------- |
| tick          | string  | FT ticker                         |
| orderId       | string  | FT sale order id                  |
| inscriptionId | string  | FT transfer inscription unique id |
| seller        | string  | seller address                    |
| amount        | integer | sale amount                       |
| price         | integer | sale price                        |

**Request Example**



**Response Example**



#### 6.1.3 **Get Address FT Sale Orders**

Get a list of on sale ft orders associated with an address.

**Request Address**

```
/v1/market/address/{address}/ft/{tick}/order/list
```

**Request Method**

GET

**Path parameters**

| parameter | type   | description |
| --------- | ------ | ----------- |
| tick      | string | FT ticker   |
| address   | string | BTC address |

**Query parameters**

| parameter | required? | type    | description                                                  |
| --------- | --------- | ------- | ------------------------------------------------------------ |
| start     | no        | integer | start offset(Default: 0)                                     |
| end       | no        | integer | end offset(Default: 10)                                      |
| order     | no        | string  | sorting rules (the default is recent, sorting by order of listing time) |

**Optional order:**

1. price_sec: sorting by average price of pre token from high to low

2. price_dsec: sorting by  average price of pre token from low to high

3. amount_sec: sorting by number of tokens from high to low

4. amount_dsec: sorting by number of tokens from low to high

5. recent: sorting by order of listing time

**Body**

null

**Response**

| parameter  | type           | description        |
| ---------- | -------------- | ------------------ |
| orderInfos | Arrary<Object> | list of order info |

**structure of orderInfos**

| parameter     | type    | description                       |
| ------------- | ------- | --------------------------------- |
| tick          | string  | FT ticker                         |
| orderId       | string  | FT sale order id                  |
| inscriptionId | string  | FT transfer inscription unique id |
| seller        | string  | seller address                    |
| amount        | integer | sale amount                       |
| price         | integer | sale price                        |

**Request Example**



**Response Example**



#### 6.1.4 Get FT transaction History List

Get a list of FT transaction history.

**Request Address**

```
/v1/market/ft/{tick}/transaction/list
```

**Request Method**

GET

**Path parameters**

| parameter | type   | description |
| --------- | ------ | ----------- |
| tick      | string | FT ticker   |

**Query parameters**

| parameter | required? | type    | description              |
| --------- | --------- | ------- | ------------------------ |
| start     | no        | integer | start offset(Default: 0) |
| end       | no        | integer | end offset(Default: 10)  |

**Body**

null

**Response**

| parameter        | type           | description              |
| ---------------- | -------------- | ------------------------ |
| transactionInfos | Arrary<Object> | list of transaction info |

**structure of orderInfos**

| parameter     | type    | description                 |
| ------------- | ------- | --------------------------- |
| tick          | string  | FT ticker                   |
| from          | integer | seller address              |
| to            | integer | buyer address               |
| amount        | integer | FT amount                   |
| price         | integer | price                       |
| time          | integer | transaction completion time |
| transactionId | string  | transaction ID              |

**Request Example**



**Response Example**



#### 6.1.5 Get FT transaction History List

Get a list of FT transaction history associated with an address.

**Request Address**

```
/v1/market/address/{address}/ft/{tick}/transaction/list
```

**Request Method**

GET

**Path parameters**

| parameter | type   | description |
| --------- | ------ | ----------- |
| tick      | string | FT ticker   |
| address   | string | BTC address |

**Query parameters**

| parameter | required? | type    | description              |
| --------- | --------- | ------- | ------------------------ |
| start     | no        | integer | start offset(Default: 0) |
| end       | no        | integer | end offset(Default: 10)  |

**Body**

null

**Response**

| parameter        | type           | description              |
| ---------------- | -------------- | ------------------------ |
| transactionInfos | Arrary<Object> | list of transaction info |

**structure of orderInfos**

| parameter     | type    | description                 |
| ------------- | ------- | --------------------------- |
| tick          | string  | FT ticker                   |
| from          | integer | seller address              |
| to            | integer | buyer address               |
| amount        | integer | FT amount                   |
| price         | integer | price                       |
| time          | integer | transaction completion time |
| transactionId | string  | transaction ID              |

**Request Example**



**Response Example**



#### 6.1.6 Create Sale Orders

Create sale order for chosen ft Transfer inscription. Before calling this interface, you need to call FT's Transfer interface.

**Request Address**

```
/v1/market/ft/order/sell/create
```

**Request Method**

POST

**Header parameters**

| parameter     | Value                    |
| ------------- | ------------------------ |
| Content-Type  | application/json         |
| Authorization | Bearer YOUR_ACCESS_TOKEN |

**Path parameters**

null

**Query parameters**

null

**Body**

| parameter       | required? | type    | description                       |
| --------------- | --------- | ------- | --------------------------------- |
| inscriptionId   | yes       | string  | FT transfer inscription unique id |
| price           | yes       | integer | FT price                          |
| receivedAddress | yes       | string  | address to receive BTC            |

**Response**

| parameter | type           | description                      |
| --------- | -------------- | -------------------------------- |
| orderId   | string         | FT sale order id                 |
| psbt      | string         | base64 format psbt               |
| signIndex | Array<integer> | Specifies the signature location |

**Request Example**



**Response Example**



#### 6.1.7 Confirm Sale Order

Confirm sale order for chosen ft Transfer inscription. You should sign in the psbt with `SIGHASH_SIGNAL | SIGHASH_ANYONECANPAY`.

**Request Address**

```
/v1/market/ft/order/sell/confirm
```

**Request Method**

POST

**Path parameters**

null

**Query parameters**

null

**Body**

| parameter | required? | type    | description                          |
| --------- | --------- | ------- | ------------------------------------ |
| orderId   | yes       | string  | FT sale order id                     |
| psbt      | yes       | integer | base64 format psbt, signed by seller |

**Response**

null

**Request Example**



**Response Example**



#### 6.1.8 Create Purchase Order

Create purchase order for chosen ft Transfer inscription. 

**Request Address**

```
/v1/market/ft/order/purchase/create
```

**Request Method**

POST

**Path parameters**

null

**Query parameters**

null

**Body**

| parameter       | required? | type    | description           |
| --------------- | --------- | ------- | --------------------- |
| orderId         | yes       | string  | FT sale order id      |
| paidAddress     | yes       | integer | address to pay BTC    |
| receivedAddress | yes       | string  | address to receive FT |

**Response**

| parameter | type           | description                      |
| --------- | -------------- | -------------------------------- |
| orderId   | string         | FT sale order id                 |
| psbt      | string         | base64 format psbt               |
| signIndex | Array<integer> | Specifies the signature location |

**Request Example**



**Response Example**



#### 6.1.9 Confirm Purchase Order

Confirm purchase order for chosen ft transfer inscription. You should sign in the psbt with SIGHASH_ALL.

**Request Address**

```
/v1/market/ft/order/purchase/confirm
```

**Request Method**

POST

**Path parameters**

null

**Query parameters**

null

**Body**

| parameter | required? | type    | description                          |
| --------- | --------- | ------- | ------------------------------------ |
| orderId   | yes       | string  | nft sale order id                    |
| psbt      | yes       | integer | base64 format psbt, signed by seller |

**Response**

null

**Request Example**



**Response Example**



#### 6.1.10 Create Change Price Order

Create an order to change the price of chosen ft transfer inscription. 

**Request Address**

```
/v1/market/ft/order/modify/create
```

**Request Method**

POST

**Path parameters**

null

**Query parameters**

null

**Body**

| parameter       | required? | type    | description            |
| --------------- | --------- | ------- | ---------------------- |
| orderId         | yes       | string  | FT sale order id       |
| newPrice        | yes       | integer | new sale price         |
| receivedAddress | yes       | string  | address to receive BTC |

**Response**

| parameter | type           | description                      |
| --------- | -------------- | -------------------------------- |
| orderId   | string         | FT sale order id                 |
| psbt      | string         | base64 format psbt               |
| signIndex | Array<integer> | Specifies the signature location |

**Request Example**



**Response Example**



#### 6.1.11 Confirm Change Price Order

Confirm change price order for chosen ft transfer inscription. You should sign in the psbt with SIGHASH_SIGNAL | SIGHASH_ANYONECANPAY.

**Request Address**

```
/v1/market/ft/order/modify/confirm
```

**Request Method**

POST

**Path parameters**

null

**Query parameters**

null

**Body**

| parameter | required? | type    | description                          |
| --------- | --------- | ------- | ------------------------------------ |
| orderId   | yes       | string  | NFT sale order id                    |
| psbt      | yes       | integer | base64 format psbt, signed by seller |

**Response**

null

**Request Example**



**Response Example**



#### 6.1.12 Create Cancellation  Order

Create a cancellation order to take down a sale order. Subsequently the seller needs to sign the rawTransaction. The rawTransaction will transfer the FT transfer inscription to your address.

**Request Address**

```
/v1/market/ft/order/cancel/create
```

**Request Method**

POST

**Path parameters**

null

**Query parameters**

null

**Body**

| parameter | required? | type   | description      |
| --------- | --------- | ------ | ---------------- |
| orderId   | yes       | string | FT sale order id |

**Response**

| parameter      | type   | description                        |
| -------------- | ------ | ---------------------------------- |
| orderId        | string | FT sale order id                   |
| rawTransaction | string | base64 format unsigned transaction |

**Request Example**



**Response Example**



#### 6.1.13 Confirm Cancellation  Order

Confirm a cancellation order to take down a sale order. 

**Request Address**

```
/v1/market/ft/order/cancel/confirm
```

**Request Method**

POST

**Path parameters**

null

**Query parameters**

null

**Body**

| parameter      | required? | type   | description                      |
| -------------- | --------- | ------ | -------------------------------- |
| orderId        | yes       | string | FT sale order id                 |
| rawTransaction | yes       | string | base64 format signed transaction |

**Response**

null

**Request Example**



**Response Example** 



### 6.2 NFT Market

#### 6.2.1 **Get NFT List**

Get the list of FT information.

**Request Address**

```
/v1/market/nft/list
```

**Request Method**

GET

**Path parameters**

null

**Query parameters**

| parameter | required? | type    | description                                                  |
| --------- | --------- | ------- | ------------------------------------------------------------ |
| start     | no        | integer | start offset(Default: 0)                                     |
| end       | no        | integer | end offset(Default: 10)                                      |
| order     | no        | string  | Sorting rules (the default is volum_sec, sorting by total transaction value from high to low) |

**Optional order:**

1. volum_sec: sorting by total transaction value from high to low

2. volum_dsec: sorting by total transaction value from low to high
3. listed_sec: sorting by number of orders from high to low
4. listed_dsec: sorting by number of orders from low to high
5. price_sec: sorting by floor price from high to low
6. price_dsec: sorting by floor price from low to high

**Body**

null

**Response**

| parameter | type          | description      |
| --------- | ------------- | ---------------- |
| ftInfos   | Array<Object> | list of NFT info |

**stuct of ftInfos**

| parameter    | type    | description                             |
| ------------ | ------- | --------------------------------------- |
| tick         | string  | NFT's ticker                            |
| max          | integer | total supply                            |
| minted       | integer | total minted amount                     |
| listed       | integer | the number of NFT orders                |
| transactions | integer | the number of transactions              |
| holders      | integer | the number of addresses holding the NFT |
| volume       | integer | total volume                            |
| floorPrice   | integer | floor price（sats/token）               |

**Request Example**



**Response Example**



#### 6.2.2 Get NFT Sale Orders

Get a list of on sale nft orders.

**Request Address**

```
/v1/market/nft/{tick}/order/list
```

**Request Method**

GET

**Path parameters**

| parameter | type   | description |
| --------- | ------ | ----------- |
| tick      | string | NFT ticker  |

**Query parameters**

| parameter | required? | type    | description                                                  |
| --------- | --------- | ------- | ------------------------------------------------------------ |
| start     | no        | integer | start offset(Default: 0)                                     |
| end       | no        | integer | end offset(Default: 10)                                      |
| order     | no        | string  | Sorting rules (the default is price_dsec, sorting by total transaction value from high to low) |

**Optional order:**

1. price_sec: sorting by average price of pre token from high to low
2. price_dsec: sorting by  average price of pre token from low to high
3. recent: sorting by order of listing time

**Body**

null

**Response**

| parameter  | type           | description        |
| ---------- | -------------- | ------------------ |
| orderInfos | Arrary<Object> | list of order info |

**structure of orderInfos**

| parameter     | type    | description                    |
| ------------- | ------- | ------------------------------ |
| tick          | string  | NFT ticker                     |
| orderId       | string  | NFT sale order id              |
| inscriptionId | string  | NFT mint inscription Unique ID |
| seller        | string  | seller address                 |
| price         | integer | sale price                     |

**Request Example**



**Response Example**



#### 6.2.3 Get Address NFT Sale Orders

Get a list of on sale nft orders associated with an address.

**Request Address**

```
/v1/market/address/{address}/nft/{tick}/order/list
```

**Request Method**

GET

**Path parameters**

| parameter | type   | description |
| --------- | ------ | ----------- |
| tick      | string | NFT ticker  |
| address   | string | BTC address |

**Query parameters**

| parameter | required? | type    | description              |
| --------- | --------- | ------- | ------------------------ |
| start     | no        | integer | start offset(Default: 0) |
| end       | no        | integer | end offset(Default: 10)  |

**Optional order:**

1.  price_sec: sorting by price of NFT from high to low
2. price_dsec: sorting by  price of NFT from low to high
3. recent: sorting by order of listing time

**Body**

null

**Response**

| parameter  | type           | description        |
| ---------- | -------------- | ------------------ |
| orderInfos | Arrary<Object> | list of order info |

**structure of orderInfos**

| parameter     | type    | description                    |
| ------------- | ------- | ------------------------------ |
| tick          | string  | NFT ticker                     |
| orderId       | string  | NFT sale order id              |
| inscriptionId | string  | NFT mint inscription Unique ID |
| seller        | string  | seller address                 |
| price         | integer | sale price                     |

**Request Example**



**Response Example**



#### 6.2.4 Get NFT transaction History List

Get a list of NFT transaction history.

**Request Address**

```
/v1/market/nft/{tick}/transaction/list
```

**Request Method**

GET

**Path parameters**

| parameter | type   | description |
| --------- | ------ | ----------- |
| tick      | string | NFT ticker  |

**Query parameters**

| parameter | required? | type    | description              |
| --------- | --------- | ------- | ------------------------ |
| start     | no        | integer | start offset(Default: 0) |
| end       | no        | integer | end offset(Default: 10)  |

**Body**

null

**Response**

| parameter        | type           | description              |
| ---------------- | -------------- | ------------------------ |
| transactionInfos | Arrary<Object> | list of transaction info |

**structure of orderInfos**

| parameter     | type    | description                    |
| ------------- | ------- | ------------------------------ |
| tick          | string  | NFT ticker                     |
| from          | integer | seller address                 |
| to            | integer | buyer address                  |
| inscriptionId | string  | NFT mint inscription Unique ID |
| price         | integer | price                          |
| time          | integer | transaction completion time    |
| transactionId | string  | transaction ID                 |

**Request Example**



**Response Example**



#### 6.2.5 Get Address NFT transaction History List

Get a list of NFT transaction history.

**Request Address**

```
/v1/market/address/{address}/nft/{tick}/transaction/list
```

**Request Method**

GET

**Path parameters**

| parameter | type   | description |
| --------- | ------ | ----------- |
| tick      | string | NFT ticker  |
| address   | string | BTC address |

**Query parameters**

| parameter | required? | type    | description              |
| --------- | --------- | ------- | ------------------------ |
| start     | no        | integer | start offset(Default: 0) |
| end       | no        | integer | end offset(Default: 10)  |

**Body**

null

**Response**

| parameter        | type           | description              |
| ---------------- | -------------- | ------------------------ |
| transactionInfos | Arrary<Object> | list of transaction info |

**structure of orderInfos**

| parameter     | type    | description                    |
| ------------- | ------- | ------------------------------ |
| tick          | string  | NFT ticker                     |
| from          | integer | seller address                 |
| to            | integer | buyer address                  |
| inscriptionId | string  | NFT mint inscription Unique ID |
| price         | integer | price                          |
| time          | integer | transaction completion time    |
| transactionId | string  | transaction ID                 |

**Request Example**



**Response Example**



#### 6.2.6 Create Sale Order

Create sale order for chosen nft. 

**Request Address**

```
/v1/market/nft/order/sell/create
```

**Request Method**

POST

**Header parameters**

| parameter     | Value                    |
| ------------- | ------------------------ |
| Content-Type  | application/json         |
| Authorization | Bearer YOUR_ACCESS_TOKEN |

**Path parameters**

null

**Query parameters**

null

**Body**

| parameter       | required? | type    | description                    |
| --------------- | --------- | ------- | ------------------------------ |
| inscriptionId   | yes       | string  | NFT mint inscription unique id |
| price           | yes       | integer | sale price                     |
| receivedAddress | yes       | string  | address to receive BTC         |

**Response**

| parameter | type           | description                      |
| --------- | -------------- | -------------------------------- |
| orderId   | string         | NFT sale order id                |
| psbt      | string         | base64 format psbt               |
| signIndex | Array<integer> | Specifies the signature location |

**Request Example**



**Response Example**



#### 6.2.7 Confirm Sale Order

Confirm sale order for chosen nft. You should sign in the psbt with SIGHASH_SIGNAL | SIGHASH_ANYONECANPAY.

**Request Address**

```
/v1/market/nft/order/sell/confirm
```

**Request Method**

POST

**Path parameters**

null

**Query parameters**

null

**Body**

| parameter | required? | type    | description                          |
| --------- | --------- | ------- | ------------------------------------ |
| orderId   | yes       | string  | NFT sale order id                    |
| psbt      | yes       | integer | base64 format psbt, signed by seller |

**Response**

null

**Request Example**



**Response Example**



#### 6.2.8 Create Purchase Order

Create purchase order for chosen nft. 

**Request Address**

```
/v1/market/nft/order/purchase/create
```

**Request Method**

POST

**Path parameters**

null

**Query parameters**

null

**Body**

| parameter       | required? | type    | description            |
| --------------- | --------- | ------- | ---------------------- |
| orderId         | yes       | string  | NFT sale order id      |
| paidAddress     | yes       | integer | address to pay BTC     |
| receivedAddress | yes       | string  | address to receive NFT |

**Response**

| parameter | type           | description                      |
| --------- | -------------- | -------------------------------- |
| orderId   | string         | NFT sale order id                |
| psbt      | string         | base64 format psbt               |
| signIndex | Array<integer> | Specifies the signature location |

**Request Example**



**Response Example**



#### 6.2.9 Confirm Purchase Order

Confirm purchase order for chosen nft. You should sign in the psbt with SIGHASH_ALL.

**Request Address**

```
/v1/market/nft/order/purchase/confirm
```

**Request Method**

POST

**Path parameters**

null

**Query parameters**

null

**Body**

| parameter | required? | type    | description                          |
| --------- | --------- | ------- | ------------------------------------ |
| orderId   | yes       | string  | NFT sale order id                    |
| psbt      | yes       | integer | base64 format psbt, signed by seller |

**Response**

null

**Request Example**



**Response Example**



#### 6.2.10 Create Change Price Order

Create an order to change the price of chosen nft. 

**Request Address**

```
/v1/market/nft/order/modify/create
```

**Request Method**

POST

**Path parameters**

null

**Query parameters**

null

**Body**

| parameter       | required? | type    | description            |
| --------------- | --------- | ------- | ---------------------- |
| orderId         | yes       | string  | NFT sale order id      |
| newPrice        | yes       | integer | new sale price         |
| receivedAddress | yes       | string  | address to receive BTC |

**Response**

| parameter | type           | description                      |
| --------- | -------------- | -------------------------------- |
| orderId   | string         | NFT sale order id                |
| psbt      | string         | base64 format psbt               |
| signIndex | Array<integer> | Specifies the signature location |

**Request Example**



**Response Example**



#### 6.2.11 Confirm Change Price Order

Confirm change price order for chosen nft. You should sign in the psbt with SIGHASH_SIGNAL | SIGHASH_ANYONECANPAY.

**Request Address**

```
/v1/market/nft/order/modify/confirm
```

**Request Method**

POST

**Path parameters**

null

**Query parameters**

null

**Body**

| parameter | required? | type    | description                          |
| --------- | --------- | ------- | ------------------------------------ |
| orderId   | yes       | string  | NFT sale order id                    |
| psbt      | yes       | integer | base64 format psbt, signed by seller |

**Response**

null

**Request Example**



**Response Example**



#### 6.2.12 Create Cancellation  Order

Create a cancellation order to take down a sale order. 

**Request Address**

```
/v1/market/nft/order/cancel/create
```

**Request Method**

POST

**Header parameters**

| parameter     | Value                    |
| ------------- | ------------------------ |
| Content-Type  | application/json         |
| Authorization | Bearer YOUR_ACCESS_TOKEN |

**Path parameters**

null

**Query parameters**

null

**Body**

| parameter | required? | type   | description       |
| --------- | --------- | ------ | ----------------- |
| orderId   | yes       | string | NFT sale order id |

**Response**

null

**Request Example**



**Response Example**