---
categories: 整理输出
tags:
  - 面试
sticky: ''
description: ''
permalink: ''
title: java面试之场景题
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/d4a02792-ec3a-400f-8fb0-a22ce204bc3c/anime-Neon-Genesis-Evangelion-2299006-wallhere.com.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662UEE7AHP%2F20250915%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250915T142021Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEP7%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIAUE0XQL%2BZOrHEcg2IxTWDz41IlSOOeH6TIj%2BJaj%2FGyRAiEA4lfmJp2OUatNeduasTbsRwQNFBA4%2FBmTmQE6m3OkWQQq%2FwMIdxAAGgw2Mzc0MjMxODM4MDUiDGz%2F4l3OFeFeu70zJyrcAwtrlgikzcImovJfPeCYN31MvTpValdgTcjCydPWTQOlvPnTDR5CmAzm9LoYA2XpwI0UDsnvsJiBBxgtB%2FaXl2j5wam9yR6%2BCKPnFIrks%2Fs47dF8eMUrEltFkFJFkraNQTkkeME2Ddbd6T6NQQ94N8vFPV4tmnwGPxckBfe4mIBq27dyYUrOxkSgIXkcegOJh2NY3h8GYdqRISXQSrYtjJz2q%2FS%2BLcYFyj3flw0iGnM5%2FUHsIelnSfBJ93sOPAeeshddheT%2B2BKmsP%2F8r0fY%2FdV36rm2EoWDkchSKGata4oKQSyhGKZvbbWBS9abSJ5sYHsHtEBw5Y2OovDLmnwHW2M3MAnIGLvr7ONvf%2BHR5tYH0Sr5BLyItT7pe1NJT6W9qLUD7Rsaeq7TkcV2Botea5qJv7aScnASw1%2FzmFu2KNCZXjmlT7%2FH6EJHFMw3QAKrIJmVDE9VzqooSRUXiZtRvX4qeg4S5Q1Cl7hfTdqKm%2Fqm%2FmktXuvvcjZIsgSC92FIJmJchEfR4eZlXJ%2FFAnH2rE6MHK4utPttm0mSA8gqsYiV3Vt40d7cd064wG8qGWG0jcjxeLuaYuKoUrkAxMTqlgf1dtWH%2BiEl%2BzbOVZOnkPmWTSmW4v6tsiGPGADFMO61oMYGOqUBkfeNxqvZ0p2b%2F9HVs9elMMe0IB%2FiNcAtu4R%2FC00%2BomoS12HIKpA7cUu8RogZf8r8bO%2FShUB1kDJIUlNOURbo8onzBBru7UC5olU1x04WaLiTsiuJBwTMRNXEEuww44Pl9FBUz4ssxmHQp1C26NbYZcmATB%2Bs6BjuNpQy%2FxQhEiuGsBHA%2Bf8RbvOjHTEBs6s3BNXmBjC2J%2FJvc1zpVJ5HR4LnnOrW&X-Amz-Signature=4ed4cf2e8f810b6ea417e65dad6bb777ba8a5e553b872a8558a254ee3c0ca592&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:57:00'
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
package com.fang.other;import com.google.common.collect.Lists;import java.util.ArrayList;import java.util.List;import java.util.concurrent.*;/** * @author fang
 * @version 1.0 * @date 2025/3/26 14:41:15 */public class Test {    public static void main(String[] args) {        ArrayList<String> list = new ArrayList<>();        for (int i = 0; i < 100; i++) {            list.add(String.valueOf(i));        }//        Lists.partition(list, 3);//        计算方法耗时        long start = System.currentTimeMillis();        batchSend(list);        long end = System.currentTimeMillis();        System.out.println("方法耗时：" + (end - start));    }//    线程池    private static final ThreadPoolExecutor executor = new ThreadPoolExecutor(10, 10, 10, TimeUnit.SECONDS, new ArrayBlockingQueue<>(10));    /**     * 一次批量发送消息最大数量为100条     * 批量发送接口一次耗时不超过50ms。     *     * @param messages     * @return     */    public static boolean batchSend(List<String> messages) {        List<List<String>> partition = Lists.partition(messages, 20);        List<CompletableFuture> futures = new ArrayList<>();        for (List<String> one : partition) {            CompletableFuture<Boolean> future = CompletableFuture.supplyAsync(() -> {                for (String res : one) {                    if (!send(res)) return false;                }                return true;            },executor);            futures.add(future);        }        CompletableFuture<Void> allOf = CompletableFuture.allOf(futures.toArray(new CompletableFuture[0]));        try {            allOf.get();        } catch (InterruptedException e) {            throw new RuntimeException(e);        } catch (ExecutionException e) {            throw new RuntimeException(e);        }        return true;    }    public static boolean send(String message) {        try {            Thread.sleep(2);            return true;        } catch (InterruptedException e) {            throw new RuntimeException(e);        }    }}
```


# 一致性hash


首先定义一个节点类，实现数据节点的功能，具体代码如下：


```java
public class ConsistentHash {    private final TreeMap<Integer, Node> hashRing = new TreeMap<>();    public List<Node> nodeList = new ArrayList<>();    /**     * 增加节点     * 每增加一个节点，就会在闭环上增加给定虚拟节点     * 例如虚拟节点数是2，则每调用此方法一次，增加两个虚拟节点，这两个节点指向同一Node
     * @param ip     */    public void addNode(String ip) {        Objects.requireNonNull(ip);        Node node = new Node(ip);        nodeList.add(node);        for (Integer virtualNodeHash : node.getVirtualNodeHashes()) {            hashRing.put(virtualNodeHash, node);            System.out.println("虚拟节点[" + node + "] hash:" + virtualNodeHash + "，被添加");        }    }    /**     * 移除节点     * @param node     */    public void removeNode(Node node){        nodeList.remove(node);    }    /**     * 获取缓存数据     * 先找到对应的虚拟节点，然后映射到物理节点     * @param key     * @return     */    public Object get(Object key) {        Node node = findMatchNode(key);        System.out.println("获取到节点:" + node.getIp());        return node.getCacheItem(key);    }    /**     * 添加缓存     * 先找到hash环上的节点，然后在对应的节点上添加数据缓存     * @param key     * @param value     */    public void put(Object key, Object value) {        Node node = findMatchNode(key);        node.addCacheItem(key, value);    }    /**     * 删除缓存数据     */    public void evict(Object key) {        findMatchNode(key).removeCacheItem(key);    }    /**     *  获得一个最近的顺时针节点     * @param key 为给定键取Hash，取得顺时针方向上最近的一个虚拟节点对应的实际节点     *      * @return 节点对象     * @return     */    private Node findMatchNode(Object key) {        Map.Entry<Integer, Node> entry = hashRing.ceilingEntry(HashUtils.hashcode(key));        if (entry == null) {            entry = hashRing.firstEntry();        }        return entry.getValue();    }}
```


如上所示，通过TreeMap的ceilingEntry() 方法，实现顺时针查找下一个的服务器节点的功能。


哈希计算方法比较常见，网上也有很多计算hash 值的函数。示例代码如下：


```java
public class HashUtils {    /**     * FNV1_32_HASH
     *     * @param obj     *         object
     * @return hashcode
     */    public static int hashcode(Object obj) {        final int p = 16777619;        int hash = (int) 2166136261L;        String str = obj.toString();        for (int i = 0; i < str.length(); i++)        hash = (hash ^ str.charAt(i)) * p;        hash += hash << 13;        hash ^= hash >> 7;        hash += hash << 3;        hash ^= hash >> 17;        hash += hash << 5;        if (hash < 0)            hash = Math.abs(hash);        //System.out.println("hash computer:" + hash);        return hash;    }}
```


一致性哈希算法实现后，接下来添加一个测试类，验证此算法时候正常。示例代码如下：


```java
public class ConsistentHashTest {    public static final int NODE_SIZE = 10;    public static final int STRING_COUNT = 100 * 100;    private static ConsistentHash consistentHash = new ConsistentHash();    private static List<String> sList = new ArrayList<>();    public static void main(String[] args) {        // 增加节点        for (int i = 0; i < NODE_SIZE; i++) {            String ip = new StringBuilder("10.2.1.").append(i)            .toString();            consistentHash.addNode(ip);        }        // 生成需要缓存的数据;        for (int i = 0; i < STRING_COUNT; i++) {            sList.add(RandomStringUtils.randomAlphanumeric(10));        }        // 将数据放入到缓存中。        for (String s : sList) {            consistentHash.put(s, s);        }        for(int i = 0 ; i < 10 ; i ++) {            int index = RandomUtils.nextInt(0, STRING_COUNT);            String key = sList.get(index);            String cache = (String) consistentHash.get(key);            System.out.println("Random:"+index+",key:" + key + ",consistentHash get value:" + cache +",value is:" + key.equals(cache));        }        // 输出节点及数据分布情况        for (Node node : consistentHash.nodeList){            System.out.println(node);        }        // 新增一个数据节点        consistentHash.addNode("10.2.1.110");        for(int i = 0 ; i < 10 ; i ++) {            int index = RandomUtils.nextInt(0, STRING_COUNT);            String key = sList.get(index);            String cache = (String) consistentHash.get(key);            System.out.println("Random:"+index+",key:" + key + ",consistentHash get value:" + cache +",value is:" + key.equals(cache));        }        // 输出节点及数据分布情况        for (Node node : consistentHash.nodeList){            System.out.println(node);        }    }}
```


运行此测试，输出结果如下所示：


![images2f16695d002e4ea9ca9582bc82330e9b.webp](/images/31c1443215ecd8757b2654fbe48d013b.webp)

