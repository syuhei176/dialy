Plasma Chamberの実装で感じたこと
=====


## 課題感


### 履歴量

- 全体で2^16(約6万)coinを扱う時の、coin辺りの履歴量は100Mbyte程度。
- RSA Accumulatorを使うと最大でも10Mbyteくらいにできるはず。
- checkpointと組み合わせることで、複数coinで数十Mbyte程度にしたい。


```
60 * 24 * 30 * 12 * 64 = 33177600 = 33Mbyte
10coinで、1年の履歴量は330Mbyte
```
RSA accumulatorを連続10blockで圧縮すると33Mbyte。
1年かけてcheckpointを作ると、履歴サイズが現実的な量になる。


### fungible感

- deposit時にcoinを、10, 5, 1, 1, 1, 1, 1と細かく分けると、fragmentする。
- つまり履歴量が多くなる。atomic swapでうまいこと、defragmentしたい。
- お互い履歴量が減る方向に、atomic swapして行く。
