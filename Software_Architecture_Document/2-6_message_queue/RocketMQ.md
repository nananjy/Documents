# RocketMQ

## RocketMQ集群模式与广播模式
1. 集群模式
    1. 一条消息只能被任意消费者消费一次
    2. 设置 consumer.setMessageModel(MessageModel.CLUSTERING);

2. 广播模式
    1. 一条消息被所有消费者都消费一次
    2. 设置 consumer.setMessageModel(MessageModel.BROADCASTING);








