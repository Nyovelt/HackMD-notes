# ST-CS120: Computer Network
## OSI Model
- https://www.forcepoint.com/zh-hans/cyber-edu/osi-model

## Performance
- Metrics
    - Bandwidth
    - Latency
        - RTT: Round-Trip Time: calc the time of send and recieve. Local because of the avoidance of time shift between devices.
        - Latency $=$ Transmit Delay $+$ Propagation Delay $+$ Queueing Delay 
            - Transmit Delay $=$ TransferSize $/$ Bandwidth
            - Propagtion Delay $=$ Distance $/$ SpeedofSignal
        - Latency $=$ TransferSize $/$ Bandwidth + $\underline{Distance/SpeedofSignal + Queueing ~Delay}_{RTT/2}$
        
## L1: Physical Layer
网线一类的东西
- 介质
- 需要调制

### Upper bound of throughout 
#### Amplitude shift passing
- $A\cos(2\pi ft)\cos(2\pi ft)= \frac12A(cos(2 \pi ft)+1)$
- 乘以载波解码
- 信号不稳定时效果较差
#### Frequencty shift passing 
- 高频部分解码归零
- 低频部分保留

#### Phase Shift Keying
- 乘以载波解码
- $\cos(2\pi ft) \cdot \cos( 2 \pi ft) = \frac 12 (\cos (2 \pi ft)+1)$

#### Quadrature Amplitude Modulation 
- ASK + PSK
- 传输更多的信息 (4 symbols)

总结: Baseband * coherence wave = Passband

### 纠错与恢复
> analog goes wrong
- Bit error rate (BER) = error bits/transmitted bits
- High rate <-> Low reliability
#### 2D Parity Check
0101001 1 \
1101001 0

0111111 x

- overhead $\frac 18$
- 1,2,3 bit OK
- 4 bit may ERROR
#### Check sum
- TCP/IP header
- sum all, check sum
- 补码加起来

#### CRC Performance
多项式


#### 汉明码

#### 多路复用
- 分时复用
- 分频复用
- 传输介质无法承载我们的传输

### ACK
#### Stop and Wait
#### Sliding Window
- 不对等的传输
- Send buffer size: Delay $\cdot$ Bandwidth
- Receive buffer size: Flow control
- 拥塞控制: 控制 buffer 大小

## L2: 链路层，介质访问子层
### Main function
- 连接协议 (e.g. Hola)

### Ethernet
```bash
ip addr show
```
![](https://i.imgur.com/O0cSXSX.png)
### WIFI and Cecullar
https://blog.csdn.net/qq_27847237/article/details/104098817
- 半双工
- 不对称

#### RTS/CTS
通过监听其它 client 之间的通讯来推测 server 与它的连接情况
- RTS/CTS does not solve hidden terminal and exposed terminal
completely
- Designing Wireless MAC is non-trivial
- overhead too large

#### IEEE 802.11 MAC
![](https://i.imgur.com/lALxS28.png)

WIFI 包经过再打包走 Ethernet
- 3-Addr: A -> AP -> C
- 4-Addr: A -> B -> C -> D (mesh)

### Switch
#### learning Bridge
- Switching method
    - Datagram / Connectionless
        - 转发表：记录下来这条链路上的所有主机和对应走哪个端口
        - No Guarantee for
            - Success delivery
            - Performance
            - Delay, Throughput
            - Packet Order
        - 打电话给 ISP
    - Vitual Circuit/Connection
        
        - Procedure
            - 预留资源 (入口与 index)
            - 当链路上的所有 switch 都有足够的资源，destination 会发回一个 ack， 然后链路就建立起来了
    - Source Routing

- Learning Switch
    - 知道 A 从哪来  (port 1)
    - 广播 (X 收到包)
    - 其它节点也知道 A 在哪了
    - 同一个冲突域内 drop