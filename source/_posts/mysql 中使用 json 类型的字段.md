---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TRJUL3A7%2F20251121%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251121T060051Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjED0aCXVzLXdlc3QtMiJHMEUCIDx0q7OyBUXLk49VtYPix5m4JvFlXeUGzIpnhoObIlkyAiEA2i%2B4TWPqi%2FD4KZuLY3zzJSEHikZ7RVcRx4wO%2FdawbWsq%2FwMIBhAAGgw2Mzc0MjMxODM4MDUiDLCLJ0Evh%2Bzu%2FCLLhSrcA%2FQ%2FBvlDxsfBQlD92hjgthKXF%2F%2Bun3W%2FltiAclFrFqxPL%2BjFGskyjUiuJmJWEttERjz860xMc0aDJzNA8Cmt52sH%2BwYzOzfXUMebyv5zC3HBs3EBoIUJmXINbGDJsN%2BlVOrSrg9ExM7DaHhOk7Rf1pCMYhYW68ALh4ds0sdsyqoIAJnsfLeo%2FULtlo5r1qxTyn%2FNXi5Na8YUeqkefFt6wolRBEnWOMFt9q1ZWst9mGSWpJf%2FtJyQlL7YT9fb6c8QAhptzWaeVZXE3Bytr0DmDsl7d10LXqVDTHLVSXL4A5JigZnyQPZU24QfHvLkm9O7n0kl5giGfFhpwMSfSDCTN3rv5Em%2BLmdcw3tog18T6etgXa8rJuGmWphSDj2ZZo0SX9HxfmXA7DZYrhFNRZ0ptrvYIny1BeG8PHOLjyTetb6x1voakqpmutQhim9%2FB%2FeP45sqcYHAxiAmv5PXl8G%2FcR5mmb5ih0F0gCjfLizoPwZG5jxht40vybl9Yiau3NuuMqROlgD%2BhOBAXMSVATLViHLfHlk89hiLCb8lbMXBYzN4VjI%2Bk%2FfpycRhRXqzZkvQT%2FBCCOw7GyEvvqTRZmOv8G6NaZC1MSvYvmarbqqhW84qk3dIpC0uE%2FOG7W9zMKTU%2F8gGOqUBQ25nirfj2L%2F6R7WcYaEzKsyFYK67193P%2F%2FWCax2vAPMF1Xq%2B624vZWZDb5V3Sq0ZRHeKYbiqMQp0k1gDS6W4W55w9T2UQ9rzg6kMHxyLO2d9jAVXWfWZ6Xx6nyUKDltVPlbIienzK9o44EwU%2BYfF7mXwUZ%2Fb5ztfsfrtN1vlgSqesHhqqjA7VQQPKaPz9M%2F3gIpAdbrGds0QlNuJTRfSsP0SPigm&X-Amz-Signature=dd37cad71c6e8cc97ae27110c26494f1bf941ecaa318e1065e7985586b9ac116&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

