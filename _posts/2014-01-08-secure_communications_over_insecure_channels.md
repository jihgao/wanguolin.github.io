---
layout: post
title: "在不可信的环境下建立可信连接(未完成)"
description: ""
category: 技术笔记
tags: [密码学]
---
{% include JB/setup %}


主流密码学的本质是信息的变幻。古典密码学更是由代换和置换[substitution and transposition][1] 这两种基本技巧所组成。比方最早期据说由凯撒大帝发明的Caesar密码，就是将26个字母分别以其后顺位第三个字母所替换：

`abcd  =>  defg       hello => khooq`

如果26个字母映射为数字，将位移扩展为k，就得到一般的Caesar算法公式：

`加密算法：C = E(p) = (p + k)mod(26)    解密算法： p = D(C) = （C - k）mod(26)`

再上面的公式中，p代表原文，C是密文，k是加密过程中的需要的重要参数，也是由密文还原出原文所需要的重要参数，如果借用现实生活中钥匙的比喻，k即是这个算法中的Key。

对于所有基于代换的派生算法，凡是可以转化为k的一元一次的加密公式的，都存在着一个统一的攻击思路：词频统计。基于大数定理，对于给定的自然语言，或是某个特定的消息集合，其26个字母的出现频率将收敛至某个常数频率。那么基于对密文的词频扫描，就可以推出他们和原文中同等词频的映射关系，进而求解出k。


    某数字公司漏洞修复代码中的漏洞信息数据库就是通过简单异或来加密一个xml的纯文本，
    可以通过词频统计出高频字符的范围来猜解被异或的数字，由于xml的首个可见字符是<，
    更比较容易的进行[选择明文]攻击。


为了屏蔽词频信息，出现了加密复杂度更高的多表代换算法，以Hill为例，这是1929年由数学家Lester Hill发明的。其加密算法是将m个连续的明文字母替换成m个密文字母。如果把26个字母映射为数字，对m个连续明文替换为m个明文字母，就是一个空间复杂度$$ m^2 $$的解空间，同时也屏蔽了单个字母出现的概率规律。我们令m=3，数学表达式如下：


$$ \mathbf{c}_1 = ( \mathbf{k}_{11}\mathbf{p}_1 + \mathbf{k}_{12}\mathbf{p}_2 + \mathbf{k}_{13}\mathbf{p}_3 ) mod 26 $$

$$ \mathbf{c}_2 = ( \mathbf{k}_{21}\mathbf{p}_1 + \mathbf{k}_{22}\mathbf{p}_2 + \mathbf{k}_{23}\mathbf{p}_3 ) mod 26 $$

$$ \mathbf{c}_3 = ( \mathbf{k}_{31}\mathbf{p}_1 + \mathbf{k}_{32}\mathbf{p}_2 + \mathbf{k}_{33}\mathbf{p}_3 ) mod 26 $$

用矩阵表示如下：


$$ \begin{bmatrix}\mathbf{c}_1 \\ \mathbf{c}_2 \\ \mathbf{c}_3 \end{bmatrix} = 
\begin{bmatrix} \mathbf{k}_{11} \mathbf{k}_{12} \mathbf{k}_{13} \\ \mathbf{k}_{21} \mathbf{k}_{22} \mathbf{k}_{23}  \\ \mathbf{k}_{31} \mathbf{k}_{32} \mathbf{k}_{33} \end{bmatrix}  
    \begin{bmatrix} \mathbf{p}_1 \mathbf{p}_2 \mathbf{p}_3 \end{bmatrix} $$


以上的方法都是用代换的方法来做信息的变幻，前面我们提到变幻的方法，有代换和置换，那么置换的思路是怎样的呢？置换就是在原有信息的集合之内做文章，譬如我们正常人读写顺序是从左往右按行读取，那么我们将信息从右往左按列重排呢？原先的阅读方式就一筹莫展了，而新的排布方式，就是还原出原始数据的密钥。由于单纯的置换仍然保留着原文一样的词频，这确实是留给密码攻击者的一个破绽，但是对于多轮替换而言，密文就会越来越杂乱无张，难于分析。在由计算机实现的置换算法中，往往需要而外的额存储空间，和由置换规则所带来的要求——长度是某个规则（行列置换）的倍数，因此置换密码并没由代换密码那么通用。


二战时候著名的Enigma密码机的本质就是一个多表代换，Enigma由一组转轮和字母输入按键所组成。每一个字母和每一组转轮，组成了一个单表映射，每输入一个字母，转轮发生一次旋转，当转轮旋转达到一个周期的时候，会引发同组的下一个转轮发生一个旋转，同时单表映射发生更换。对于由三个转轮26个字母所组成的密码机而言,三个滚轮协同完成的一个周期所生成的代换表的数目为$$ 26^3 $$ = 17576 。对于如此强大的周期而言，


对于如此强大的
以上方法

[1]:/assets/cryptography_and_network_security.pdf  "Cryptography and Network Security - Principles and Practice, Chapter 2.2.2, Substitution Techniques. 《密码编码学与网络信息安全》"






