# Fast Secure Two-Party ECDSA Signing

[TOC]

### 1. 概述

两方门限ECDSA签名方案，利用Paillier的加同态属性和零知识证明系统，实现了两方分别使用各自的私钥share生成签名碎片，然后通过交互，聚合签名碎片，从而生成了标准的ECDSA签名。

### 2. 预备知识

本节主要介绍方案涉及的密码学概念，包括ECDSA签名方案，Paillier加密方案和零知识证明方案。

#### 2.1. ECDSA

ECDSA签名算法基于椭圆曲线群，其简要描述如下：

椭圆曲线系统参数：

+ $G$ 椭圆曲线群的生成元
+ $q$ 椭圆曲线群的阶

假设待签名消息为$m$，签名者的私钥为$x \leftarrow \mathbb Z_{q}$，公钥为$Q = x \cdot G$

生成消息 $m$ 的签名$(r,s)$的步骤为：

> 1. 选择随机值$k \leftarrow \mathbb Z_{q}$
> 2. 计算$R=k \cdot G=(r_x, r_y)$ ，以及 $r\equiv r_x\ mod\ q$，若$r=0$，则返回第一步，重新选择$k$
> 3. 计算$m^{'} =H(m)$ ，其中$H$为哈希函数
> 4. 计算$s \equiv k^{-1}(m^{'}+r \cdot x)\ mod\ q$，若$s=0$，则返回第一步，重新选择$k$
> 5. 输出消息 $m$ 的签名$(r,s)$



对签名的验证步骤如下：

> 1. 验证 $r,s\in [1,q-1]$ ，若不成立，则签名无效
> 2. 计算 $m^{'}=H(m)$ 
> 3. 计算$u_1\equiv m^{'} \cdot s^{-1}\ mod\ q$,  $u_2\equiv r \cdot s^{-1}\ mod\ q$
> 4. 计算$R=(r_x, r_y)=u_1G+u_2Q$，若$(r_x, r_y)=O$，则签名无效
> 5. 若 $r\equiv r_x\ mod\ q$ ，签名有效，否则，签名无效

#### 2.2. Paillier

Paillier加密算法是密码学家Pascal Paillier在1999年设计的公钥密码方案。Paillier算法是一种非确定性的非对称加密算法，基于复合剩余类的困难问题。该算法满足同态加法属性，即在用户仅知道公钥$pk$和消息$m_1$，$m_2$的密文情况下，可以计算出$Enc_{pk}(x+y)$。

算法主要包括三个部分：

- 密钥生成：与RSA类似，需要生成两个长度相同的大素数$p$和$q$，从而计算$N=p \cdot q$ 和 $\phi(N)$，并取$g = N+1$，输出公钥$N$ 和私钥 $(N, \phi(N))$
- 加密：随机选择$r$，满足$0<r<$ N，$r\in \mathbb{Z}_{N}^*$，与$N$互质，并计算密文$c=g^m\cdot r^N mod ~ N^2$
- 解密：计算明文$m=L(c^{\lambda}~mod~N^2)\cdot \mu ~mod~N$，其中$L(x) =\frac{x-1}{N}$。

**Paillier中的加法同态**：

$$Enc_{pk}(m_1,r_1) \cdot Enc_{pk}(m_2,r_2)=g^{m_1}r_1^n\cdot g^{m_2}r_2^n=g^{m_1+m_2}\cdot (r_1r_2)^n=g^{m_1+m_2}r_3^n=Enc_{pk}(m_1+m_2,r_3)$$



#### 2.3. 零知识证明

在零知识证明系统中，证明者 P 知道问题的答案，他需要向验证者V 证明“他知道答案”这一事实，但是要求验证者不能获得答案的任何信息。

> an interactive method for one party to prove to another that a (usually mathematical) statement is true, without revealing anything other than the veracity of the statement。

零知识证明具有 3 个安全特性：

- 完备性 completeness：验证者 V 无法欺骗证明者 P，即如果证明者 P 知道答案，那么 P使 V 以绝对的优势的概率相信他的证明。
- 可靠性 soundness：证明者 P 无法欺骗验证者 V，即如果 P 不知道答案，那么 P 使 V 相信他的证明的概率很低。
- 零知识性 zero-knowledge：验证者 V 无法获取关于答案的任何信息。

### 3. 方案描述

签名方案主要包括两个子协议：密钥生成协议和签名生成协议。

#### 3.1. 定义

系统参数：

+ $\mathbb G :$ 椭圆曲线群
+ $G：$ 群$\mathbb{G}$的生成元
+ $q：$ 群$\mathbb{G}$的阶

定义：

1. 承诺$\mathcal{F}_{com}$:

2. 零知识证明$\mathcal{F}_{zk}^{R}$:

3. $\mathcal{F}_{com-zk}^{R}$:

> 1）$P_i$ 发送消息$(com-prove,sid, x, w)$
>
> 2)   $\mathcal{F}_{com-zk}^{R}$存储$(sid, i ,x)$并发送消息$(proof-receipt,sid)$给$P_{3-i}$
>
> 3）$P_i$发送$(decom-proof,sid)$
>
> 4）$\mathcal{F}_{com-zk}^{R}$发送$(decom-proof,sid,x)$给$P_{3-i}$

零知识证明：

+ 证明Pallier公钥正确生成：

$$
R_P = \{(N, \phi(N))\ |\ gcd(N, \phi(N)) = 1\}
$$

+ 证明用户知道椭圆曲线群中某个点$P$的离散对数：

$$
R_{DL} = \{(\mathbb{G}, G,q,P,w)\ |\ P = w \cdot G\}
$$

+ 证明使用Pallier加密的密文$c$的明文是某个给定点$Q_1$的离散对数：

$$
L_{PDL} = \{(c,pk,Q_1,\mathbb{G},G,q)\ |\ \exists(x_1,r)\ 使得 c = Enc_{pk}(x;r),\ Q_1 = x_1 \cdot G,\ x_1 \in \mathbb{Z}_q\}
$$



#### 3.2. 密钥生成 $KeyGen(\mathbb G,G,q)$

给定$(\mathbb G,G,q)$和安全参数$1^n$。

1. $P_1的初始信息$：

   （a）$P_1$选择随机$x_1\leftarrow \mathbb Z_{q/3}$,计算$Q_1=x_1\cdot G$。

   （b）$P_1$将$(com-prove,1,Q_1,x_1)$发送到$\mathcal F_{com-zk}^{R_{DL}}$。

2. $P_2的初始信息$：

   （a）$P_2$从$\mathcal F_{com-zk}^{R_{DL}}$中接收 $(proof-receipt, 1)$。

   （b）$P_2$选择随机$x_2\leftarrow \mathbb Z_q$,计算$Q_2=x_2\cdot G$。

   （c）$P_2$将$(prove, 2, Q_2, x_2)$发送到$\mathcal F_{zk}^{R_{DL}}$。

3. $P_1的第二条信息$：

   （a）$P_1$从$\mathcal F_{zk}^{R_{DL}}$中接收$(prove, 2, Q_2)$。如果未接收到则密钥生成失败。

   （b）$P_1$将$(decom-proof, 1)$发送到$\mathcal F_{com-zk}^{R_{DL}}$。

   （c）$P_1$生成一个长度为$min(3 log |q| + 1, n)$的Paillier密钥对$(pk,sk)$,并计算$c_{key} = Enc_{pk}(x_1)$。

   （d）$P_1$将$(prove, 1, N,(p1, p2))$发送给$\mathcal F_{zk}^{R_{P}}$，其中$pk = N = p_1 · p_2$,并将$c_{key}$发送给$P_2$。

4. ZK proof:

   $P_1$向$P_2$提供零知识证明：$(c_{key},pk,Q_1)\in L_{PDL}$。

5. $P_2$的验证：

   当以下条件都满足时$P_2$验证通过，若有一条不满足则密钥生成失败：

   （a）从$\mathcal F_{zk}^{R_{DL}}$接收到$(decom-proof, 1，Q_1)$；从$\mathcal F_{zk}^{R_P}$接收到$(proof, 1，N)$。

   （b）接收到证明：$(c_{key},pk,Q_1)\in L_{PDL}$。

   （c）密钥$pk=N$的长度至少为$min(3 log |q| + 1, n)$。

输出：

（a）$P_1$计算$Q=x_1\cdot Q_2$并保存$(x_1,Q)$。

（b）$P_2$计算$Q=x_2\cdot Q_1$并保存$(x_2,Q，c_{key})$。

#### 3.3. 签名生成 $Sign(sid, m)$

经过上述密钥生成算法后，协议的参与者$P_1,\ P_2$分别持有：

+ $P_1$ : $x_1, Q$
+ $P_2$ : $x_2, Q, c_{key}$

其中$Q$为两方合成的公钥，作为最终签名的公钥。

输入：

+ $P_1$ : $x_1, Q, m, sid$ 
+ $P_2$ : $x_2, Q, c_{key}, m, sid$

$P_1,\ P_2$分别对消息进行哈希计算 $m^,\leftarrow H_q(m)$，并且验证sid的唯一性。

1. $P_1$ :

   （a）$P_1$选择随机$k_1\leftarrow \mathbb Z_{q}$,计算$R_1=k_1\cdot G$。

   （b）$P_1$将$(com-prove,sid ||1, R_1, k_1)$发送到$\mathcal F_{com-zk}^{R_{DL}}$。

2. $P_2$ :

   （a）$P_2$从$\mathcal F_{com-zk}^{R_{DL}}$中接收 $(proof-receipt, sid||1)$。

   （b）$P_2$选择随机$k_2\leftarrow \mathbb Z_q$, 计算$R_2=k_2\cdot G$。

   （c）$P_2$将$(prove, sid||2, R_2, k_2)$发送到 $\mathcal F_{zk}^{R_{DL}}$。

3. $P_1$：

   （a）$P_1$从$\mathcal F_{zk}^{R_{DL}}$中接收$(prove, sid||2, R_2)$。如果未接收到则签名失败。

   （b）$P_1$将$(decom-proof, sid||1)$发送到$\mathcal F_{com-zk}^{R_{DL}}$。

4. $P_2$ :

   （a）$P_2$ 从$\mathcal F_{com-zk}^{R_{DL}}$接收到$(decom-proof, sid||1，R_1)$。如果为接收到则协议终止。

   （b）$P_2$ 计算 $R = k_2 \cdot R_1$。记 $R = (r_x, r_y)$，$P_2$计算$r = r_x\ mod\ q$。

   （c）$P_2$ 选择随机数 $\rho \leftarrow \mathbb Z_{q^2}$，并计算：

   ​		$c_1 = Enc_{pk}(\rho \cdot q + [k_2^{-1} \cdot m^{'}\ mod\ q])$，$v =  k_2^{-1} \cdot r \cdot x_2\ mod\ q$ , 

   ​		$c_2 = v \odot c_{key}$,

   ​		$c_3 = c_1 \oplus c_2$.

   （d）$P_2$ 将$c_3$ 发送给 $P_1$。

5. $P_1$产生签名并输出：

   （a）$P_1$计算 $R = k_1 \cdot R_2$。记 $R = (r_x, r_y)$，$P_1$计算$r = r_x\ mod\ q$。

   （b）$P_1$ 计算 $s^{'} = Dec_{sk}(c_3)$和 $s^{''} = k_1^{-1} \cdot s^{'}\ mod\ q$。令$s = min\{s^{''}, q - s^{''}\}$

   （c）$P_1$ 验证签名对$(r,s)$的有效性。若有效，则输出签名，否则，签名失败。



### 4. 零知识证明方案

+ $L_{PDL}$:

  **输入：** 双方都具有的参数为：$(c, pk, Q_1, \mathbb G, G, q)$，prover具有的参数为$(x_1,sk)$，其中$x_1\in\mathbb Z_{q/3}$。

  **协议：**

  1. $V$随机选择$a\leftarrow \mathbb Z_q$，$b\leftarrow \mathbb Z_{q^2}$。计算$c' = (a \odot c) \oplus b$ , $c''=commit(a,b)$,然后$V$把$(c',c'')$发送给$P$。同时$V$计算$Q'=a\cdot Q_1+b\cdot G$。
  2. $P$从$V$中接收$(c',c'')$，解密获得$\alpha=Dec_{sk}(c')$,并计算$\hat Q=\alpha\cdot G$。$P$将$\hat c=commit(\hat Q)$发送给$V$。
  3. $V$进行$decommit(c'')$，公开$(a,b)$。
  4. $P$验证$\alpha=a\cdot x_1+b$。如果不满足等式，则验证失败。验证成功后对$\hat c$进行$decommit$得到$\hat Q$并公开。
  5. Range-ZK proof:  与上述过程同时进行，$P$零知识证明$x_1\in \mathbb Z_q$。

  **$V$的输出：**

  $V$验证成功当且仅当：满足range proof 以及 $\hat Q=Q'$



### 参考文献

