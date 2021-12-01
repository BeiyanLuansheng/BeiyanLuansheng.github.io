---
title: 5 数据链路层
author: BeiyanLuansheng
date: 2020-12-11 22:09:24 +0800
categories: [学习笔记, 计算机网络]
tags: [计算机网络, HITCN, 2020秋]
math: true
mermaid: true
---

## 5.1 数据链路层服务

> 主机和路由器：**结点(nodes)**
>
> 连接相邻结点的通信信道：**链路(links)**，有线链路(wired links)、无线链路(wireless links)、局域网(LANs)
>
> 链路层(第2层)数据分组：**帧(frame)**，封装网络层数据报

数据链路层负责通过一条链路从一个节点向另一个物理链路直接相连的相邻结点传送数据报。

**组帧(framing)**：封装数据报构成数据帧，加首部和尾部。完成**帧同步**：通过在首部和尾部加入特定的比特串。

**链路接入(link access)**：如果是共享介质，需要解决信道接入(channel access)。帧首部中的“MAC”地址，用于标识帧的源和目的，不同于IP地址！

**相邻结点间可靠交付**：在低误码率的有线链路上很少采用 (如光纤，某些双绞线等)。无线链路：误码率高，需要可靠交付

**流量控制(flow control)**：协调(pacing)相邻的发送结点和接收

**差错检测(error detection)**：信号衰减和噪声会引起差错。接收端检测到差错：通知发送端重传或者直接丢弃帧

**差错纠正(error correction)**：接收端直接纠正比特差错

**全双工和半双工通信控制**

- 全双工：链路两端结点同时双向传输

- 半双工：链路两端结点交替双向传输

## 5.2 差错编码

差错编码基本原理：D→DR，其中R为差错检测与纠正比特（冗余比特）。即加入了冗余信息，使得原本不存在的比特位建立联系。

- 分组码：多见于计算机网络
  - **线性分组码**：R建立起的D之间的关系是线性的。此类多见
  - 非线性分组码：R建立起的D之间的关系是非线性的。
- 卷积码：应用于通信领域

> 差错编码不能保证100%可靠

### 5.2.1 差错编码的检错能力

差错编码可分为检错码与纠错码

- 对于检错码，如果编码集的汉明距离d~s~=r+1，则该差错编码可以检测r位的差错

- 对于纠错码，如果编码集的汉明距离d~s~=2r+1，则该差错编码可以纠正r位的差错

奇偶校验码

- 1比特校验位：检测奇数位差错

- 二维奇偶校验：检测奇数位差错、部分偶数位差错；纠正同一行/列的奇数位错

Internet校验和(Checksum)

