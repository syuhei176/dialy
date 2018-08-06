# はじめに

Atomic Swapについてです。

https://github.com/decred/atomicswap

Atomic Swapは異なるブロックチェーンで暗号通貨を交換する方法です。
具体的な手順を簡単に調べたので、記録として残します。


# 仕組みの前提

Atomic Swapは二つのチェーンで以下のステップで実現できます。ETHのようなスマートコントラクトを実行できるチェーンでは、以下をsmart contractで実現できます。

暗号通貨を交換する人は `Initiator` と `Participant` という役割に分かれます。

smartc contractには以下の４つの操作が必要です。

- Initiate: Initiatorがatomic swapに参加する
- Participate: ParticipantがAtomic swapに参加する
- redeem
- refund: 

### データ構造

```
struct Swap {
  uint initTimestamp;
  uint refundTime;
  bytes20 hashedSecret;
  bytes32 secret;
  address initiator;
  address participant;
  uint256 value;
  bool emptied;
  State state;
}
```

### refund

Initiator側のcontractであればInitiatorに送金し、Participator側のcontractであればParticipatorに送金します。
つまりdepositした資金を元に戻します。
redundを行うには、hashedSecretがあれば十分です。


### redeem

Initiator側のcontractであればParticipantに送金し、Participator側のcontractであればInitiatorに送金します。
redeemを行うには、secretが必要です。

# Atomic Swapの手順

Chain A, Chain Bが存在してChain Aの暗号通貨を持っている人をParty A、Chain Bの暗号通貨を持っている人をParty Bとします。

まずParty AはChain Bでaddressを作成し、Party BもChain Aでaddressを作成してお互いのアドレスを相手に渡します。

## Initiate

Party Aは Chain AのコントラクトでInitiateします。
またsecret文字列を手元で生成して、そのhash(hashedSecret)を計算しておきます、
Initiateの際には、以下のパラメータを渡します。

- Party BのChain Aでのアドレス(participant address)を指定します。
- hashedSecretを渡します。
- refundTimeを指定します。

# Reference

シーケンス図
https://github.com/decred/atomicswap

evm contract
https://github.com/AltCoinExchange/ethatomicswap/blob/master/contracts/AtomicSwap.sol
