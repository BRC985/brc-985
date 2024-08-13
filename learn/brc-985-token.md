# brc-985-token

## deploy

```json
{ 
    "p": "brc-985-token",
    "op": "deploy",
    "tick": "memo",
    "max": "21000000",
    "lim": "1000"
}
```

| key  | Required | Description          |
| ---- | -------- | -------------------- |
| p    | yes      | Protocol name        |
| op   | yes      | Operation            |
| tick | yes      | Ticker               |
| max  | yes      | Maximum token supply |
| lim  | no       | Limit pre mint       |

1. Check the inscription's value according to the following rules. 
   - Check if the protocol name is `brc-985-token`. 
   - Checks if the token represented by **"tick"** exists in the token list; if it does, then the deploy inscription is invalid;
   - If `lim` is set, then `lim`  should not be greater than `max`. 

2. If the above checks pass, then the deploy inscription is valid. 

3. Update the token list, add the token information to the token list.

## mint

```json
{ 
    "p": "brc-985-token",
    "op": "mint",
    "tick": "memo",
    "amt": "1000"
}
```

| key  | Required | Description                                                  |
| ---- | -------- | ------------------------------------------------------------ |
| p    | yes      | Protocol name                                                |
| op   | yes      | Operation                                                    |
| tick | yes      | Ticker                                                       |
| amt  | yes      | Minting amount. If `lim` is set, then `amt` must be less than `lim` |

1. Check the inscription's value according to the following rules:

   - Check if the protocol name is `brc-985-token`;

   - Checks if the token represented by `tick` exists in the token list; if it does, then the deploy inscription is invalid;

   - If `lim` is set, then `lim`  should not be greater than `max`. 

2. If the above checks pass, then the deploy inscription is valid. 

3. Update the token list, add the token information to the token list.

## transfer

```json
{ 
    "p": "brc-985-token",
    "op": "transfer",
    "tick": "memo",
    "amt": "100"
}
```

| key  | Required | Description                            |
| ---- | -------- | -------------------------------------- |
| p    | yes      | Protocol name                          |
| op   | yes      | Operation                              |
| tick | yes      | Ticker                                 |
| amt  | yes      | The amount of tokens to be transferred |

**Note: The transfer of tokens is divided into two steps, first create the transfer inscription, then send the transfer inscription to the target address, and Each transfer inscription can only be used once. And the token's balance is divided into available balance and transferable balance.**

**Note: The transfer of tokens is only related to the transfer inscription, the transfer of the mint inscription (sending the mint inscription to others through UTXO) does not lead to a change in balance.**

1. Check the inscription's value according to the following rules:

   - Check if the protocol name is `brc-985-token`;

   - Check if the token represented by `tick` exists in the token list; if it does not, then the mint inscription is invalid;

   - Check if the available balance of the Owner of the transfer inscription is greater than `amt`; if it is less, then the transfer inscription is invalid.

2. If the above checks pass, then the mint inscription is valid;

3. Update the available balance of the Owner of the transfer inscription, subtract `amt`;

4. Update the transferable  balance of the Owner of the transfer  inscription, add `amt`.
