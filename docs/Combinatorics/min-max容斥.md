## min-max容斥

通过集合最小值计算集合最大值，或者反过来

$$max(S)=\sum\limits_{T\subseteq S}min(T)(-1)^{|T|-1}$$

$|T|$ 表示 $T$ 的元素个数

$$min(S)=\sum\limits_{T\subseteq S}max(T)(-1)^{|T|-1}$$

如果只是计算集合最大最小值，$O(n)$ 即可完成，但是这个结论可以放到期望上

即，max表示满足所有条件的期望，min表示满足至少一个条件的期望

## Kthmin-max容斥

$$Kthmax(S)=\sum\limits_{T\subseteq S}min(T)\dbinom{|T|-1}{k-1}(-1)^{|T|-k}$$

## gcd-lcm容斥

$$lcm(S)=\sum\limits_{T\subseteq S}gcd(T)(-1)^{|T|-1}$$
