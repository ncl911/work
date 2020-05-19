## xxl-job
只需要在启动类加上@EnableXxlJob注解就好，不需要另写一个配置类专门加这个注解（商品中心）

## 商品中心uitl包下的CommonUtil类
其中做了ThreadLocal的longinSession，用于存储session中的一些公共值，用于其他类的调用，这属于重复工作，RequestHolderUtil可以获取对应的公共值；
而且不建议这么做，Threadlocal没有进行释放，会导致内存溢出

## 商品中心uitl包下的Parse类，获取field注解优化
可以根据注解类型直接获取对应的注解，不需要获取所有的注解去循环

## 商品中心uitl包下的TableNameUtil类
该类定义的是表名，应属于constants类，且static final定义的属性应用大写下划线

## 商品中心uitl包下的WareLoginInfo类
该类继承自BaseController用于获取登录信息，属于重复工作，且对groupId以及companyId的类型进行了转换，Long->Integer，不知为啥

## 商品中心uitl包下的WareUtil类
这类中的方法是否可以合并到对应的service类中或者manager类中，这并不属于util类型

## 商品中心com.hydee.h3.ware.common.config
该包中定义的是不同rocketmq sender的实例配置类，每一个sender一个配置类
建议：写一个专门的sender配置类，统一生成sender实例

## controller后的层级类中的方法问题
需要什么参数尽量在方法上写明，不要从公共的地方取参数，应该从上层传进来

## 校验方法应该直接抛出参数异常，而不是返回null在业务代码中判断null

## 校验方法里面对校验参数进行修改问题
不建议！！！

## 参数校验类
建议：涉及数据库数据的校验不放参数校验类，参数校验类方法可都为static，不用交由容器管理


# 底层基础包

## rocketmq
- 包名不一致
- 只有事务生产者？？？
- 消费者中实例化一个生产者？？？
- 只有并行消费者（concurrently），没有串行消费者（orderly）
- 某些写法有点小问题
- 暴露的参数不够 

## distribute-id
redis做workerId没用上，如果不需要redis，建议给每个业务线设定自己的workerId，引用的工程自己配置一个snowflake实例并传入对应的workerId

## hd-frame-base
- 跨域配置对于scg网关好像不起作用，scg有专门的配置，对于单独的spring boot或者zuul，可以重写跨域filter，并做成yml配置，比较灵活
- 全局异常处理handler建议各个异常的处理方法分开写，扩展性比较好
- 拦截器建议写一套通用的拦截器组件，暴露一个抽象拦截器父类，应用只需要去继承该父类实现自己的拦截器，并在yml中配置对应的拦截参数，而不需要自己去实现WebMvcConfigurer注册自己的拦截器


## validate
- 没有校验list类型数据的校验

## gateway动态路由问题
各服务的routeId存的string，取得时候用得keys命令取，不建议这么做，应该为hash存储。
<!--stackedit_data:
eyJoaXN0b3J5IjpbMTQ4Nzc4NjM2MCwxNTQ3MTkzMjksNjU5NT
IwMTMwXX0=
-->