# NoSQL
非关系型数据库
# redis
需要在linux操作系统中使用，由C语言开发的一款内存高速缓存数据库，数据模型key-value，有丰富的数据结构
## 简介
缓存：

    1.数据缓存：经常用在CMS(content manage system)内存管理系统里面（内容读取单一、集中）
    2.页面缓存：经常会用在页面的具体数据里面（各个数据比较独立）
redis和memcache区别：<br>
Redis支持的数据类型多，Redis支持(master-slave)主-从模式，支持数据持久化，可以将内存中数据保持在磁盘中，重启时就可以再次加载<br>
Redis单个value最大限制为1GB，memcache只能保存1MB数据<br>
## redis安装与启动
安装：

    1）安装redis编译的c环境，yum install gcc-c++
    2）将redis-xx.tar.gz上传到Linux系统中
    3）解压到/usr/local下  tar -xvf redis-x.tar.gz -C /usr/local
    4）进入redis解压目录 使用make命令编译redis
    5）在redis-解压目录中 使用安装命令：make PREFIX=/usr/local/redis install ，作用是redis到/usr/local/redis中
    6）拷贝redis-xx中的redis.conf到安装目录redis中
    7）启动redis 在bin下执行命令 ./redis-server redis.conf
    8）打开端口，/sbin/iptables -I INPUT -p tcp --dport 6379 -j ACCEPT
启动：./redis-server前台启动；./redis-server redis.conf后台启动并使用reids.conf配置文件<br>
使用：./redis-cli<br>
关闭：./redis-cli shutdown<br>
## redis使用
参考ssm_01_firstRedis项目<br>
使用java连接redis数据库<br>
一般都使用set("键","字符串类型的值");进行存储，其他类型可以转化成json串
```java
    这个是单独连接数据库
    private static void Test1(){
        //第一个为IP地址，第二个为端口号
        Jedis jedis = new Jedis("192.168.181.128",6379);

        String name = jedis.get("name");//获取redis数据库中的数据
        System.out.println(name);

        jedis.set("addr","chi");//使用set()方法进行存储
        System.out.println(jedis.get("addr"));
    }
```
用连接池的方式操作数据库
```java
package henhaochi.javaredis;

import redis.clients.jedis.Jedis;
import redis.clients.jedis.JedisPool;
import redis.clients.jedis.JedisPoolConfig;

import java.io.IOException;
import java.io.InputStream;
import java.util.Properties;

/**
 * @Author: 中国很好吃
 * @Description: 连接池工具
 * @Date: 10:12 2020.8.28
 */
public class PoolUtils {

    private static JedisPool pool = null;//创建一个池对象

    static {
        //从reids.properties这个配置文件中获取相关数据
        InputStream in = JedisPool.class.getClassLoader().getResourceAsStream("redis.properties");
        Properties pro = new Properties();
        try {
            pro.load(in);
        } catch (IOException e) {
            e.printStackTrace();
        }
        //获得池对象
        JedisPoolConfig config = new JedisPoolConfig();
        config.setMaxIdle(Integer.parseInt(pro.get("redis.maxIdle").toString()));//虽大闲置个数
        config.setMinIdle(Integer.parseInt(pro.get("redis.minIdle").toString()));//最小闲置个数
        config.setMaxTotal(Integer.parseInt(pro.get("redis.maxTotal").toString()));//最大连接数
        //创建数据库连接
        pool = new JedisPool(config,pro.get("redis.url").toString(),Integer.parseInt(pro.get("redis.port").toString()));
    }

    //返回redis资源，然后对redis数据库中的数据进行操作，操作完成之后要关闭资源，即jedis.close();
    public static Jedis getJedisPool(){
        Jedis jedis= pool.getResource();
        return jedis;
    }
}
```
## redis数据类型
键值对类型，key不要太长或太短，要有规范<br>
删除数据：del key<br>
### String类型
自增只能对数值类型数据进行操作
    
    自增：incr key 当key不存在的时候，创建这个key=0然后进行操作，查询这个key的值为1
    自减：decr key 当key不存在的时候，创建这个key=0然后进行操作，查询这个key的值为-1
    增加：incrby key number key=key+number，key的新值为key+number值
    减少：decrby key number key=key-number
