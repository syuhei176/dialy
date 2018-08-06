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
- refundTime(refundできる期間)を指定します。

この時点でPartyAはrefundはできません。指定したrefundTime以後でかつredeemされてない場合、refund可能です

## Participate

ParticipantはInitiatorからsecretHashを受け取ります。
これによりChain Bのコントラクトでparticipateが可能です。
Participateの際には、以下のパラメータを渡します。

- Party AのChain Bでのアドレス(initiator address)を指定します。
- 受け取ったhashedSecretを渡します。
- refundTime(refundできる期間)を指定します。

この時点でPartyBはrefundはできません。指定したrefundTime以後でかつredeemされてない場合、refund可能です

この時点でParty AはChain Bのコントラクトのredeemが可能です。**Party BはChain AのコントラクトのrefundTimeが十分であることを事前に確認しなければいけません。**

## Redeem

Party AがChain Bのコントラクトでredeemするためには、Contractにsecretを渡さなければいけません。
そしてその時点でsecretが明らかになり、Party BもChain Aでredeemすることができます。

# Reference

シーケンス図
https://github.com/decred/atomicswap

evm contract
https://github.com/AltCoinExchange/ethatomicswap/blob/master/contracts/AtomicSwap.sol
