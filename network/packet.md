# 网络收包发包过程

## 收包流程

1.网卡收到网络帧后，通过DMA，直接将网络报放到收包报文中；然后触发硬件中断收到包 
2.网卡中断程序为网络帧分配内核数据结构（sk_buff），并copy 到sk_buff缓冲区中，再出发软中断,通知内核收到网络包 
3.内核从缓冲区中取出网络帧，通过协议栈解析 
4.协议栈解析： 
（1）数据链路层：检查报文合法性，找出上层协议类型，去掉帧头和帧尾，交给网络层 
（2）网络层：判断DIP是本机还是转发，如果不是本机，查找路由表转发（ip_forward=1），若是本机，找出协议类型（TCP/UDP），去掉IP头，交给传输层处理 
（3）传输层：根据四元组（DIP/DPORT/SIP/SPORT），找出对应的socket，将数据copy到socket接收缓冲区 
5.应用层读出socket的数据，进行处理 

## 发包流程

1.应用程序组装数据，调用socket API（如sendmsg）发送网络包 
2.通过系统调用，进入到内核态的socket层中，socket层将数据放到socket的发送缓冲区 
3.网络协议栈从socket的发送缓冲区取出数据，通过TCP/UDP、IP（查路由）封装头，并按MTU进行分片 
4.分片后的数据包送到网络接口层，进行物理寻址，找到DMAC（APP学习），封装ETH层，再添加帧尾，放到发包队列中 
5.触发软中断，通知驱动程序：发包队列中有新的数据包需要发送 
6.驱动程序通过DMA，从发包队列中读出数据帧，copy到物理网络发送出去