拼接字符串：append key "value"
### Hash类型
存储形式
|键|hash键|值|
|:-:|-:|-:|
|keyname1|key11|value1|
||key12|value2|
|keyname2|key21|value1|
||key22|value2|
存取

    单个存储值：hset keyname key(二级) value
    多个存储值：hmset keyname key1(二级) value1 key2(二级) value2
    单个获取值：hget keyname key
    多个存储值：hmget keyname key1(二级) key2(二级)
    获取全部数据：hgetall keyname
    删除数据：del keyname
其他

    获取keyname长度：hlen keyname 
    显示keyname中对应的值：hkeys keyname 
    显示keyname中对应的值：hvals keyname
    判断keyname中的key对应的值是否存在：hexists keyname name
### list类型
存储形式：先找到key的值key1，然后再取值<br>
|key1|value11 value12|<br>
|key2|value21 value22|<br>
栈：先进后出

    lpush key1 value1 value2 值从左边开始压栈，value1 value2顺序入栈
    rpush key1 value1 value2 值从右边开始压栈，value2 value1顺序入栈
    lpushx key1 value1 value2 判断key1是否为空，不为空的时候从左边开始压栈
    linsert key1 after/before value value1 在value后/前插入数据value1
    lrange key1 start end 从key1中查询值，从start开始到end结束，start为数字0代表第一个，1代表第二个，end为负数-1代表最后一个，-2代表倒数第二个
    llen key1 查询key1中数据个数
    lpop key1 把栈中最左边的数据弹出 
    rpop key1 把栈中最右边的数据弹出
    lrem key1 x y x为正数，从左开始删除x个y；x为负数，从右开始删除x个y
    lset key1 x y 把key1位置为 x 的值替换成y
    rpoplpush keyx keyy 把keyx链表尾部的值取出添加到keyy链表头部
### set类型
默认是不能重复，且无序<br>
|key1|value11 value12|<br>
|key2|value21 value22|<br>

    sadd myset value1 value2 value3 把value1、value2、value3添加到myset中，无序且不可重复
    srem myset value1 删除myset中的value1数据
    smembers myset 返回myset中全部的值
    sismember myset value 查询myset中是否存在value，存在为1，不存在为0
    scard myset myset中值的个数
    srandmember myset 随机返回myset中的一个值
    sdiff myset myset2返回myset中有的值但myset2中没有的值
    sdiffstore myset3 myset myset2把myset中有的值但myset2中没有的值存放到myset3中
    sinter myset myset2返回myset 和myset2共有的部分
    sinterstore myset4 myset myset2把myset和myset2共有的部分添加到myset4中
    sunion myset myset2返回myset和myset2的并集
    sunionstore myset5 myset myset2把myset和myset2并集存放到myset5中
### sortedset
数据通过权重排序，

    zadd mysort key mumber key1 mumber1 给mysort中添加数据
    zrange mysort start end 无序查询mysort中数据
    zrange mysort start end withscores 按从小到大顺序排序显示mysort中的数据
    zcard mysort 查询mysort中数据条数
    zscore mysort key 查询mysort中key对应的值
    rangebyscore mysort start end limit start1 end1 查询mysort中范围在start到end之间数据，排序从start1（从0开始）到end1
    zrank mysort key 返回key对应的排名，从0开始
    zremrangebyrank mysort start end 删除范围在start到end之间的数据
    zremrangebyscore mysort min max  删除值范围在min到max之间的数据
### key的通用部分

    keys xx* 查询名字中以xx开头的数据，xx可以不写
    del name 删除名为name的redis数据，一次可以删除多个
    exists name 查询名为name的redis数据是否存在
    rename oldname newname 重命名
    expire name time 给name设置过期时间为time秒
    ttl name 查询name剩余的过期时间并返回，未设置过期时间返回-1，不存在name返回-2
    type name查询name的类型
## redis特性
### 多数据库
一个redis数据库中有多个数据库<br>
move name 1 把name这个数据转移到库号为1这个数据库中，select 1 切换到库号为1的数据库中，然后进行其他操作<br>
flushall：删除数据库中所有key
### 订阅和事务
subscribe 名字 订阅名字这个系统<br>
publish 名字 内容 发布这些信息<br>
事务特点：类似于批量处理，第一条错误，不影响第二条操作，回滚所以操作失效<br>
multi开启事务，exec提交事务，discard回滚
### 持久化
RDB以快照形式的数据存储到磁盘中，AOF以日志的形式把数据存储到磁盘中