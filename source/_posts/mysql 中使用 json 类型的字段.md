---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WEDQNQVQ%2F20251107%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251107T230041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEP7%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIBsuK%2BfsFPEdMqvfab16OQFGAHd1Dw0qRPKJHzw8ZHkBAiEA6aR8UwtIQNuGh02v%2FoDwVct%2B8GNsNYEAkyz3gHRYElcqiAQIx%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDBgIg0%2FyaLX43n2vKyrcA2Qthmx8dBarDgV%2FLAEkgoCaAYRadqsYGvjyaSUF%2FtBG8Dcc58lx4rR8cef91OU5WzDeASIgSz0EdzDweXrF2NTmq9mgDV%2F7CRi%2FaoHLkm42iJL9QsB6Bebo%2FhQa0h6omkWuADLLI8bOYOz%2FE5j6dbcJ736R3H1cuf7wIudueAMsoabN8cUJwSj%2F5w9%2BtojqJ9qVd3oRm1mCwxKOxG4BH9CE3Snh46iTEfYlF1xC5ai77Q8rRGzipQN1eDlmyh0H31x8i6GwzPZ6mLOML1M9UMeBO3gjArIM2nZf4HGBCeNs0dN8PrFZ%2FKAEQtHNa6o%2F92SiW1X4%2Bq998FZ2jDWA8Ziemw%2BUN7RTLNtxfNkW7RsWhEAZB1I9ibd8xtW4VHDZHPf%2FmSW%2Bqw1VGtVUpvzjQeYDlKh6TnNR2SA1piO%2B4VZxhKZ%2F7%2F%2FTW7FqMBBZFIdrxIGgPI7s0izhMWtVph06osp3gz4jjNSufp4GPG6sZV%2Fasa6O4bezGyoKmfkB8F286%2BdCJFqiJbTUMdCwzF5LUgL8NtVmHcUpLvRd%2FIDPzfbAKmEOCQPSQTIxhVTJ4aN8Q1coZ9eLKHPNmIFfAOzEC3P96TAvhEA0PdYs42M3es98kqjr%2B8P7lKylxMUtMPHducgGOqUBqhQwCzwWEVA3OzVP69w%2BziTt66QOChbkVvgo290cFJQcBWlSbCZAj%2FXEgqYO9NcV1okSriK%2F6LkWrtB%2BsjUeP0zq%2BiVdwr0UyucAQPMBs%2FA6zm9hvpRYcMOp%2B85pcMZ0wteD5lDQUEG50p2EmKl5idpOUyoI62QxBY%2FU6vFoKyHJfKQfcN%2BR918Gq%2BkOhKIP1xkGuVFMd%2FFBs2IKN8XhN00iNNAR&X-Amz-Signature=6ec7b589444ea5d61f3687673d67b146007c12b03ecc2062af71076055516e64&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

