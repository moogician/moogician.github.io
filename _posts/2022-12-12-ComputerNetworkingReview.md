---
layout: post
title: Review of Computer Network
date: 2022-12-12 20:00:00
description: final review of the course Computer Network in PKU
tags: network
categories: course-note
---

# 计算机网络复习

## 应用层

好处：
- 屏蔽底层细节
- 抽象
- 提供额外的功能

Socket编程
- 进程通过socket完成网络通信
- 真正的并发是开很多socket（单socket多进程要冲突控制）

组织架构
- 客户/服务器(C/S)方式
- 对等(P2P)方式

### 协议

#### WWW

- Web页面（html）：包含到多种对象的链接
- Web对象：可以是图片，音频，视频等
    - 静态对象：多媒体，css
    - 动态对象：交互信息，如注册信息等
- 对象用URL编址
- 动态Web：用CGI协议等，前后端分离，数据库+脚本

#### HTTP

- 缺省使用TCP的80端口
- 无状态协议
- 不同标准
    - HTTP/1.0: 无状态，非持久连接
    - HTTP/1.1: 支持长连接，流水线机制和断点冲脸
    - HTTPS: 加入TLS保证安全
    - HTTP/2.0: 提高带宽利用率，降低延迟，服务端可以主动推送消息确定存活
- 服务过程：三次握手，四次分手
- 请求报文结构
    - 开始行 (请求叫请求行，响应叫状态行)
        - 方法：GET，POST（提交数据）
        - 版本
        - URL / 状态码 
            - 200 OK
            - 301 MOVED
            - 400 BAD REQUEST
            - 404 NOT FOUND
            - 505 HTTP VERSION NOT SUPPORTED
    - 首部行：用key: value记录控制信息
    - 实体主体
- 缓存：浏览器缓存和代理缓存
    - 可通过询问原始服务器保证副本一致
- Cookie：存储在用户主机，保存服务器发来的信息
    - 只保存文本，不被执行

#### DNS

- 提供域名和ip的转换
    - 网络层服务，但以应用层方式实现
- 采用层次结构
    - 下层服务器没有数据的时候就往上层问
    - 根服务器和顶级域服务器不一定有数据，但二级域服务器一定有其管辖区的数据
    - 管辖区可能小于管辖域（所有使用当前级别域名的域名）
- 使用UDP端口53：减少开销
- 域名查询方式
    - 递归查询(代替询问者查询)：主机向本地服务器
    - 迭代查询(返回新服务器的IP)：本地服务器向更上级服务器
    - 先通过递归查询到本地域最顶层，在从根往下迭代查询
- 报文：基础结构，问题，资源记录
- 不安全：所有流量明文传输，没有身份验证机制和签名
    - 通信链路窃听
    - 服务器收集

#### 电子邮件

- 基本架构：用户代理->传送代理
- SMTP：传送邮件
    - 不包括认证
    - 明文
    - 只能发ASCII
- POP3：接收邮件
    - 拉操作（SMTP一直需要目标在线）
- IMAP：接收邮件
    - 更加复杂，维护文件夹（POP3删除）
    - 用户状态信息，可以访问邮件一部分

#### P2P

- 去中心化
- 核心问题：peer索引
    - 中心化索引
        - 单点故障
        - 性能瓶颈
    - 洪泛索引
        - 找不到就直接往所有邻居发询问
        - 信息太多，可扩展性有限
    - 混合索引
        - 层次化，超级节点间flooding
- BitTorrent
    - 一个实际的P2P协议
    - 可能自私退出
    - 优化策略：
        - 找罕见的块下载
        - 找速度匹配的peer
        - 投入越多回报越高
    - 问题多：速度慢，版权，不共享，恶意节点
- 分布式哈希表
    - 不需要中心化就能查询数据在哪
    - Chord
        - 顺时针排，key由下一个顺时针的peer存储
        - 问题：负载均衡/能力不一样
        - 可以划分更多虚拟节点

#### RTSP, RTP, RTCP

- 用于流媒体传输的控制协议
    - 服务质量（平滑性，画质等）很重要
