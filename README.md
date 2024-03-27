# Dogecoin Multisig Tutorial

In this tutorial we will create a 2-of-2 multisig address. Fund it and spend the Doges afterwards. One share will be send to a new address, and the change will be send back to the multisig address.

All commands can be executed in your Dogecoin Core Wallet's console.

## Preperation

To complete this tutorial you first need two address. Ususally they would exist in two different core walltes, but for this tutorial we will create everything within one wallet.
```
getnewaddress
nhKDjDDysUTooNMeXUUAMDiPnEHc2XPwaf

getnewaddress
nkk3yxFM8m3W1NABsTTLGcrVzLoiFBy63s
```

To sign the transactions afterwards we will need the private keys of those addresses. You can get them with this command

```
dumpprivkey nhKDjDDysUTooNMeXUUAMDiPnEHc2XPwaf
cgHU6xrBRWhFoxWwBgFs5JKLzGGjBd6UfW4RcU5NbEBLh5KEuozf

dumpprivkey nkk3yxFM8m3W1NABsTTLGcrVzLoiFBy63s
cmJieM35LY3EZ2QLZ8miAKJiiAxgDQf4PfbqzvFB7FBHPHM4toKd
```

## Create the Multisig address

With the next command you will create a multisig address for those addresses.

```
createmultisig 2 "[\"nhKDjDDysUTooNMeXUUAMDiPnEHc2XPwaf\", \"nkk3yxFM8m3W1NABsTTLGcrVzLoiFBy63s\"]"
{
  "address": "2NA25QXBJBbTouHL9LSx9n3TyDTS4kHMyZd",
  "redeemScript": "5221032708b38ce0a425185ac35dad3b906e2a0b6dc177da078515bc96f8aa535309782102fa4ee3e47ac7e61bcff3a82d4a223cedf736242ee3431e59ece2b883882c934452ae"
}
```

The redeemScript will be required for signing the transaction.

## Fund the address

To have some Doges to spend afterwards we will send 5 Doges to the multisig address.

```
sendtoaddress "2NA25QXBJBbTouHL9LSx9n3TyDTS4kHMyZd" 5
9a7745f180e0d6be8df7334922aa422a4d93ac33febfcd3281cfcb2332ac941c
```

We will need the hex of the vout's scriptPubKey for the signin later. So we do the following command to get the required data.

```
getrawtransaction 9a7745f180e0d6be8df7334922aa422a4d93ac33febfcd3281cfcb2332ac941c 1
{
  "hex": "0100000001b7617c52b1d4fe599f83bd4dab6260247d10f3f033f7a36548a4214e2a0555ee000000006a47304402206894ae2798cbdad9b14b6b0f070a9139f2b0b79fcaf40463c4942192ac3fc8c6022042114bd69d5bdafcd1555b28e9c6175b623b392ae5f7a50695d90c69714d50f30121037bae66b6519128759d9ea1defef2d894733209f3b83a85301f0e40d0ce176a8afeffffff020065cd1d0000000017a914b7fd911bc1a3e4be0c1d17f6f0e45bd3c42f30098700ed0399e80000001976a9143c77d1f442c6441c3a9502cc0e291c330e78eb4388ac999e5c00",
  "txid": "9a7745f180e0d6be8df7334922aa422a4d93ac33febfcd3281cfcb2332ac941c",
  "hash": "9a7745f180e0d6be8df7334922aa422a4d93ac33febfcd3281cfcb2332ac941c",
  "size": 223,
  "vsize": 223,
  "version": 1,
  "locktime": 6069913,
  "vin": [
    {
      "txid": "ee55052a4e21a44865a3f733f0f3107d246062ab4dbd839f59fed4b1527c61b7",
      "vout": 0,
      "scriptSig": {
        "asm": "304402206894ae2798cbdad9b14b6b0f070a9139f2b0b79fcaf40463c4942192ac3fc8c6022042114bd69d5bdafcd1555b28e9c6175b623b392ae5f7a50695d90c69714d50f3[ALL] 037bae66b6519128759d9ea1defef2d894733209f3b83a85301f0e40d0ce176a8a",
        "hex": "47304402206894ae2798cbdad9b14b6b0f070a9139f2b0b79fcaf40463c4942192ac3fc8c6022042114bd69d5bdafcd1555b28e9c6175b623b392ae5f7a50695d90c69714d50f30121037bae66b6519128759d9ea1defef2d894733209f3b83a85301f0e40d0ce176a8a"
      },
      "sequence": 4294967294
    }
  ],
  "vout": [
    {
      "value": 5.00000000,
      "n": 0,
      "scriptPubKey": {
        "asm": "OP_HASH160 b7fd911bc1a3e4be0c1d17f6f0e45bd3c42f3009 OP_EQUAL",
        "hex": "a914b7fd911bc1a3e4be0c1d17f6f0e45bd3c42f300987",
        "reqSigs": 1,
        "type": "scripthash",
        "addresses": [
          "2NA25QXBJBbTouHL9LSx9n3TyDTS4kHMyZd"
        ]
      }
    },
    {
      "value": 9989.99584000,
      "n": 1,
      "scriptPubKey": {
        "asm": "OP_DUP OP_HASH160 3c77d1f442c6441c3a9502cc0e291c330e78eb43 OP_EQUALVERIFY OP_CHECKSIG",
        "hex": "76a9143c77d1f442c6441c3a9502cc0e291c330e78eb4388ac",
        "reqSigs": 1,
        "type": "pubkeyhash",
        "addresses": [
          "nZhtM2gdg5fZjS2zQkj8K1do53dhJjx4sd"
        ]
      }
    }
  ],
  "blockhash": "8e290b3ab86d852b4fbf7cea9789f70985a56bf755aba27cf5412ee5b09a6efb",
  "confirmations": 43,
  "time": 1711569880,
  "blocktime": 1711569880
}
```

