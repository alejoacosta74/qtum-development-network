# Qtum blockchain network for testing

This project contains required files to quickly start up a test/dev environment with QTUM using REGTEST mode

## Pre requisites

Docker and docker-compose should be installed

## Usage

### Start qtum containers

```
docker-compose up -d
```

### Config different qtum CLI for each qtum node

```
source cli.sh
```
### Verify setup

```
❯ q1 -getinfo
{
  "version": 220100,
  "blocks": 0,
  "headers": 0,
  "verificationprogress": 1,
  "timeoffset": 0,
  "connections": {
    "in": 1,
    "out": 0,
    "total": 1
  },
  "proxy": "",
  "difficulty": {
    "proof-of-work": 4.656542373906925e-10,
    "proof-of-stake": 4.656542373906925e-10
  },
  "chain": "regtest",
  "moneysupply": 0,
  "relayfee": 0.00400000,
  "warnings": ""
}
❯ q2 -getinfo
{
  "version": 220100,
  "blocks": 0,
  "headers": 0,
  "verificationprogress": 1,
  "timeoffset": 0,
  "connections": {
    "in": 0,
    "out": 1,
    "total": 1
  },
  "proxy": "",
  "difficulty": {
    "proof-of-work": 4.656542373906925e-10,
    "proof-of-stake": 4.656542373906925e-10
  },
  "chain": "regtest",
  "moneysupply": 0,
  "relayfee": 0.00400000,
  "warnings": ""
}
❯ q3 -getinfo
{
  "version": 220100,
  "blocks": 0,
  "headers": 0,
  "verificationprogress": 1,
  "timeoffset": 0,
  "connections": {
    "in": 1,
    "out": 1,
    "total": 2
  },
  "proxy": "",
  "difficulty": {
    "proof-of-work": 4.656542373906925e-10,
    "proof-of-stake": 4.656542373906925e-10
  },
  "chain": "regtest",
  "moneysupply": 0,
  "relayfee": 0.00400000,
  "warnings": ""
}
```
### Config addresses and get coins

1. Create wallet
   ```
   	❯ q1 createwallet node1wallet
	{
	"name": "node1wallet",
	"warning": ""
	}
```
2. Get new address on node1 and node2
```
	❯ q1 getnewaddress "Alice"
	qXTj9PdnenC6buKeYwgrZmj7BAckbwZRKp

	❯ q1 getaddressesbylabel "Alice"
	{
		"qXTj9PdnenC6buKeYwgrZmj7BAckbwZRKp": {
		"purpose": "receive"
		}
	}
```

```
❯ q2 getnewaddress "Bob"
qZE72EftCZmfvcwDkfYM2TfFuaFiDjYx8v
```

3. Generate +2000 blocks to make matured utxo in node1

```
	❯ q1 -generate 2001
	"4932aeba20733aa8d94233d4bb2110d2b7095197b8130a284da7c3cb9a9b8a7b",
	"4272c5d5d739f153f40866e3afc8268597f85308d8a428798be6aceb0b7117bd",
	"32219854a20bbfb06b470ac23b58737709645cf6ae711cf649eda2919165d14f",
	...
	"31750fcfdd1898b3d1f8433ea7ce3eca8ee6dc2c1da71502c3688f7fb9d71a1f",
	"7d294f644b277437f962c93db4910f5784d57a144b33837ff8b2e898dfc74820",
	"2584a8157c6f80689bb75243138d6335fe27d9e948ae3adeeb6f922289c155cd",
	"73ddc87597f2419fac033cb24eea5871facff0891f77961ad019058ead4b894f"

	❯ q1 getbalance
	20000.00000000

4. Verify all nodes are in sync with latests blocks generated

```
❯ q1 getblockcount
2014
❯ q2 getblockcount
2014
❯ q3 getblockcount
2014
```

5. Send coins to "Bob"

```
❯ q1 sendtoaddress qZE72EftCZmfvcwDkfYM2TfFuaFiDjYx8v 100
f1e6b12734581f9e90114664b42f4ac6ebbb19e8c97ce13951d310806619389a
```


6. Create new block and broadcast TX in the mempool

```
❯ q1 -generate 1
{
  "address": "qZ2SipqV9iuwWje8iQBoBXBJWqn2HD7bgj",
  "blocks": [
    "593386e82eb963aa08099985a015e54f5205627a54a5faa7e549d2c0457ca233"
  ]
}
```

7. Verify new balance

```
❯ q2 getbalance
100.00000000
```