- RTP
    - 提供实时传输，但没有任何保证
    - 不对数据块做任何处理
- RTSP
    - 多媒体播放控制
    - 控制暂停继续，前进后退等
- RTCP
    - 和RTP配合
    - 保证服务质量，同步等

## 传输层

- Socket是应用层和传输层间的接口
- 端口号：16bit
    - 熟知端口：0-1023
    - 注册端口：1024-49151
    - 私有或动态端口：49152-65535
- 复用分用
    - 复用：打包将不同端口的东西一起传送
    - 分用：来自不同端口的包送到不同socket

### UDP

- 不包含任何保证和流量控制
- 交付的套接字只和报文中目的端口号和IP有关，和源没关系
- 有边界，以报文为单位
- 可选检查报文完整性
    - checksum：16bit和取反（checksum=0）
- 无发送缓冲区，有接收缓冲区
- 发的快，开销小，处理简单
    - 适合流媒体等容忍丢包的应用
    - 适合以单次请求响应为主的应用
    - 或者应用层保证可靠

### TCP

- 有连接，套接字和源及目的都有关
- 流传输，没有边界
- 传输触发条件：收到数据，应用程序调用，超时
- 报文结构
    - 主要是有seq，checksum，port等
    - 重要选项
        - 最大段长度
        - 窗口比例因子
        - 选择重传
- 主要机制
    - 建立连接
        - 三次握手
            - SYN, SYNACK, ACK
            - 保证服务器和客户端都在线
            - 起始序号不能重叠：时钟
        - 四次分手
            - 双方各自FIN, ACK
            - 保证服务器和客户端都离线
            - 优化：两个可以一起发送
            - 丢包就重传，多次不行就放弃
        - 安全隐患
            - flooding发SYN占资源
            - TCP端口扫描：通过发SYN或FIN得知端口是否被占用
    - 可靠数据传输
        - 一般可靠性
            - 重传出错数据包：数据包号seq+ACK
            - 计时器防止丢包
        - 优化:流水线传输（GBN/SR）
            - 滑动窗口限制未确认包上限
            - 回退N（GBN）
                - 发送端：维护一个计时器，到时间重传所有未确认的
                - 接收端：不缓存包
                - 优点：减轻接收端负担
                - 缺点：增加发送端和信道负担
            - 选择重传（SR）
                - 发送端：给每个包维护单独的计时器，只重传单个
                - 接收端：缓存出错包后的包/乱序包
                - 优点：减少重传
                - 缺点：缓存，单独的计时器
            - SR的窗口大小不能超过seq一半，GBN则不能超过seq大小
        - TCP的可靠性
            - 定时器和确认方式与GBN类似
            - 重发策略和缓存与SR类似
            - 减少不必要的重传
            - 优化1：超时值的设置-Karn算法
                - 移动指数加权平均测量平均RTT
                - 由于方差大，加一个安全距离衡量方差
                - 只考虑一次发送成功的包
                - 发生重传就直接超时翻倍直到上界
            - 优化2：快速重传
                - 三次重复ACK马上重传
            - 优化3：推迟确认
                - 接收方可以在收到多个报文后发送延迟确认
                - 为了保证可靠，至少每隔一个报文段正常确认
                - 上限500ms
    - 流量控制
        - 发送端控制发送速率，避免接收缓存溢出
        - 由于又要可靠又有缓冲区，才存在流量控制（比较GBN/SR和UDP）
        - 接受缓存的剩余空间叫接收窗口
        - 非零窗口通告
            - 接收方在接收窗口为0时告诉发送端
            - 由于传输限制（3条件），只能发送端发探测看接收窗口是否为0
            - 零窗口探测靠超时
            - 接收端返回接收窗口的大小
            - 糊涂窗口综合症：不断发小窗口通告并被填满
                - 发送方等等再发送：Nagle算法启发式
                - 接收方推迟确认且等接收窗口显著增大再说(达到MSS或缓存的一半)
    - 拥塞控制
        - 流量控制考虑接收端，拥塞控制考虑网络能力
        - 大量资源用于不必要重传和被丢掉的包
        - 传统TCP不考虑网络层的额外信息，自行推断拥塞
        - 发送方利用丢包感知拥塞
            - 超时
            - 3ACK
        - 发送方通过拥塞窗口cwnd限制发送速率
        - AIMD：乘性减少，加性增加拥塞窗口大小
            - 丢一次包cwnd减半：缓解拥塞
            - 过一个RTT将cwnd增大一个MSS：避免震荡
        - 调整cwnd
            - 慢启动：cwnd太小时候指数级增加cwnd
                - cwnd初始为1MSS
                - 加性太慢了
                - 比没拥塞控制慢。。。
                - 直到ssthresh或丢包
            - 拥塞避免：cwnd超过ssthresh就线性增长
                - 超时：网络传输能力极差
                - 3ACK：海绵里的水挤一挤总还有
                - Tahoe不区分这两个，直接重新慢启动
                - Reno在3ACK的时候进入快速恢复
                - 两个算法都是令ssthresh = cwnd/2
            - 快速恢复
                - 一开始令cwnd=cwnd/2+3, 即cwnd比ssthresh多3
                - 继续收到相同的ACK：每次cwnd多MSS
                - 新ACK：cwnd=ssthresh然后拥塞避免
                - 丢包时：重新开始
        - 吞吐量
            - 丢包时窗口大小W，忽略慢启动
            - 丢包前吞吐W/RTT，丢包后W/2RTT
            - 平均约为0.75W/RTT（分析见图）