## Spend the funds

Now we want to spend 1 Dogecoin to a new address. To get the new address run the following command.
```
getnewaddress
nk1XmseegNw5pi3mHMmvzVk1gPUJfNhTX3
```

We want to send 1 Doge to this new address, 3 Doges back to the multisig address (change) and 1 Doge as mining fee. Be aware of the fact, that the order for this transaction is
- first 3 Doges back to the multisig
- second 1 Doge to the new address nk1XmseegNw5pi3mHMmvzVk1gPUJfNhTX3
- the fee is the total of our inputs (5) minus the sum of our outputs (4)

```
createrawtransaction '[{"txid":"9a7745f180e0d6be8df7334922aa422a4d93ac33febfcd3281cfcb2332ac941c","vout":0}]' '{"2NA25QXBJBbTouHL9LSx9n3TyDTS4kHMyZd":3,"nk1XmseegNw5pi3mHMmvzVk1gPUJfNhTX3":1}'
01000000011c94ac3223cbcf8132cdbffe33ac934d2a42aa224933f78dbed6e080f145779a0000000000ffffffff0200a3e1110000000017a914b7fd911bc1a3e4be0c1d17f6f0e45bd3c42f30098700e1f505000000001976a914ad7f64974fef9d7bf8950e607fbb43bf7226fb5c88ac00000000
```

Now it is important to understand that the first signer will sign the transaction (the private key of the first address of the multisig)
```
signrawtransaction '01000000011c94ac3223cbcf8132cdbffe33ac934d2a42aa224933f78dbed6e080f145779a0000000000ffffffff0200a3e1110000000017a914b7fd911bc1a3e4be0c1d17f6f0e45bd3c42f30098700e1f505000000001976a914ad7f64974fef9d7bf8950e607fbb43bf7226fb5c88ac00000000' '[{"txid":"9a7745f180e0d6be8df7334922aa422a4d93ac33febfcd3281cfcb2332ac941c","vout":0,"scriptPubKey":"a914b7fd911bc1a3e4be0c1d17f6f0e45bd3c42f300987","redeemScript":"5221032708b38ce0a425185ac35dad3b906e2a0b6dc177da078515bc96f8aa535309782102fa4ee3e47ac7e61bcff3a82d4a223cedf736242ee3431e59ece2b883882c934452ae"}]' '["cgHU6xrBRWhFoxWwBgFs5JKLzGGjBd6UfW4RcU5NbEBLh5KEuozf"]'
{
  "hex": "01000000011c94ac3223cbcf8132cdbffe33ac934d2a42aa224933f78dbed6e080f145779a000000009100473044022054652cb0a1394d9a1b0b8890d8f2e7f453de601dea348a39cfb13d9f486d34a0022041290d8c7d2e8454bb340490c33ae51880ef16f4710f1eed26ba3e34b6c55df801475221032708b38ce0a425185ac35dad3b906e2a0b6dc177da078515bc96f8aa535309782102fa4ee3e47ac7e61bcff3a82d4a223cedf736242ee3431e59ece2b883882c934452aeffffffff0200a3e1110000000017a914b7fd911bc1a3e4be0c1d17f6f0e45bd3c42f30098700e1f505000000001976a914ad7f64974fef9d7bf8950e607fbb43bf7226fb5c88ac00000000",
  "complete": false,
  "errors": [
    {
      "txid": "9a7745f180e0d6be8df7334922aa422a4d93ac33febfcd3281cfcb2332ac941c",
      "vout": 0,
      "scriptSig": "00473044022054652cb0a1394d9a1b0b8890d8f2e7f453de601dea348a39cfb13d9f486d34a0022041290d8c7d2e8454bb340490c33ae51880ef16f4710f1eed26ba3e34b6c55df801475221032708b38ce0a425185ac35dad3b906e2a0b6dc177da078515bc96f8aa535309782102fa4ee3e47ac7e61bcff3a82d4a223cedf736242ee3431e59ece2b883882c934452ae",
      "sequence": 4294967295,
      "error": "Operation not valid with the current stack size"
    }
  ]
}
```

