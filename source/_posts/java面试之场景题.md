---
categories: 整理输出
tags:
  - 面试
sticky: ''
description: ''
permalink: ''
title: java面试之场景题
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/d4a02792-ec3a-400f-8fb0-a22ce204bc3c/anime-Neon-Genesis-Evangelion-2299006-wallhere.com.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466U5FUQULI%2F20251101%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251101T090044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEF8aCXVzLXdlc3QtMiJGMEQCICuD3lEzrcFGOUPsJInNx9KnoH8jpQXysSpy1D1R41iCAiBCtS2l%2BrOHp6BqL%2FiwfHQpI25ykaexTDI1z2CvantDpSr%2FAwgoEAAaDDYzNzQyMzE4MzgwNSIMRxjoyV8k0q%2BCYMxgKtwDE5a6%2B1ZOstyX%2BU4rvtWQEwCqKIyMUSl%2FLl4tvMXDzn7jz7jk1Z448OV372sAndHzJwg0vx66ENOJEkj9cNlSm2PnlMnfiIFnSYN5WXBDk0uMIVdjER96UXawrv%2BxWiSl4iibmdUpdF%2BcXMKKfVC5KuElpIh%2F264tUCSAlIgBVHSCvRbhpF7Gsq0c2zU1GxSCI2a%2Bd5Q6VTP9EgROww4GyeKkGiaYfKZgzdF5j%2ByGrZll2SQnYp%2BoqRWJzkVh7QDGgL%2FKHqMCeFHoLecnzS%2Fyl1zZ1Zg3VhLXc5whBStEJSm4pciXqkOJcxpJkUeEmh9Swm%2BXOPbp7UsVPYbbIJBEyu7XSz%2FNrtjF5gKjySVMRfTSpxvvqk5cgoTY3mih8PNFHoMGzIPbcY7HtDd8ZG1xvzfnIJXS0Y7SQASPcrsWhr%2BTh%2BjnoO9Pmf0ywoe4V0FWW5HnptgIhfV%2F%2ByzEfw6Evfdm%2BqyVLjFBkoEOOXZhI5%2BAwENT8G9wB3nEQoNE4Gx4d0PMDG6lFHC4Yr8snzKEAL7U70MAmwUPRWHSXO67QB9JFpAHLpwxodvplEh5GbD3VNXuAkMybvpW67Fq9rG4LPoGRR%2B670f4r5LyfV2cq9BAd4hqz%2BfXBlASwU8w79CWyAY6pgFHSlCq%2BAfg%2F8%2FVYOrmwS%2F9mQSf98sSZ4nZohWTlurYaySyIDKSrQsJs7hRONHoMLRVHMBj6BSJplkjnzJdzGpR%2FHFZLBeKqm52CrzFeUsKw3enfLc9KVkNujDNhgFF59CPMk4ok0lbOlVcBdamT%2B1VKN4L06begJUpxMVLvFdypU8WZP1GTTqFtVdymzA9A2SWQIZ6hMkSdPjQSaH2YvX%2BNItmws9z&X-Amz-Signature=8a02904a577890915619de3affd1fe11c1e3ad7eddd4262a4f63da78b2bcf00f&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-16 13:54:00'
index_img: /images/15787b0b74cc7dc5b455660dc155b279.jpg
banner_img: /images/15787b0b74cc7dc5b455660dc155b279.jpg
---

# CompletableFuture之控制时间


## 题目


有一个消息发送接口MessageService.send(String message)，每次消息发送需要耗时2ms；


基于以上接口，实现一个批量发送接口MessageService.batchSend(List messages);


要求如下：


1）一次批量发送消息最大数量为100条


2）批量发送接口一次耗时不超过50ms。


3）要求返回消息发送是否成功的结果。


## 解决


思路：将list进行分割，然后遍历分割后的list，创建任务，然后等全部执行完


```xml
<dependency>
 <groupId>com.google.guava</groupId>
```

> guava
> 30.1-jre

```plain text
使用了guava进行list的分割
```


代码：


