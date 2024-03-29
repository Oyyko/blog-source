
---
title: 讲一讲 congestion control 的方法
date: 2022-04-18
tag: 
- 面试
category: 面试
mathjax: false
---
讲一讲 congestion control 的方法
<!--more-->

## 讲一讲 congestion control 的方法

流量控制：用来解决发送方和接收方的速度不匹配的问题。

拥塞控制：防止过多数据注入到网络中，从而路由器或者链路过载

方法：

1.慢开始

2.拥塞避免

3.快重传

4.快恢复

TCP的拥塞控制采用的是窗口机制，通过调节窗口的大小实现对数据发送速率的调整。

TCP的发送端维持一个称为拥塞窗口cwnd的变量，单位为字节。
为了防止拥塞窗口cwnd增长过大引起网络拥塞，还需要设置一个慢开始门限ssthresh状态变量
1.当cwnd < ssthresh时，使用上述的慢开始算法
2.当cwnd > ssthresh时， 停止使用慢开始算法而改用拥塞避免算法
3.当cwnd = ssthresh时 ， 即可以使用慢开始算法，也可以使用拥塞避免算法。
什么是RTT
1、一个传输轮次所经历的时间其实就是往返时间RTT（RTT并非恒定的数值）
例如，拥塞窗口cwnd的大小是4个报文段，那么这时的往返时间RTT就是发送方连续发送4个报文段，并收到这4个报文段的确认，总共经历的时间。
2、使用“传输轮次”是更加强调：把拥塞窗口所允许发生的报文段都连续发送出去，并收到了对已发送的最后一个字节的确认
3、在TCP的实际运行中，发送方只要收到一个对新报文段的确认，其拥塞窗口cwnd就立即加1，并可以立即发送新的报文段，而不需要等这个轮次中所有的确认都收到后再发送新的报文段。

**2、慢开始流程：**

- 1、 在主机刚刚开始发送报文段时可先设置拥塞窗口 cwnd = 1，即设置为一个最大报文段 MSS 的数值。
- 2、 在每收到一个对新的报文段的确认后，每经过一个传输轮次，拥塞窗口cwnd就加倍，即增加 MSS 的数值



拥塞避免算法：
当拥塞窗口的值增加到ssthresh时，就要减缓拥塞窗口的增长速度。

1、具体的做法是每经过一个RTT，拥塞窗口cwnd的值加1（单位为MSS），这样就可以使cwnd按线性规律缓慢增长。
2、只要发送方判断网络出现拥塞（其根据就是没有按时收到确认），就要把慢开始门限 ssthresh
设置为出现拥塞时的发送方窗口值的一半（但不能小于2）。
3、然后把拥塞窗口 cwnd 重新设置为 1，执行慢开始算法。

快重传算法规定，发送方只要一收到3个重复确认，就知道接受方确实没有收到报文段M3，因而应当立即进行重传（即“快重传”），这样就不会出现超时，发送方也不就会认为出现了网络拥塞。



当发送端连续收到三个重复确认时，就将慢开始门限ssthresh减半，以预防网络拥塞的发生，并且将拥塞窗口cwnd的值置为减半后的ssthresh，然后开始执行拥塞避免算法，使得cwnd缓慢地加性增大。



快恢复的两个特点

1、当发送方连续收到三个重复确认时，就执行“乘法减小”算法，把慢开始门限减半，这是为了预防网络发生拥塞。
2、由于发送方现在认为网络很可能没有发生拥塞，因此现在不执行慢开始算法，而是把cwnd值设置为慢开始门限减半后的值，然后开始执行拥塞避免算法，是拥塞窗口的线性增大。

**快恢复和慢开始的区别：**

慢开始算法只是在TCP建立时才使用，快恢复是在遇到网络拥塞接收不到数据时触发，常常伴随着快重传算法。

## 拥塞控制和流量控制的区别

1、拥塞控制是防止过多的数据注入到网络中，可以使网络中的路由器或链路不致过载，是一个全局性的过程。
2、流量控制是点对点通信量的控制，是一个端到端的问题，主要就是抑制发送端发送数据的速率，以便接收端来得及接收