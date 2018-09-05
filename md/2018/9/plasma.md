plasma general state exitについて
=====

by @syuhei176, @_sgtn

Plasma MVP(Moreの方), Plasma Cashの説明は省略。


## 現状のPlasmaの問題点

Plasma EVMの難しさ

1. 誰の責任でexitするか？
2. 最終状態をどうやって証明するか?
3. exit gameが成立するか？

## 提案手法

- multi child chainを導入
- shelterを導入
- 子チェーンにTXVM+weight, owner有りUTXOを導入

UTXO = (contract, optional<owner>, weight)

子チェーンからはUTXOをexitする。
ownerがある場合、weightでexit状態に入る。
ownerがない場合、shelter contractに格納される。

### 登場人物

- RootChain Contract

global deposit poolとchild chain毎のdeposit poolがある。

- Shelter Contract

exitしたUTXOを退避しておく。

1. 誰の責任でexitするか？

UTXOを必要とする人がexitする。
ownerがある場合、ownerがexitし、withdrawできる。
ownerがない場合、（multisigとか）利用可能なユーザしかexitする必然性はない。
誰がexitしても、shelter contractに格納され、誰でも再度別チェーンにdepositできる。


2. 最終状態をどうやって証明するか?

Plasma MVPと同様だが、UTXOはportableなcontractを持っている。

3. exit gameが成立するか？

全てのUTXOはweightを持っている、weightは保存法則てきに分割されて行くので、合計がdepositされた総額と等しい。
子チェーンからのexitにより、子チェーンのdeposit pool額からweightが引かれていく、Plasma MVPと同等にexit gameが成立する。

## 安全なdeposit

depositでは子チェーン管理poolに総額が記録される。
deposit時に記録されるため、2重depositは不可である。
double depositされた子チェーンに入ってはいけない。入ってしまった場合でも、そのまま出ればdeposit記録で勝てる？