```java
package com.fang.other;

import com.google.common.collect.Lists;
import java.util.ArrayList;
import java.util.List;
import java.util.concurrent.*;

/**
 * @author fang
 * @version 1.0
 * @date 2025/3/26 14:41:15
 */
public class Test {

    public static void main(String[] args) {
        ArrayList<String> list = new ArrayList<>();
        for (int i = 0; i < 100; i++) {
            list.add(String.valueOf(i));
        }

        // Lists.partition(list, 3);
        // 计算方法耗时
        long start = System.currentTimeMillis();
        batchSend(list);
        long end = System.currentTimeMillis();
        System.out.println("方法耗时：" + (end - start));
    }

    // 线程池
    private static final ThreadPoolExecutor executor = new ThreadPoolExecutor(
        10, 10, 10, TimeUnit.SECONDS, new ArrayBlockingQueue<>(10)
    );

    /**
     * 一次批量发送消息最大数量为100条
     * 批量发送接口一次耗时不超过50ms。
     *
     * @param messages
     * @return
     */
    public static boolean batchSend(List<String> messages) {
        List<List<String>> partition = Lists.partition(messages, 20);
        List<CompletableFuture> futures = new ArrayList<>();
        for (List<String> one : partition) {
            CompletableFuture<Boolean> future = CompletableFuture.supplyAsync(() -> {
                for (String res : one) {
                    if (!send(res)) return false;
                }
                return true;
            }, executor);
            futures.add(future);
        }
        CompletableFuture<Void> allOf = CompletableFuture.allOf(futures.toArray(new CompletableFuture[0]));
        try {
            allOf.get();
        } catch (InterruptedException e) {
            throw new RuntimeException(e);
        } catch (ExecutionException e) {
            throw new RuntimeException(e);
        }
        return true;
    }

    public static boolean send(String message) {
        try {
            Thread.sleep(2);
            return true;
        } catch (InterruptedException e) {
            throw new RuntimeException(e);
        }
    }
}
```


# 一致性hash


首先定义一个节点类，实现数据节点的功能，具体代码如下：


```java
public class ConsistentHash {

    private final TreeMap<Integer, Node> hashRing = new TreeMap<>();
    public List<Node> nodeList = new ArrayList<>();

    /**
     * 增加节点
     * 每增加一个节点，就会在闭环上增加给定虚拟节点
     * 例如虚拟节点数是2，则每调用此方法一次，增加两个虚拟节点，这两个节点指向同一Node
     *
     * @param ip
     */
    public void addNode(String ip) {
        Objects.requireNonNull(ip);
        Node node = new Node(ip);
        nodeList.add(node);
        for (Integer virtualNodeHash : node.getVirtualNodeHashes()) {
            hashRing.put(virtualNodeHash, node);
            System.out.println("虚拟节点[" + node + "] hash:" + virtualNodeHash + "，被添加");
        }
    }

    /**
     * 移除节点
     *
     * @param node
     */
    public void removeNode(Node node) {
        nodeList.remove(node);
    }

    /**
     * 获取缓存数据
     * 先找到对应的虚拟节点，然后映射到物理节点
     *
     * @param key
     * @return
     */
    public Object get(Object key) {
        Node node = findMatchNode(key);
        System.out.println("获取到节点:" + node.getIp());
        return node.getCacheItem(key);
    }

    /**
     * 添加缓存
     * 先找到hash环上的节点，然后在对应的节点上添加数据缓存
     *
     * @param key
     * @param value
     */
    public void put(Object key, Object value) {
        Node node = findMatchNode(key);
        node.addCacheItem(key, value);
    }

    /**
     * 删除缓存数据
     */
    public void evict(Object key) {
        findMatchNode(key).removeCacheItem(key);
    }

    /**
     * 获得一个最近的顺时针节点
     * @param key 为给定键取Hash，取得顺时针方向上最近的一个虚拟节点对应的实际节点
     *
     * @return 节点对象
     */
    private Node findMatchNode(Object key) {
        Map.Entry<Integer, Node> entry = hashRing.ceilingEntry(HashUtils.hashcode(key));
        if (entry == null) {
            entry = hashRing.firstEntry();
        }
        return entry.getValue();
    }
}
```


如上所示，通过TreeMap的ceilingEntry() 方法，实现顺时针查找下一个的服务器节点的功能。


哈希计算方法比较常见，网上也有很多计算hash 值的函数。示例代码如下：


