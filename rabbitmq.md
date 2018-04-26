springboot 集成 rabbitmq
原文 https://blog.csdn.net/i_vic/article/details/72742277
1.添加 rabbitmq依赖
 <dependency>  
            <groupId>org.springframework.boot</groupId>  
            <artifactId>spring-boot-starter-amqp</artifactId>  
</dependency>
2.创建rabbitmq配置类

import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.springframework.amqp.rabbit.connection.CachingConnectionFactory;
import org.springframework.amqp.rabbit.connection.ConnectionFactory;
import org.springframework.amqp.rabbit.core.RabbitTemplate;
import org.springframework.beans.factory.config.ConfigurableBeanFactory;
import org.springframework.boot.context.properties.ConfigurationProperties;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.context.annotation.Scope;
/**
 * 
 * rabbitmq配置类
 * @author Administrator
 *
 */

@Configuration  
@ConfigurationProperties(prefix = "spring.rabbitmq")  
public class RabbitMQConfig{  
  
    private static Logger logger = LoggerFactory.getLogger(RabbitMQConfig.class)  ;
  
    private String host;  
  
    private int port;  
  
    private String username;  
  
    private String password;  
  
    // 链接信息  
    @Bean  
    public ConnectionFactory connectionFactory() {  
        CachingConnectionFactory connectionFactory = new CachingConnectionFactory(host, port);  
        connectionFactory.setUsername(username);  
        connectionFactory.setPassword(password);  
        connectionFactory.setVirtualHost("/");  
        connectionFactory.setPublisherConfirms(true);  
        return connectionFactory;  
    }  
  
    @Bean  
    //singleton、prototype、request、session、global session
    @Scope(ConfigurableBeanFactory.SCOPE_PROTOTYPE)  
    public RabbitTemplate rabbitTemplate() {  
        RabbitTemplate template = new RabbitTemplate(connectionFactory());  
        return template;  
    }

	public String getHost() {
		return host;
	}

	public void setHost(String host) {
		this.host = host;
	}

	public int getPort() {
		return port;
	}

	public void setPort(int port) {
		this.port = port;
	}

	public String getUsername() {
		return username;
	}

	public void setUsername(String username) {
		this.username = username;
	}

	public String getPassword() {
		return password;
	}

	public void setPassword(String password) {
		this.password = password;
	}  
    
}
2.application.properties中添加配置信息
#rabbitmq  
spring.rabbitmq.host=192.168.0.166  
spring.rabbitmq.port=5672  
spring.rabbitmq.username=wyf
spring.rabbitmq.password=wyf

3.创建队列