And this signed transaction will now be signed by the second signer
```
signrawtransaction '01000000011c94ac3223cbcf8132cdbffe33ac934d2a42aa224933f78dbed6e080f145779a000000009100473044022054652cb0a1394d9a1b0b8890d8f2e7f453de601dea348a39cfb13d9f486d34a0022041290d8c7d2e8454bb340490c33ae51880ef16f4710f1eed26ba3e34b6c55df801475221032708b38ce0a425185ac35dad3b906e2a0b6dc177da078515bc96f8aa535309782102fa4ee3e47ac7e61bcff3a82d4a223cedf736242ee3431e59ece2b883882c934452aeffffffff0200a3e1110000000017a914b7fd911bc1a3e4be0c1d17f6f0e45bd3c42f30098700e1f505000000001976a914ad7f64974fef9d7bf8950e607fbb43bf7226fb5c88ac00000000' '[{"txid":"9a7745f180e0d6be8df7334922aa422a4d93ac33febfcd3281cfcb2332ac941c","vout":0,"scriptPubKey":"a914b7fd911bc1a3e4be0c1d17f6f0e45bd3c42f300987","redeemScript":"5221032708b38ce0a425185ac35dad3b906e2a0b6dc177da078515bc96f8aa535309782102fa4ee3e47ac7e61bcff3a82d4a223cedf736242ee3431e59ece2b883882c934452ae"}]' '["cmJieM35LY3EZ2QLZ8miAKJiiAxgDQf4PfbqzvFB7FBHPHM4toKd"]'
{
  "hex": "01000000011c94ac3223cbcf8132cdbffe33ac934d2a42aa224933f78dbed6e080f145779a00000000da00473044022054652cb0a1394d9a1b0b8890d8f2e7f453de601dea348a39cfb13d9f486d34a0022041290d8c7d2e8454bb340490c33ae51880ef16f4710f1eed26ba3e34b6c55df801483045022100d1959b027899a2c948ff69963e45bc16268ca14f0a01f8772cc061b8469cbe7702202b5a2807644a787ddc336f1715ea4225c9ed292e51e36ca48992988152d1cb8601475221032708b38ce0a425185ac35dad3b906e2a0b6dc177da078515bc96f8aa535309782102fa4ee3e47ac7e61bcff3a82d4a223cedf736242ee3431e59ece2b883882c934452aeffffffff0200a3e1110000000017a914b7fd911bc1a3e4be0c1d17f6f0e45bd3c42f30098700e1f505000000001976a914ad7f64974fef9d7bf8950e607fbb43bf7226fb5c88ac00000000",
  "complete": true
}
```

However has that signed transaction can now broadcast the transaction
```
sendrawtransaction 01000000011c94ac3223cbcf8132cdbffe33ac934d2a42aa224933f78dbed6e080f145779a00000000da00473044022054652cb0a1394d9a1b0b8890d8f2e7f453de601dea348a39cfb13d9f486d34a0022041290d8c7d2e8454bb340490c33ae51880ef16f4710f1eed26ba3e34b6c55df801483045022100d1959b027899a2c948ff69963e45bc16268ca14f0a01f8772cc061b8469cbe7702202b5a2807644a787ddc336f1715ea4225c9ed292e51e36ca48992988152d1cb8601475221032708b38ce0a425185ac35dad3b906e2a0b6dc177da078515bc96f8aa535309782102fa4ee3e47ac7e61bcff3a82d4a223cedf736242ee3431e59ece2b883882c934452aeffffffff0200a3e1110000000017a914b7fd911bc1a3e4be0c1d17f6f0e45bd3c42f30098700e1f505000000001976a914ad7f64974fef9d7bf8950e607fbb43bf7226fb5c88ac00000000
ab580ab6620730411250ac92759e634c3f64a308ae4288bc93feaeb3d50ec0c5
```