```java
public class HashUtils {

    /**
     * FNV1_32_HASH
     *
     * @param obj object
     * @return hashcode
     */
    public static int hashcode(Object obj) {
        final int p = 16777619;
        int hash = (int) 2166136261L;
        String str = obj.toString();
        for (int i = 0; i < str.length(); i++) {
            hash = (hash ^ str.charAt(i)) * p;
        }
        hash += hash << 13;
        hash ^= hash >> 7;
        hash += hash << 3;
        hash ^= hash >> 17;
        hash += hash << 5;
        if (hash < 0) {
            hash = Math.abs(hash);
        }
        // System.out.println("hash computer:" + hash);
        return hash;
    }
}
```


一致性哈希算法实现后，接下来添加一个测试类，验证此算法时候正常。示例代码如下：


```java
public class ConsistentHashTest {

    public static final int NODE_SIZE = 10;
    public static final int STRING_COUNT = 100 * 100;
    private static ConsistentHash consistentHash = new ConsistentHash();
    private static List<String> sList = new ArrayList<>();

    public static void main(String[] args) {
        // 增加节点
        for (int i = 0; i < NODE_SIZE; i++) {
            String ip = new StringBuilder("10.2.1.").append(i).toString();
            consistentHash.addNode(ip);
        }

        // 生成需要缓存的数据
        for (int i = 0; i < STRING_COUNT; i++) {
            sList.add(RandomStringUtils.randomAlphanumeric(10));
        }

        // 将数据放入到缓存中
        for (String s : sList) {
            consistentHash.put(s, s);
        }

        for (int i = 0; i < 10; i++) {
            int index = RandomUtils.nextInt(0, STRING_COUNT);
            String key = sList.get(index);
            String cache = (String) consistentHash.get(key);
            System.out.println("Random:" + index + ",key:" + key + ",consistentHash get value:" + cache + ",value is:" + key.equals(cache));
        }

        // 输出节点及数据分布情况
        for (Node node : consistentHash.nodeList) {
            System.out.println(node);
        }

        // 新增一个数据节点
        consistentHash.addNode("10.2.1.110");

        for (int i = 0; i < 10; i++) {
            int index = RandomUtils.nextInt(0, STRING_COUNT);
            String key = sList.get(index);
            String cache = (String) consistentHash.get(key);
            System.out.println("Random:" + index + ",key:" + key + ",consistentHash get value:" + cache + ",value is:" + key.equals(cache));
        }

        // 输出节点及数据分布情况
        for (Node node : consistentHash.nodeList) {
            System.out.println(node);
        }
    }
}
```


运行此测试，输出结果如下所示：


![images2f16695d002e4ea9ca9582bc82330e9b.webp](/images/31c1443215ecd8757b2654fbe48d013b.webp)


```java
public class ConsistentHashTest {
    public static final int NODE_SIZE = 10;
    public static final int STRING_COUNT = 100 * 100;
    private static ConsistentHash consistentHash = new ConsistentHash();
    private static List<String> sList = new ArrayList<>();

    public static void main(String[] args) {
        // 增加节点
        for (int i = 0; i < NODE_SIZE; i++) {
            String ip = new StringBuilder("10.2.1.").append(i).toString();
            consistentHash.addNode(ip);
        }

        // 生成需要缓存的数据
        for (int i = 0; i < STRING_COUNT; i++) {
            sList.add(RandomStringUtils.randomAlphanumeric(10));
        }

        // 将数据放入到缓存中
        for (String s : sList) {
            consistentHash.put(s, s);
        }

        for (int i = 0; i < 10; i++) {
            int index = RandomUtils.nextInt(0, STRING_COUNT);
            String key = sList.get(index);
            String cache = (String) consistentHash.get(key);
            System.out.println("Random:" + index + ",key:" + key + ",consistentHash get value:" + cache + ",value is:" + key.equals(cache));
        }

        // 输出节点及数据分布情况
        for (Node node : consistentHash.nodeList) {
            System.out.println(node);
        }

        // 新增一个数据节点
        consistentHash.addNode("10.2.1.110");

        for (int i = 0; i < 10; i++) {
            int index = RandomUtils.nextInt(0, STRING_COUNT);
            String key = sList.get(index);
            String cache = (String) consistentHash.get(key);
            System.out.println("Random:" + index + ",key:" + key + ",consistentHash get value:" + cache + ",value is:" + key.equals(cache));
        }

        // 输出节点及数据分布情况
        for (Node node : consistentHash.nodeList) {
            System.out.println(node);
        }
    }
}
```

