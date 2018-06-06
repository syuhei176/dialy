
https://github.com/omisego/plasma-mvp は[こちら]( https://ethresear.ch/t/minimal-viable-plasma/426/97 )を元に実装されている、plasmaの現状(2018年6月)の最小構成だそうです。

# できること

## contents

- room chain contract
- child chain
- cli

## features

- deposit
- sendtx
- submit
- withdraw

# Reading

Makefileから見ていきます。

https://github.com/omisego/plasma-mvp/blob/master/Makefile
```
.PHONY: root-chain
root-chain:
	python deployment.py

.PHONY: child-chain
child-chain:
	python plasma/child_chain/server.py
```

root-chainへのcontractのデプロイは `deployment.py` 、child_chainのentrypointは `plasma/child_chain/server.py` であることがわかります。

## deposit

depositoを追っていきましょう。

cli -> rootchain

https://github.com/omisego/plasma-mvp/blob/master/plasma/root_chain/contracts/RootChain/RootChain.sol#L103

`require(currentDepositBlock < childBlockInterval);`


ここだけみるとchildBlockInterval回だけdepositできる。
