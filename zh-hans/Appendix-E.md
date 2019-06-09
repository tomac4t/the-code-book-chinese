## 附录五 RSA 的数学原理

以下是 RSA 加密和解密机制的简易数学描述。

1. Alice 选取两个大的质数（prime numbers）p 和 q. 质数应当尽可能的大，但为了简化说明，我们假设 Alice 取 p=17, q=11. 她必须对这两个数字保密。

2. Alice 将它们相乘得到新数字 N. 在本例中 N=187. 现在她选取新数字 e, 在本例中她取 e=7（e 和 (p−1)×(q−1)应当互质（relatively prime），但这是技术性的）。

3. 现在 Alice 可以将 e 和 N 发表在类似电话簿的地方。由于这两个数字是加密的必要条件，它们必须能被想要给 Alice 发送加密消息的人获取到。这两个数字合称公钥（the public key）。（e 也可以是其他人公钥的组成部分，就像它是 Alice 公钥组成部分一样。然而，其他人必须有着不同的 N 值，它取决于 p 和 q的取值。）

4. 加密的消息首先要转换成数字 M. 比如，一个单词可以转换成 ASCII 二进制数字，二进制数字可以被转换成十进制。根据公式 C=Me (mod N), M 被加密成密文 C.

5. 假设 Bob 想要给 Alice 发送一个小玩意：字母 X. 在 ASCII 中它被表示为 1011000, 等于十进制 88. 因此 M=88.

6. 要加密这条消息，Bob 查找了 Alice 的公钥，找到 N=187, e=7. 这提供了他给 Alice 发送加密消息的加密等式所需要的信息. 由 M=88, 可知等式 C=887 (mod 187).

7. 直接在计算器计算是很困难的，因为它无法显示如此大的数字。然而，幂模运算（exponentials in modular arithmetic）中有一个技巧，因为我们知道 7=4+2+1,

```
887 (mod 187)=[884 (mod 187)×882 (mod 187)×881 (mod 187)] (mod 187)
881=88=88 (mod 187)
882=7744=77 (mod 187)
884=59969536=132 (mod 187)
887=881×882×884=88×77×132=894432=11 (mod 187)
```

现在 Bob 可以发送密文 C=11 给 Alice。

8. 我们已经知道幂模运算是一种单向函数（one-way functions），因此从 C=11反过来恢复到原始消息 M 是很困难的，Eve 无法解密消息。


9. 但 Alice 可以，因为她得到了某个特别的消息：她知道 p 和 q 的取值。她计算一个特别的数字，解密密钥 d, 它也被称为私钥。数字 d 的计算遵从以下等式：

```
e×d=1 (mod (p–1)×(q–1))
7×d=1 (mod 16×10)
7×d=1 (mod 160)
d=23
```

（计算 d 的值并不容易，但是辗转相除法（Euclid’s algorithm）能够让 Alice 又快又准地找到 d.）

10. 要解密消息，Alice 使用这个等式：

```
M=Cd (mod 187)
M=1123 (mod 187)
M=[111 (mod 187)×112 (mod 187)×114 (mod 187)×1116 (mod 187)] (mod 187)
M=11×121×55×154 (mod 187)
M=88=X in ASCII
```

Rivest, Shamir 和 Adleman 创造了一种特殊的单向函数，只有拥有特殊信息（即 p和 q 值）的人才能执行其逆运算。通过选取 p 和 q，他们相乘等于 N，得到特定的函数。

为了清楚地阐明要点，我们已经逐字逐句地解释了 RSA 加密消息的过程。在前面的例子中，RSA 有效地解决了单字母替换密码的密钥分发问题。在实际中，加密会使用巨大的二进制数字块进行计算，这使得频率分析成为不可能。

<script type="text/javascript" src="https://tomcat.one/ajax/libs/mathjax/2.7.2/MathJax.js?config=TeX-MML-AM_CHTML" async></script><script type="text/x-mathjax-config">
MathJax.Hub.Config({
  jax: ["input/TeX", "output/HTML-CSS"],
  tex2jax: {
    inlineMath: [["$", "$"]],
    displayMath: [["$$", "$$"]],
    skipTags: ['script', 'noscript', 'style', 'textarea', 'pre', 'code']
  },
  "HTML-CSS": {
    preferredFont: "TeX", availableFonts: ["STIX", "TeX"]
  }
});
</script>