- 发送端：将“数据”(校验内容)划分为16位的二进制“整数”序列，求和(sum)：补码求和(最高位进位的“1”，返回最低位继续加）。校验和(Checksum)：sum的反码。放入分组(UDP、TCP、IP)的校验和字段。

- 接收端：与发送端相同算法计算。计算得到的"checksum"：为16位全0（或sum为16位全1）则无错，否则有错

### 5.2.2 循环冗余校验码(CRC)

检错能力更强大的差错编码

将数据比特，D，视为一个二进制数，选择一个r+1位的比特模式 (生成比特模式)，G。目标：选择r位的CRC比特，R，满足：

- <D,R>刚好可以被G整除(模2)

- 接收端检错：利用G除<D,R>，余式全0，无错；否则，有错！

- 可以检测所有突发长度小于r+1位差错。

> 广泛应用于实际网络 (以太网，802.11 WiFi，ATM)

期望：D * 2^r^ XOR R = nG

相当于：D * 2^r^ = nG XOR R

相当于：如果利用G去除D*2r, 则余式即为R=余式[D * 2^r^ / G]

![image-20201212124204283](image-20201212124204283.png)

## 5.3 多路访问控制(MAC)协议

### 5.3.1 MAC协议

两类“链路”：

- **点对点链路**
  - 拨号接入的PPP
  - 以太网交换机与主机间的点对点链路
- **广播链路 **(共享介质)
  - 早期的总线以太网
  - HFC的上行链路
  - 802.11无线局域网

单一共享广播信道，两个或者两个以上结点同时传输：干扰(interference)

冲突(collision)：结点同时接收到两个或者多个信号→接收失败！

**多路访问控制协议(multiple access control protocol)**：采用分布式算法决定结点如何共享信道，即决策结点何时可以传输数据；必须基于信道本身，通信信道共享协调信息；无带外信道用于协调

#### 5.3.1.1 理想MAC协议

给定：速率为R bps的广播信道，期望：
1. 当只有一个结点希望传输数据时，它可以以速率 R发送.
2. 当有M个结点期望发送数据时，每个节点平均发送数据的平均速率是R/M
3. 完全分散控制：无需特定结点协调；无需时钟、时隙同步
4. 简单

#### 5.3.1.2 MAC协议分类

- 信道划分(channel partitioning)MAC协议
  - 多路复用技术
  - TDMA、FDMA、CDMA、WDMA等

- 随机访问(random access)MAC协议
  - 信道不划分，允许冲突
  - 采用冲突“恢复”机制
  - ALOHA, S-ALOHA, CSMA, CSMA/CD
  - CSMA/CD应用于以太网；CSMA/CA应用802.11无线局域网
- 轮转(“taking turns”)MAC协议
  - 结点轮流使用信道
  - 主结点轮询；令牌传递
  - 蓝牙、FDDI、令牌环网

### 5.3.2 信道划分MAC协议

#### 5.3.2.1 TDMA

TDMA: time division multiple access

- “周期性”接入信道

- 每个站点在每个周期，占用固定长度的时隙(e.g.长度=分组传输时间)

- 未用时隙空闲(idle)

例如：6-站点LAN，1,3,4传输分组，2,5,6空闲

#### 5.3.2.2 FDMA

FDMA: frequency division multiple access

- 信道频谱划分为若干频带(frequency bands)
- 每个站点分配一个固定的频带
- 无传输频带空闲

例如：6站点LAN, 1,3,4频带传输数据, 2,5,6频带空闲。

### 5.3.3 随机访问MAC协议

当结点要发送分组时：利用信道全部数据速率R发送分组，由于没有事先的结点间协调，所以存在两个或多个结点同时传输导致冲突。

随机访问MAC协议需要定义：如何检测冲突；如何从冲突中恢复 (e.g., 通过延迟重传)

> 典型的随机访问MAC协议：时隙(sloted)ALOHA、ALOHA、CSMA、CSMA/CD、CSMA/CA

#### 5.3.3.1 时隙ALOHA协议

假定：

- 所有帧大小相同
- 时间被划分为等长的时隙（每个时隙可以传输1个帧）
- 结点只能在时隙开始时刻发送帧
- 结点间时钟同步
- 如果2个或2个以上结点在同一时隙发送帧，结点即检测到冲突

运行：当结点有新的帧时，在下一个时隙(slot)发送

- 如果无冲突：该结点可以在下一个时隙继续发送新的帧
- 如果冲突：该结点在下一个时隙**以概率p重传**该帧，直至成功

优点:

- 单个结点活动时，可以连续以信道全部速率传输数据
- 高度分散化：只需同步时隙
- 简单

缺点:

- 容易冲突，浪费时隙
- 空闲时隙
- 结点也许能以远小于分组传输时间检测到冲突
- 时钟同步

>**效率(efficiency)：长期运行时，成功发送帧的时隙所占比例 (很多结点，有很多帧待发送)**

假设N个结点有很多帧待传输，每个结点在每个时隙均以概率p发送数据

对于给定的一个结点，在一个时隙将帧发送成功的概率= p(1-p)^N-1^

对于任意结点成功发送帧的概率= Np(1-p)^N-1^

最大效率：求得使Np(1-p)^N-1^最大的p\*

对于很多结点，求Np\*(1-p\*)^N-1^当N趋近无穷时的极限，可得：最大效率= 1/e = 0.37。即最好情况下信道被成功利用的时间仅占37%

#### 5.3.3.2 ALOHA协议

非时隙(纯)Aloha：更加简单，无需同步

当有新的帧生成时立即发送，这也导致冲突可能性增大：在t~0~时刻发送帧，会与在 (t~0~-1, t~0~+1) 期间其他结点发送的帧冲突。

**效率**：P(给定结点成功发送帧) = P(该结点发送) \* P(无其他结点在[t~0~-1, t~0~]期间发送帧) \* P(无其他结点在[t~0~, t~0~+1]期间发送帧)

= p \* (1-p)^N-1^ \* (1-p)^N-1^  =  p . (1-p)^2(N-1)^             选取最优的p，并令n→∞

= 1/(2e) = 0.18

>比时隙ALOHA协议更差

#### 5.3.3.3 CSMA协议

**载波监听多路访问协议CSMA (carrier sense multiple access)**：发送帧之前，监听信道(载波)：信道空闲时发送完整帧；信道忙时推迟发送：

- 1-坚持CSMA：持续监听信道，一旦发现空闲即发送数据。

- 非坚持CSMA：随机等待一段时间后再监听信道

- P-坚持CSMA：以概率P持续监听信道，以概率1-P随机等待一段时间后再监听信道

但由于信号传播延迟，**冲突可能仍然发生**。如果两个结点同时监听到空闲然后同时发送数据，同样会发生冲突。

#### 5.3.3.4 CSMA/CD协议

**CSMA/CD (CSMA with Collision Detection)**：短时间内可以检测到冲突，冲突后传输中止，减少信道浪费。冲突检测:

- 有线局域网易于实现：测量信号强度，比较发射信号与接收信号

- 无线局域网很难实现：接收信号强度淹没在本地发射信号强度下

> “边发边听，不发不听”

**条件**：网络带宽：R bps，数据帧最小长度：Lmin (bits)，信号传播速度：V (m/s)下，需满足：L / R ≥ 2d~max~ / V

- L~min~ / R = 2d~max~ / V

- 由于实际存在中继，可能存在其他延迟：L~min~ / R = RTT~max~

**效率**：$\displaystyle\frac 1 {1+5t_{prop} / t_{trans}}$

t~prop~ = LAN中2个结点间的最大传播延迟

t~trans~ = 最长帧传输延迟

> t~prop~ 趋近于0或者t~trans~ 趋近于∞时，效率趋近于1，远优于ALOHA，并且简单、分散！

### 5.3.4 轮转访问MAC协议

信道划分MAC协议：

- 网络负载重时，共享信道效率高，且公平

- 网络负载轻时，共享信道效率低
  

随机访问MAC协议：

- 网络负载轻时，共享信道效率高，单个结点可以利用信道的全部带宽

- 网络负载重时，产生冲突开销
  

轮转访问MAC协议： 综合两者的优点，两种举例：

- **轮询(polling)**：主结点轮流“邀请”从属结点发送数据。典型应用：“哑(dumb)” 从属设备

  问题：轮询开销；等待延迟；单点故障

- **令牌传递(token passing)**：控制令牌依次从一个结点传递到下一个结点.。令牌：特殊帧

  问题：令牌开销；等待延迟；单点故障

## 5.4 ARP协议

- 32位IP地址：接口的网络层地址，用于标识网络层(第3层)分组，支持分组转发。**IP地址是层次地址：不可“携带”，IP地址依赖于结点连接到哪个子网**

- MAC地址(或称LAN地址,物理地址,以太网地址)：用于局域网内标识一个帧从哪个接口发出，到达哪个物理相连的其他接口。48位MAC地址(用于大部分LANs)，固化在网卡的ROM中，有时也可以软件设置。e.g.：1A-2F-BB-76-09-AD。局域网中的每块网卡都有一个唯一的MAC地址。MAC地址由IEEE统一管理与分配，网卡生产商购买MAC地址空间(前24比特)。**MAC地址是“平面”地址：可“携带”，可以从一个LAN移到另一个LAN**

> 类比：MAC地址：身份证号；IP地址：邮政地址

**ARP**：地址解析协议。解决在同一个LAN内**已知目的接口的IP地址前提下确定其MAC地址**问题。

ARP表：LAN中的**每个IP结点**(主机、路由器)维护一个表，存储某些LAN结点的IP/MAC地址映射关系：**< IP地址; MAC地址; TTL>**。经过TTL (Time To Live)时间以后该映射关系会被遗弃(典型值为20min)。

**A想要给同一局域网内的B发送数据报**，但B的MAC地址不在A的ARP表中，所以：

1. A广播ARP查询分组，其中包含B的IP地址，目的MAC地址 = FF-FF-FF-FF-FF-FF。
2. LAN中所有结点都会接收ARP查询，B接收ARP查询分组，IP地址匹配成功，向A应答B的MAC 地址，利用单播帧向A发送应答。
3. A在其ARP表中，缓存B的IP-MAC地址对，直至超时。超时后，再次刷新。ARP是“即插即用”协议，结点自主创建ARP表，无需干预。

**A通过路由器R向B发送数据报**：假设A知道B的IP地址，A知道第一跳路由器R (左)接口IP地址 (默认网关)，A知道第一跳路由器R (左)接口MAC地址 (ARP协议)。

1. A构造IP数据报，其中源IP地址是A的IP地址，目的IP地址是B的IP地址。
2. A构造链路层帧，其中源MAC地址是A的MAC地址，目的MAC地址是R(左)接口的MAC地址，封装A到B的IP数据报。

3. 帧从A发送至R，R接收帧，提取IP数据报，传递给上层IP协议。

4. R转发IP数据报（**源和目的IP地址不变**）R创建链路层帧，其中源MAC地址是R(右)接口的MAC地址，目的MAC地址是B的MAC地址，封装A到B的IP数据报。

## 5.5 以太网

### 5.5.1 以太网(ETHERNET)

**物理拓扑**

- **总线(bus)**：所有结点在同一冲突域(collision domain) (可能彼此冲突)

- **星型(star)**：中心交换机(switch)，每个结点一个单独冲突域(结点间彼此不冲突)

**以太网：不可靠、无连接服务**

无连接(connectionless)：发送帧的网卡与接收帧的网卡间没有“握手”过程。

不可靠(unreliable)：接收网卡不向发送网卡进行确认。差错帧直接丢弃，丢弃帧中的数据恢复依靠高层协议 (e.g., TCP)，否则，发生数据丢失。

以太网的MAC协议：**采用二进制指数退避算法的CSMA/CD**

> **二进制指数退避算法**用于计算随机等待的时间

#### 5.5.1.1 以太网CSMA/CD算法

1. NIC从网络层接收数据报，创建数据帧。
2. 监听信道：如果NIC监听到信道空闲，则开始发送帧；如果NIC监听到信道忙，则一直等待到信道空闲，然后发送帧。
3. NIC发送完整个帧，而没有检测到其他结点的数据发送，则NIC确认帧发送成功！
4. 如果NIC检测到其他结点传输数据，则中止发送，并发送堵塞信号 (jam signal)
5. 中止发送后，NIC进入二进制指数退避：第m次连续冲突后：取n = Min(m, 10)，NIC 从{0,1,2, …, 2^n^-1}中随机选择一个数K，NIC等待K·512比特的传输延迟时间，再返回第2步。
   - 连续冲突次数越多，平均等待时间越长。
   - 一般情况下，连续16次冲突后就不在发送，向上层报告错误。

#### 5.5.1.2 以太网帧结构

发送端网卡将IP数据报(或其他网络层协议分组)封装到以太网帧中：

![image-20201030132123536](/cn/image-20201030132123536.png)

- **前导码(Preamble)(8B)**：用于发送端与接收端的时钟同步。7个字节的10101010，第8字节为10101011。一般情况下，我们所说的**以太网帧长度不包含前导码的长度**。

- **目的MAC地址、源MAC地址(各6B)**： 如果网卡的MAC地址与收到的帧的目的MAC地址匹配，或者帧的目的MAC地址为广播地址(FF-FF-FF-FF-FF-FF)，则网卡接收该帧，并将其封装的网络层分组交给相应的网络层协议。否则，网卡丢弃(不接收)该帧。

- **类型(Type)(2B)**：指示帧中封装的是哪种高层协议的分组(如，IP数据报、Novell IPX数据报、AppleTalk数据报等)

- **数据(Data)(46-1500B)**：指上层协议载荷。
  - R=10Mbps，RTT~max~=512μs，L~min~ / R = RTT~max~
  - L~min~=512bits=64B，Data~min~=L~min~-18=46B

- **CRC(4B)**：循环冗余校验码，丢弃差错帧

### 5.5.2 交换机

链路层设备

- 主机利用独享(dedicated)链路直接连接交换机

- 存储-转发以太网帧，交换机缓存帧
- 检验到达帧的目的MAC地址，选择性(selectively) 向一个或多个输出链路转发帧
- 交换机在每段链路上利用CSMA/CD收发帧，但无冲突，且可以全双工。每段链路一个独立的冲突域。

透明(transparent)：主机感知不到交换机的存在

即插即用(plug-and-play)

交换(switching)：A-A’与B-B’的传输可以同时进行，没有冲突

每个交换机有一个**交换表(switch table)**，每个入口(entry)：(主机的MAC地址，到达主机的接口，时间戳)

交换机通过**自学习(self-learning)**，获知到达主机的接口信息，无需配置。当收到帧时，交换机“学习”到发送帧的主机（通过帧的源MAC地址），位于收到该帧的接口所连接的LAN网段，将发送主机MAC地址/接口信息记录到交换表中。

#### 5.5.2.1 帧过滤/转发

当交换机收到帧:
1. 记录帧的源MAC地址与输入链路接口（自学习）
2. 利用目的MAC地址检索交换表
3. if 在交换表中检索到与目的MAC地址匹配的入口(entry)  then {
if 目的主机位于收到帧的网段（源和目的主机连在交换机的同一个接口上） then 丢弃帧
else 将帧转发到该入口指向的接口
}
else 泛洪(flood) /* 向除收到该帧的接口之外的所有接口转发 */

#### 5.5.2.2 交换机 vs. 路由器

**均为存储-转发设备**：

- 路由器：网络层设备 (检测网络层分组首部)

- 交换机：链路层设备 (检测链路层帧的首部)

**均使用转发表**：

- 路由器：利用路由算法(路由协议)计算(设置)，依据IP地址

- 交换机：利用自学习、泛洪构建转发表，依据MAC地址

### 5.5.3 虚拟局域网(VLAN)

虚拟局域网(Virtual Local Area Network)：支持VLAN划分的交换机，可以在一个物理LAN架构上配置、定义多个VLAN

**基于端口的VLAN**：分组交换机端口 (通过交换机管理软件)，于是，单一的物理交换机就像多个虚拟交换机一样运行。

- **流量隔离(traffic isolation)**：去往/来自端口1-8的帧只到达端口1-8，同理，也可以基于MAC地址定义VLAN，而不是交换端口

- **动态成员**：端口可以动态分配给不同VLAN

- **在VLAN间转发**：通过路由(就像在独立的交换机之间)。而实践中，厂家会将交换机与路由器集成在一起

跨越多交换机的VLAN，可以使用多线缆连接，每个线缆连接一个VLAN，但显然每个交换机都会浪费一个端口。

**中继端口(trunk port)**：在跨越多个物理交换机定义的VLAN承载帧。为多VLAN转发802.1帧容易产生歧义，所以必须携带VLAN ID信息，802.1q协议为经过中继端口转发的帧增加/去除额外的首部域

![image-20201030140800909](/cn/image-20201030140800909.png)

## 5.5 PPP协议

点对点数据链路控制：一个发送端，一个接收端，一条链路。比广播链路容易：无需介质访问控制(Media Access Control)；无需明确的MAC寻址。e.g., 拨号链路，ISDN链路

常见的点对点数据链路控制协议：

- HDLC (High Level Data Link Control)
- PPP (Point-to-Point Protocol)

### 5.5.1 PPP设计需求

- **组帧**：将网络层数据报封装到数据链路层帧中。可以同时承载**任何网络层协议**分组(不仅IP数据报)；可以向上层实现分用（多路分解）。
- **比特透明传输**：数据域必须支持承载**任何比特模式**
- **差错检测**：(无纠正)
- **连接活性(connection liveness)检测**：检测、并向网络层通知链路失效
- **网络层地址协商**：端结点可以学习/配置彼此网络地址
- **无需支持**的功能：无需差错纠正/恢复；无需流量控制；不存在乱序交付；无需支持多点链路。差错恢复、流量控制等由高层协议处理。

### 5.5.2 PPP数据帧

![image-20201030185824316](/cn/image-20201030185824316.png)

- **标志(Flag)**：定界符(delimiter)

- **地址(Address)**：无效(仅仅是一个选项)，目前全部取1
- **控制(Control)**：无效；未来可能的多种控制域
- **协议(Protocol)**：上层协议 (eg, PPP-LCP, IP, IPCP, etc)
- **信息(info)**：上层协议分组数据
- **校验(check)**：CRC校验，用于差错检测

> 通过协商，可以省略地址字段和控制字段等字节，一个PPP数据帧最多可以**节省5个字节**的长度：地址字段1字节、控制字段1字节、协议字段1字节、校验字段2个字节。

### 5.5.3 字节填充

数据透明传输要求数据域必须允许包含标志模式<01111110>

发送端：在数据中的<01111110>和<01111101>字节前添加额外的字节<01111101> (“填充(stuffs)”)

接收端：

- 单个字节<01111101>表示一个填充字节；

- 连续两个字节<01111101>：丢弃第1个，第2个作为数据接收；

- 单个字节<01111110>：标志字节

> 综上，字节<01111101>相当于转义符，如果在数据域中出现了标志字节<01111110>或者转义符自身，就需要在原字节之前加一个转义符，将它转成原本的含义，而非特殊含义“标志”或“转义”。

在交换网络层数据之前，PPP数据链路两端必须：

- 配置PPP链路：最大帧长；身份认证(authentication)；协商配置各个可变字段的长度；etc.

- 学习/配置网络层信息：对于IP协议，通过交换IPCP协议 (IP Control Protocol )报文 (IP分组首部的“上层协议”字段取值: 8021)，完成IP地址等相关信息配置
