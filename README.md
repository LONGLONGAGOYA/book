# book
# About Encryption


## 为何要加密

> 将明文转化成密文，保护数据安全。

## 加密算法分类

### 对称加密
>常见的对称加密算法有DES、3DES、AES、Blowfish、IDEA、RC5、RC6。

>*加密和解密使用相同的密钥*

#### DES
* 介绍  
	>1.  输入：64位明文输入块
	>2.  输出：64位密文输出块
	>3.  密钥：64位密钥 （实际用到了56位，第8、16、24、32、40、48、56、64位是校验位， 使得每个密钥都有奇数个1）

* 过程

	* 初始置换    
	> 其功能是把输入的**64位数据块**按位重新组合，并把输出分为**L0、R0**两部分，每部分各长32位，其**置换规则为将输入的第58位换到第一位,第50位换到第2位……依此类推,最后一位是原来的第7位**。L0、R0则是换位输出后的两部分，L0是输出的左32位，R0是右32位

	>  **例** :设置换前的输入值为D1D2D3……D64,则经过初始置换后的结果为:L0=D58D50……D8;R0=D57D49……D7。

	> **其置换规则见下表**：
		>> 58,50,42,34,26,18,10,2,

		>>60,52,44,36,28,20,12,4,

		>>62,54,46,38,30,22,14,6,

		>>64,56,48,40,32,24,16,8,

		>>57,49,41,33,25,17,9,1,

		>>59,51,43,35,27,19,11,3,

		>>61,53,45,37,29,21,13,5,

		>>63,55,47,39,31,23,15,7,

	* 16轮相同的运算
		1. 加密过程
		>![](https://img-blog.csdn.net/20150319115656456?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvaGFwcHlsZWU2Njg4/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center)

		2. 解密过程
		>![](https://gss3.bdstatic.com/7Po3dSag_xI4khGkpoWK1HF6hhy/baike/c0=baike92,5,5,92,30/sign=de44e957314e251ff6faecaac6efa272/e824b899a9014c08f17663c80b7b02087bf4f40a.jpg)

	* 逆置换
	> 经过 16 次迭代运算后，得到 L16、R16，将此作为输入，进行逆置换，逆置换正好是初始置换的逆运算，由此即得到密文输出。

### 非对称加密
>  RSA、Elgamal、背包算法、Rabin、D-H、ECC（椭圆曲线加密算法）。

>*加密和解密使用不同的密钥：公开密钥和私有密钥*

#### RSA

1. 加密
	> __密文__＝__明文__的__E__次方 mod **N**

	> 公钥＝(E,N)
2. 解密
	> __明文__＝__密文__的__D__次方 mod **N**

	> 私钥=（D,N）

3. 生成密钥对
	>数学依据：对极大整数做因数分解的难度决定了RSA算法的可靠性。换言之，对一极大整数做因数分解愈困难，RSA算法愈可靠

	#### 数学上的概念

	1. 互质关系

		```
		如果两个正整数，除了1以外，没有其他公因子，我们就称这两个数是互质关系（coprime）。比如，15和32没有公因子，所以它们是互质关系。这说明，不是质数也可以构成互质关系。

		关于互质关系，不难得到以下结论：

    　　1. 任意两个质数构成互质关系，比如13和61。

    　　2. 一个数是质数，另一个数只要不是前者的倍数，两者就构成互质关系，比如3	和10。

    　　3. 如果两个数之中，较大的那个数是质数，则两者构成互质关系，比如97和57。

    　　4. 1和任意一个自然数是都是互质关系，比如1和99。

    　　5. p是大于1的整数，则p和p-1构成互质关系，比如57和56。

    　　6. p是大于1的奇数，则p和p-2构成互质关系，比如17和15。
		```

	2. 欧拉函数

		```
		请思考以下问题：

    　　任意给定正整数n，请问在小于等于n的正整数之中，有多少个与n构成互质关系？（比如，在1到8之中，有多少个数与8构成互质关系？）

		计算这个值的方法就叫做欧拉函数，以φ(n)表示。在1到8之中，与8形成互质关系的是1、3、5、7，所以 φ(n) = 4。

		φ(n) 的计算方法并不复杂，但是为了得到最后那个公式，需要一步步讨论。

		第一种情况

		如果n=1，则 φ(1) = 1 。因为1与任何数（包括自身）都构成互质关系。

		第二种情况

		如果n是质数，则 φ(n)=n-1 。因为质数与小于它的每一个数，都构成互质关系。比如5与1、2、3、4都构成互质关系。

		第三种情况

		如果n是质数的某一个次方，即 n = p^k (p为质数，k为大于等于1的整数)，则

		比如 φ(8) = φ(2^3) =2^3 - 2^2 = 8 -4 = 4。

		这是因为只有当一个数不包含质数p，才可能与n互质。而包含质数p的数一共有p^(k-1)个，即1×p、2×p、3×p、...、p^(k-1)×p，把它们去除，剩下的就是与n互质的数。

		上面的式子还可以写成下面的形式：

		可以看出，上面的第二种情况是 k=1 时的特例。

		第四种情况

		如果n可以分解成两个互质的整数之积，

    　　n = p1 × p2

		则

    　　φ(n) = φ(p1p2) = φ(p1)φ(p2)

		即积的欧拉函数等于各个因子的欧拉函数之积。比如，φ(56)=φ(8×7)=φ(8)×φ(7)=4×6=24。

		这一条的证明要用到"中国剩余定理"，这里就不展开了，只简单说一下思路：如果a与p1互质(a<p1)，b与p2互质(b<p2)，c与p1p2互质(c<p1p2)，则c与数对 (a,b) 是一一对应关系。由于a的值有φ(p1)种可能，b的值有φ(p2)种可能，则数对 (a,b) 有φ(p1)φ(p2)种可能，而c的值有φ(p1p2)种可能，所以φ(p1p2)就等于φ(p1)φ(p2)。

		第五种情况

		因为任意一个大于1的正整数，都可以写成一系列质数的积。

		根据第4条的结论，得到

		再根据第3条的结论，得到

		也就等于

		这就是欧拉函数的通用计算公式。比如，1323的欧拉函数，计算过程如下：
		```

	3. 欧拉定理

		```
			欧拉函数的用处，在于欧拉定理。"欧拉定理"指的是：
	    	如果两个正整数a和n互质，则n的欧拉函数 φ(n) 可以让下面的等式成立：
			也就是说，a的φ(n)次方被n除的余数为1。或者说，a的φ(n)次方减去1，可以被n整除。比如，3和7互质，而7的欧拉函数φ(7)等于6，所以3的6次方（729）减去1，可以被7整除（728/7=104）。
			欧拉定理的证明比较复杂，这里就省略了。我们只要记住它的结论就行了。
			欧拉定理可以大大简化某些运算。比如，7和10互质，根据欧拉定理，
			已知 φ(10) 等于4，所以马上得到7的4倍数次方的个位数肯定是1。
			因此，7的任意次方的个位数（例如7的222次方），心算就可以算出来。
			欧拉定理有一个特殊情况。
	    	假设正整数a与质数p互质，因为质数p的φ(p)等于p-1，则欧拉定理可以写成
			这就是著名的费马小定理。它是欧拉定理的特例。
			欧拉定理是RSA算法的核心。理解了这个定理，就可以理解RSA。

		```

	4. 模反元素
		```
	    如果两个正整数a和n互质，那么一定可以找到整数b，使得 ab-1 被n整除，或者说ab被n除的余数是1。

	    这时，b就叫做a的"模反元素"。
			比如，3和11互质，那么3的模反元素就是4，因为 (3 × 4)-1 可以被11整除。显然，模反元素不止一个， 4加减11的整数倍都是3的模反元素 {...,-18,-7,4,15,26,...}，即如果b是a的模反元素，则 b+kn 都是a的模反元素。

			欧拉定理可以用来证明模反元素必然存在。

			可以看到，a的 φ(n)-1 次方，就是a的模反元素。
		```

#### 生成步骤

1. 第一步，随机选择两个不相等的质数p和q。

	>爱丽丝选择了61和53。（实际应用中，这两个质数越大，就越难破解。）

2. 第二步，计算p和q的乘积n。

	>爱丽丝就把61和53相乘。
	>　n = 61×53 = 3233

	>n的长度就是密钥长度。3233写成二进制是110010100001，一共有12位，所以这个密钥就是12位。实际应用中，RSA密钥一般是1024位，重要场合则为2048位。

3. 第三步，计算n的欧拉函数φ(n)。

	> 根据公式：

	>　　φ(n) = (p-1)(q-1)

	>爱丽丝算出φ(3233)等于60×52，即3120。

4. 第四步，随机选择一个整数e，条件是1< e < φ(n)，且e与φ(n) 互质。

	>爱丽丝就在1到3120之间，随机选择了17。（实际应用中，常常选择65537。）

5. 第五步，计算e对于φ(n)的模反元素d。
	> 所谓"模反元素"就是指有一个整数d，可以使得ed被φ(n)除的余数为1。

	> ed ≡ 1 (mod φ(n))

	> 这个式子等价于

 >　ed - 1 = kφ(n)

	>于是，找到模反元素d，实质上就是对下面这个二元一次方程求解。

  >　ex + φ(n)y = 1

	>已知 e=17, φ(n)=3120，

 >　17x + 3120y = 1

	>这个方程可以用"扩展欧几里得算法"求解，此处省略具体过程。总之，爱丽丝算出一组整数解为 (x,y)=(2753,-15)，即 d=2753。

	>至此所有计算完成。

6. 第六步，将n和e封装成公钥，n和d封装成私钥。

## OSI模型  
	1. 应用层
		(QQ，钉钉，HTTP，HTTPS)
	2. 传输层
		(TCP，UDP)
	3. 网络层
		(IPv4,IPv6)
	4. 数据链路层
		...
	5. 物理层
		...

		

## HTTPS

## HTTP

## 电子签名  

## 电子证书  

## 跟证书  

## 加密的方式

## MD5

## 抓包  

## PKI







加密：以某种特殊的算法改变原有的信息数据，使得未授权的用户即使获得了已加密的信息，但因不知解密的方法，仍然无法了解信息的内容。


加密的安全性应该由密钥来保证，而非加密的算法，越是公开的加密算法，其经历的攻击测试越是长久，相比非公开的加密算法要安全可靠

应用领域：
	从最开始的政府保密机构延伸到公共领域，比如：因特网电子商务，手机网络和银行自动取款及等。

故事：
	在古代，加密是由许多办法完成的。在中国较“流行”使用淀粉水在纸上写字，再浸泡在碘水中使字浮现出来。而外国就不同了，最经典的莫过于伯罗奔尼撒战争。公元前405年，雅典和斯巴达之间的伯罗奔尼撒战争已进入尾声。斯巴达军队逐渐占据了优势地位，准备对雅典发动最后一击。这时，原来站在斯巴达一边的波斯帝国突然改变态度，停止了对斯巴达的援助，意图是使雅典和斯巴达在持续的战争中两败俱伤，以便从中渔利。在这种情况下，斯巴达急需摸清波斯帝国的具体行动计划，以便采取新的战略方针。正在这时，斯巴达军队捕获了一名从波斯帝国回雅典送信的雅典信使。斯巴达士兵仔细搜查这名信使，可搜查了好大一阵，除了从他身上搜出一条布满杂乱无章的希腊字母的普通腰带外，别无他获。情报究竟藏在什么地方呢？斯巴达军队统帅莱桑德把注意力集中到了那条腰带上，情报一定就在那些杂乱的字母之中。他反复琢磨研究这些天书似的文字，把腰带上的字母用各种方法重新排列组合，怎么也解不出来。最后，莱桑德失去了信心，他一边摆弄着那条腰带，一边思考着弄到情报的其他途径。当他无意中把腰带呈螺旋形缠绕在手中的剑鞘上时，奇迹出现了。原来腰带上那些杂乱无章的字母，竟组成了一段文字。这便是雅典间谍送回的一份情报，它告诉雅典，波斯军队准备在斯巴达军队发起最后攻击时，突然对斯巴达军队进行袭击。斯巴达军队根据这份情报马上改变了作战计划，先以迅雷不及掩耳之势攻击毫无防备的波斯军队，并一举将它击溃，解除了后顾之忧。随后，斯巴达军队回师征伐雅典，终于取得了战争的最后胜利。
雅典间谍送回的腰带情报，就是世界上最早的密码情报，具体运用方法是，通信双方首先约定密码解读规则，然后通信—方将腰带（或羊皮等其他东西）缠绕在约定长度和粗细的木棍上书写。收信—方接到后，如不把腰带缠绕在同样长度和粗细的木棍上，就只能看到一些毫无规则的字母。后来，这种密码通信方式在希腊广为流传。现代的密码电报，据说就是受了它的启发而发明的。


分类：对称加密和非对称加密

	对称加密：采用相同的密钥
		有关算法：DES（Data Encrypton Standard）--DES使用56位密钥对64位的数据块进行加密，并对64位的数据块进行16轮编码。与每轮编码时，一个48位的"每轮"密钥值由56位的完整密钥得出来。DES用软件进行解码需用很长时间，而用硬件解码速度非常快。幸运的是，当时大多数黑客并没有足够的设备制造出这种硬件设备。在1977年，人们估计要耗资两千万美元才能建成一个专门计算机用于DES的解密，而且需要12个小时的破解才能得到结果。当时DES被认为是一种十分强大的加密方法。
随着计算机硬件的速度越来越快，制造一台这样特殊的机器的花费已经降到了十万美元左右，而用它来保护十亿美元的银行，那显然是不够保险了。另一方面，如果只用它来保护一台普通服务器，那么DES确实是一种好的办法，因为黑客绝不会仅仅为入侵一个服务器而花那么多的钱破解DES密文。



	非对称加密：存在两个密钥：公钥（可以对外公开），私钥（对外保密），使用公钥加密的数据只能通过对应的私钥解密，反之亦然
		有关算法：RSA（Rivest-Shamir-Adleman）算法是基于大数不可能被质因数分解假设的公钥体系。简单地说就是找两个很大的质数。一个对外公开的为"公钥"（Public key） ，另一个不告诉任何人，称为"私钥"（Private key）。这两个密钥是互补的，也就是说用公钥加密的密文可以用私钥解密，反过来也一样


假设用户甲要寄信给用户乙，他们互相知道对方的公钥。甲就用乙的公钥加密邮件寄出，乙收到后就可以用自己的私钥解密出甲的原文。由于别人不知道乙的私钥，所以即使是甲本人也无法解密那封信，这就解决了信件保密的问题。另一方面，由于每个人都知道乙的公钥，他们都可以给乙发信，那么乙怎么确信是不是甲的来信呢？那就要用到基于加密技术的数字签名了。
甲用自己的私钥将签名内容加密，附加在邮件后，再用乙的公钥将整个邮件加密（注意这里的次序，如果先加密再签名的话，别人可以将签名去掉后签上自己的签名，从而篡改了签名）。这样这份密文被乙收到以后，乙用自己的私钥将邮件解密，得到甲的原文和数字签名，然后用甲的公钥解密签名，这样一来就可以确保两方面的安全了。


	5.确保正确实施加密
事实上，实施加密系统并非易事，因为它有许多动态部件，任何一个部件都有可能成为一个薄弱环节。你必须进行大量调查，确保正确实施加密。
在实施加密过程中，哪些方面容易出错？除了密钥容易遭受攻击，还有CBC（密码分组链接）的实施方式。使用CBC，可以用同样长度的随机文本块（也称为初始化向量）对纯文本进行异或运算，然后对其进行加密，产生一个加密文本块。然后，将前面产生的密文块作为一个初始化向量对下一个纯文本块进行异或运算。
CBC的正确实施要求在开始每个过程时都有一个新的初始化向量。一个常见的错误是用一个不加改变的静态初始化向量来实施CBC。如果正确实施了CBC，那么，如果我们在两个不同的场合加密了文本块，所生产的密文块就不会相同。
6.不要忽视外部因素
公司几乎无法控制的外部因素有可能破坏加密系统的安全性。例如，SSL依赖于数字证书，而且这些因素依赖于嵌入在浏览器（如IE、火狐、Chrome等）中的根证书颁发机构的完整性。但是，我们如何知道其是否可信，或者这些证书颁发机构不是某外国情报机构的幌子？你是否觉得这听起来牵强附会，但却有可能是事实。
此外，DNS也是不得不重视的弱点。只要DNS被攻克，攻击者就可以使用钓鱼技术绕过加密。
