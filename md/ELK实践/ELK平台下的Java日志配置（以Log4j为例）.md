# ELK平台下的Java日志配置（以Log4j为例）


## 1.Log4j配置文件的配置
    
    # 开发日志将在本地输出，并输出SQL
    log4j.rootLogger=${log4j.leve},A1,DRF,DE
    
    # 设置以下包的日志信息不输出
    #log4j.logger.org.springframework=OFF,A1,DRF,DE
    #log4j.logger.com.alibaba.dubbo=OFF,A1,DRF,DE
    #log4j.logger.org.apache.zookeeper=OFF,A1,DRF,DE

    # 设置com.dolceviva包下的日志输出等级和目的地，这里输出到ELK系统中
    log4j.logger.com.dolceviva=INFO, socket

    # 将日志信息网络传输到ELK系统
    log4j.appender.socket=org.apache.log4j.net.SocketAppender
    log4j.appender.socket.Port=4567
    log4j.appender.socket.RemoteHost=192.168.1.117
    log4j.appender.socket.layout=org.apache.log4j.PatternLayout
    log4j.appender.socket.layout.ConversionPattern=[%-5p][%d{yyyy-MM-dd HH:mm:ss,SSS}][%t:%r][%C{1}:%L] %m%n
    log4j.appender.socket.ReconnectionDelay=10000

    # 输出到控制台
    log4j.appender.A1=org.apache.log4j.ConsoleAppender
    log4j.appender.A1.layout=org.apache.log4j.PatternLayout
    # log4j.appender.A1.layout.ConversionPattern=%d %5p [%t] (%F:%L) - %m%n
    log4j.appender.A1.layout.ConversionPattern=%d %5p [%F:%L] : %m%n
 
	# 输出到本地日志文件（INFO等级的信息）	
    log4j.appender.DRF=org.apache.log4j.DailyRollingFileAppender
    log4j.appender.DRF.Threshold=INFO
    log4j.appender.DRF.DatePattern='.'yyyy-MM-dd
    log4j.appender.DRF.File=/var/tomcat7/logs/dolceviva-service-order-business-info.log
    log4j.appender.DRF.Append=true
    log4j.appender.DRF.layout=org.apache.log4j.PatternLayout
    log4j.appender.DRF.layout.ConversionPattern=[%-5p][%d{yyyy-MM-dd HH:mm:ss,SSS}][%t][%C{1}:%L] %m%n

    # 输出到本地日志文件（ERROR等级的信息）
    log4j.appender.DE=org.apache.log4j.DailyRollingFileAppender
    log4j.appender.DE.Threshold=ERROR
    log4j.appender.DE.DatePattern='.'yyyy-MM-dd
    log4j.appender.DE.File=d:\\logs\\dolceviva-service-order-error.log
    log4j.appender.DE.Append=true
    log4j.appender.DE.layout=org.apache.log4j.PatternLayout
    log4j.appender.DE.layout.ConversionPattern=[%-5p][%d{yyyy-MM-dd HH:mm:ss,SSS}][%thread][%C{1}:%L] %m%n

    # 输出SQL 
    log4j.logger.com.ibatis=${log4j.ale}
    log4j.logger.com.ibatis.common.jdbc.SimpleDataSource=${log4j.ale}
    log4j.logger.com.ibatis.common.jdbc.ScriptRunner=${log4j.leve}
    log4j.logger.com.ibatis.sqlmap.engine.impl.SqlMapClientDelegate=${log4j.ale}
    log4j.logger.java.sql.Connection=${log4j.ale}
    log4j.logger.java.sql.Statement=${log4j.ale}
    log4j.logger.java.sql.PreparedStatement=${log4j.ale}
    org.springframework = ${log4j.ale}
    org.apache.zookeeper = ${log4j.ale}

## 2.Java代码记录日志信息
*应用中不可直接使用日志系统 （Log 4 j 、 Logback） 中的 API ，而应依赖使用日志框架
SLF 4 J 中的 API ，使用门面模式的日志框架，有利于维护和各个类的日志处理方式统一。*

`例：`

    import org.slf4j.Logger;
    import org.slf4j.LoggerFactory;
    private static final Logger LOGGER = LoggerFactory.getLogger(OrderServiceImpl.class);
    ...
    // 记录业务日志
    LOGGER.info(
		    "create order with id:{},memberId:{},memberEmail:{},payStatus:{},paymentMethod:{},shippingFee:{},goodTotal:{},orderFee:{},currency:{},createTime{}",
		    orderId, order.getMemberId(), order.getMemberEmail(), order.getPayStatus(),
		    order.getPaymentMethod(), order.getShippingFee(), order.getGoodsTotal(), order.getOrderFee(),
		    order.getCurrency(), order.getCreateTime());
    ...
    // 记录异常堆栈信息日志
    LOGGER.error("Exception happend: ",e);