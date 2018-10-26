Plasma Cashの細かいところ整理
=====



# confirmation signature

confsig無しだと

1->2- (invalid->invalid->...) >3

こういう状態になり得る。3がexit不可にならないように、exit gameを修正しないといけない。
confsigありだと最後の3にconfsigをしないので、exit不可にはならない。
送金だけなら、confsig無しの方が良いと思う。


# atomic swap

AliceとBobがcoinAとcoinBを交換するシナリオで、
atomic swapのproofをwithholdした上で、BobがcoinBをinvalid historyでexitするケース。

atomic swapにconfsigが必要にすれば、Aliceは少なくともtxとsignは持っている。
それらでforce includeすると、coinAとcoinBのexitが、atomic swapのproofなどが公開されるまで、cancelされる。
