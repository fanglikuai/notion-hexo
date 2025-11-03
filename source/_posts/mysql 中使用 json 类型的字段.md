---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666IERB3GO%2F20251103%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251103T140052Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJX%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIFpdIf7WDkPueUa5R9wkhtQbO6xZbUJYRtY1Xr9ORRrzAiEAl0lSuXKKiTrti4YOqOOLqDUOOVy9w1LU1lmAVRg3PiIq%2FwMIXhAAGgw2Mzc0MjMxODM4MDUiDBs4T52LSD6ExtL0xyrcA7mKA%2FNk%2F%2Bt4PNHQnx6Qye0m1%2FykXdfmE1Ouuo8AiJSghA%2F4AtGhrc62YWtSdK%2Fx8wGqjd%2B3%2F%2BJLARTIiXZfoaa9XPaYXYZvpxx4KFlIXigMviwLYZ1foVnsqqQsgHrGiELw0vaMjUWpZ39t5nBTRru0Tx28%2FFwpdD8eXVtHjbFkNnFr2c41v9R8Boq9K7CUy7bkGE75Epvpufb3ndP3Y7q57tjlgBpDSecXmlHs0UXxmsPRfPETfCKav4uVSv3sLTM3Wy4Qb2s6PDaaGBuMUpHdiDNk%2Bz%2B%2B8kwfQ9US8%2FvzfOeMsFyBPzadCd3iMVEtlpXOvAn6VNwEuOmKQ%2B4h%2FzzT82DoWhw5ILlFfyfMnW4Wr4CamGwiusFT3xwfGRLZxbedlJPakZWsbhwQwbFyD96ztQJqAXtKIaP%2FlNjx7ta292VpcJGhrfbpB3044HIGX1nPRiiCAtI4BXUYSRWDmn4TFZPx1P8aigxNSg77%2BChVeQKGXZBhHrM%2FtwOasj069W1bMgYhWfKpbeL9ldSfaqILH1UCFeId9vYXgDSdtOnnHWXROsIgVv0eAii8zEFEzfqT5zXlppcbR5H54vHLaxVatPx9vkRUVk0%2F3GyXttSXJm4AalupJXPzZoA9MOLDosgGOqUBEBKB3TPAed%2B1wXvYVNqw8lyoxkVBHHFii%2BOgQJHXmbsVTRn0iounXroUV510MngqplWfqs5jPd8%2F766Q5HVSFZp0Tsyjp3OL1%2BC0aNfUslzyMgJg%2BdFZ2GC8LUibsblnHelfx0jzi8%2FHUNvJoc%2BivzZedAiEiLVQzxci0%2FJ%2BBbvV4irByoLUFdiq5seSp09IxMQMcqJrCW0WUBsD3GNrvi%2BEqgfH&X-Amz-Signature=e7bd418db979250be5be6cd86a5be3518d3727e0da4cfe56ef2afe7a6c40a43e&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

