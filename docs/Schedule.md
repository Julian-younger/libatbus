# Schedule List

+ [ ] endpoint短线重连后的重发机制
  + [ ] endpoint断开后短期内不移除（pending_endpoint_gc_list 也采用定时器，影响单元测试的析构判定，也需要做相应变更）
  + [ ] custom command和forward协议增加和endpoint绑定的sequence，用于对重发的消息回调去重（其他协议不需要去重）
  + [ ] 根据hash code、hostname、pid允许重置和endpoint绑定的sequence
  + [ ] endpoint握手仅连接最快可访问的数据链接(ios连接需要排队挨个重试)，需要防止正在连接导致的误判，需要处理正在连接过程中GC生效的问题
  + [ ] ios channel允许重绑定写出缓冲区管理器，和endpoint内共享。
  + [ ] buffer_manager增加atomic的private_guard用于CAS操作处理正在写出的独占connection。connection释放时要解除占用。endpoint定期检查独占有效性（防止死锁）
  + [ ] connection握手成功后需要开始尝试重发和endpoint内共享的缓冲区
  + [ ] ios channel在写出完成的会调用发现需要关闭connection或正在关闭connection不再清理buffer_manager
  + [ ] 增加ios channel拆包接口，用于处理如果endpoint丢弃时有些包需要通知发送失败的回调+单元测试
  + [ ] 兄弟节点endpoint在进入gc列表时定期自动重连（父子节点会由子节点自动重连）（node非CLOSING时）
  + [ ] 已绑定endpoint的connection在收到注册协议时要检查endpoint信息是否和原来一致。不一致说明同地址换endpoint了，需要移除endpoint的无效地址缓存，并重置connection的binding_
  + [ ] 已绑定endpoint的connection在收到注册协议时要检查endpoint的数据节点是否和上报地址匹配，不一致说明同地址换endpoint了，移除endpoint的无效地址缓存，并移除无效的connection后需要重新发起endpoint的数据通道连接流程
  + [ ] endpoint的数据通道全部离线后不再下线，而改为进入GC检查列表后尝试重新发起endpoint的数据通道连接流程
  + [ ] endpoint的控制通道全部离线后不再下线，而改为进入GC检查列表后尝试重新发起endpoint的控制通道连接流程
  + [ ] ios channel切换到shm/mem channel后发送的data/cmd消息要重排序,并且要处理shm/mem channel拥塞后的自动重发
  + [ ] 增加消息超时机制，用于处理重发和GC机制联动导致的消息事件重复触发，forward_response事件增加不确定是否成功的错误码
  + [ ] 共享connection支持
  + [ ] 全协议代理机制,proxy mode
  + [ ] gRPC连接层