<div class="row mt-3">
<div class="col-sm mt-3 mt-md-0">
    {% include figure.html path="assets/img/blog/tcp-throughput.png" class="img-fluid rounded z-depth-1" %}
</div>
</div>

        - 公平性
            - 多个TCP共享同一个瓶颈链路，速度应该相同
            - 公平性来自于AIMD（见下图）

<div class="row mt-3">
<div class="col-sm mt-3 mt-md-0">
    {% include figure.html path="assets/img/blog/tcp-fairness1.png" class="img-fluid rounded z-depth-1" %}
</div>
<div class="col-sm mt-3 mt-md-0">
    {% include figure.html path="assets/img/blog/tcp-fairness2.png" class="img-fluid rounded z-depth-1" %}
</div>
<div class="col-sm mt-3 mt-md-0">
    {% include figure.html path="assets/img/blog/tcp-fairness3.png" class="img-fluid rounded z-depth-1" %}
</div>
<div class="col-sm mt-3 mt-md-0">
    {% include figure.html path="assets/img/blog/tcp-fairness4.png" class="img-fluid rounded z-depth-1" %}
</div>
</div>
    
- 拥塞控制改进
    - New Reno：快速重传后若还是不行则马上重传第一个未被确认的包
    - BIC和CUBIC：更快找到合适的cwnd大小 
        - BIC使用二分查找逼近Wmax
            - 如果有新的Wmax就对称指数找新的Wmax
            - 过于复杂
            - 不公平（RTT越短cwnd增长越快）
        - CUBIC：窗口增长函数只和距离上次丢包时间有关
            - 和RTT无关
            - 用三次函数逼近BIC曲线
    - BBR：以瓶颈链路带宽*往返时间作为目标（不要把缓冲区填满）
    - DCTCP：面向数据中心
        - 问题：总流量大(incast)，端口占用(queue buildup)，缓冲区占用(buffer pressure)
        - 更精细的窗口
        - 交换机：队列长度超N时给之后的包标记ECN
        - 接收端：只有出现或消失ECN时直接ACK，否则delay ACK
        - 发送端：每个RTT更新cwnd，根据ECN ACK/总ACK数动态调整
        - 队列长度稳定且低

### QUIC
- TCP存在的问题
    - 应用无法修改TCP
    - 握手时延大（要三个RTT）
    - 队头阻塞（丢包复杂，多路复用慢）
- 架构：基于UDP，替代TCP，TLS和部分HTTP，模块化拥塞控制
- 优势
    - 解决了队头阻塞问题
    - seq更明确，RTT更精确
    - 支持ip/port切换
    - 容易部署更新



### 新型技术
#### BIC和CUBIC

#### BBR
#### DCTCP
#### QUIC







