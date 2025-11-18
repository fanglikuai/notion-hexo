---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466Y57GIUZN%2F20251118%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251118T230046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAQaCXVzLXdlc3QtMiJHMEUCIQDj6YwHCADhEw2BidYSrvSsO6kdI2R3DQnZHz4laGfBGAIgLLX%2Fkgs3DRDXWcMt7A8azL6x63rAAS8S7YcvEfkrOtcqiAQIzf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDD0woJp81FQDAp7LeyrcA%2BxOrpXWaMb2rQSYpUQTcKQVx79%2BO9MWeOsu9yG%2FLto%2BQp9vNqw4qVl4HZm8ZJZiRKLNkz9%2FcmttbwK9GO3DZ7hytbYyI%2FOZvY2daHrsC5fCb820OKDrf7EFN%2BuwciuQ9vB99n1WCaHvT9UJUHqi7CNzOZyYIBTuAiRgq3hES7cVShpm3hHBOjid2VpWDhbvhDEanrUVPeA9S1QZ8ld0vUb%2BcnvqLDksuj5Vvc3%2FEji%2F1jM226ONtedwFAta6beTTFDzcRkC4t5B5jQGyCurkMkWbunz%2FqGvguVUWtCAC5Op4qsgYzuqD1Nh%2BqzWrg69eLj1WPYfkdYp3%2FHmpLgZ7v89abn9Ra6sWyiLSeGFwUjBxfnswTPbWauNxVdt%2FiFx%2BhrCvsucqm9MP3KxZBDWFotqDw%2FHdbRSXQKsFGFXnv8%2FszlaTN3bToeRhb5axLD5tGdpsbSRmEQKQRTG7%2FjiWX%2BuTMmC5E7w%2B5QToXGng8cKeWTDBtNcoDyJy9UePeTfRUHcpV%2FmXpI13Ox4bmAFbaHGJqWqQqOn0r1E%2BWPoTAgxgBR0Et2X1BXuXp%2B0uGzy2bwMlVZ7ODkeYro6K%2F3TXPAQzjTDIBddI%2FOKWNLuJQYPvznyWnfUpk31iohlMKyj88gGOqUBpBYlMKfPkLh0I8jsEGFWP%2BDYhB0n4uxJnohNv6u9Y8uypDPK2vMmbWTV%2BRk%2Bj5H0xsWpSg86mcJaMFGW4huzpzwKL2TEIbdBuVfyrLItMksHOJ2qizN3Eaec9arlbM14p7ezJYdPLl6QVVlQuHv0tupyqML2rOgbxXqN7ytdYze0ii9MahXs8U1VysPqyTsfFvaS6cJ4FNGbdnwOE4RKAmPqxHSp&X-Amz-Signature=1a64c2f29e9846f7f139288d17dd348016a0955267fdba41fb11eaf8f6baa9ea&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-16 13:55:00'
index_img: /images/fedfca57fabadaf76b871d791f9f19f0.jpg
banner_img: /images/fedfca57fabadaf76b871d791f9f19f0.jpg
---

5.7 之后支持了 json 格式


但是在实际应用中好像不怎样


# 配置&使用流程

> springboot+mybatisplus+mysql5.7

## 代码配置


java：


![imagescce2478e5401f24de6234fcc9a70b5b4.png](/images/476a1133e7aaa3e257f0f6fe9cb407b6.png)


mysql 中的表：


![imagese0bbc4d10d8ec7819433a5e83f307a52.png](/images/e2532123fe03eee4705d5db2c2ecc85d.png)


## 配置类型转换插件


```java
package org.example.studyboot.demos.web;

import com.alibaba.fastjson2.JSONObject;
import org.apache.ibatis.type.BaseTypeHandler;
import org.apache.ibatis.type.JdbcType;
import org.apache.ibatis.type.MappedJdbcTypes;
import org.apache.ibatis.type.MappedTypes;

import java.sql.CallableStatement;
import java.sql.PreparedStatement;
import java.sql.ResultSet;
import java.sql.SQLException;

@MappedTypes(JSONObject.class)
@MappedJdbcTypes(JdbcType.VARCHAR)
public class JsonHandler extends BaseTypeHandler<JSONObject> {

    /**
     * 设置非空参数
     *
     * @param ps
     * @param i
     * @param parameter
     * @param jdbcType
     * @throws SQLException
     */
    @Override
    public void setNonNullParameter(PreparedStatement ps, int i, JSONObject parameter, JdbcType jdbcType) throws SQLException {
        ps.setString(i, String.valueOf(parameter.toJSONString()));
    }

    /**
     * 根据列名，获取可以为空的结果
     *
     * @param rs
     * @param columnName
     * @return
     * @throws SQLException
     */
    @Override
    public JSONObject getNullableResult(ResultSet rs, String columnName) throws SQLException {
        String sqlJson = rs.getString(columnName);
        if (null != sqlJson) {
            return JSONObject.parseObject(sqlJson);
        }
        return null;
    }

    /**
     * 根据列索引，获取可以为空的结果
     *
     * @param rs
     * @param columnIndex
     * @return
     * @throws SQLException
     */
    @Override
    public JSONObject getNullableResult(ResultSet rs, int columnIndex) throws SQLException {
        String sqlJson = rs.getString(columnIndex);
        if (null != sqlJson) {
            return JSONObject.parseObject(sqlJson);
        }
        return null;
    }

    /**
     * @param cs
     * @param columnIndex
     * @return
     * @throws SQLException
     */
    @Override
    public JSONObject getNullableResult(CallableStatement cs, int columnIndex) throws SQLException {
        String sqlJson = cs.getNString(columnIndex);
        if (null != sqlJson) {
            return JSONObject.parseObject(sqlJson);
        }
        return null;
    }
}
```


在yaml 中配置：


![images944ad29a7fcf96a0c51a577d6bc43317.png](/images/4d25cc1863ee3e3fa6ae7e6d4c2a6cf7.png)


xml中配置：


![imagesd6de49b9a7b17849e0d393569b93bca5.png](/images/1067c14ea63fdd81764edc7b0b6e9828.png)


# 对比MongoDb


假设有以下数据


```json
{
  "name": "John",
  "age": 25,
  "address": {
    "street": "123 Main St",
    "city": "New York"
  }
}
```


使用嵌套查询即可


```bash
db.persons.find({"address.city": "New York"})
```


可以看到，直接被秒杀了

