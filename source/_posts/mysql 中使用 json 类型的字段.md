---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VSU4GYTQ%2F20251029%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251029T180040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECIaCXVzLXdlc3QtMiJHMEUCIQDkIhgdK3gjYlbgG1RtQc%2FZrdrFV%2FUSrUMDV1Ifb%2FMyAwIgfMvrqOQXaTrU5ZTeB1Ge0s%2BTi14tg1KdGKc3n2Q3McYqiAQI2%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDFSsoWlFgLrzxMcqUyrcAxGwBbbmMABVt3ixj8VRyLNEWlqLOtadMDogKWOggw5FhYuxtVud9gTFITJzRcvCsZJz8h4PLraigF9WrDPLAGknX13d6z59bDqx1J4AC91kiTiek%2BsSFLnazSQmtTSmpm9b9GJsKdHy9gvsPYY%2Fc2DEkkDFnRkJeBTRSo%2F%2B3Dx8z0b3eP0wz%2F7%2BM3sfCH%2FPvTMUdqY%2Fa9L7nu%2BN%2FoooAlpMp7QzSuG5Qy9bH1nkphFS1KBhjznKW%2B7txiLG0uSWKkhh3xvSWXbiikUEBZK0bM9MJTfox%2BxFKq0OyFYnk5TIIhr0x2MBDbdPMYdAZv1R0ehM3COH4dMDLsNo6Vqje%2Fcl1EcLhPx8W2Pro3BrWJHzkHgNalb6nWO8hZhsNDD0UDRRs1O1L%2FXLARpOaKBzoU0mlD%2B%2FE6mp6MYTSmWvjEPfjSc8lhqgNvMXvYdezy8ZTUHsye%2F1N%2FtRCxLRQjnweFNcuyPZLwF%2F0EiYUXgRLf4ulX34dRYmO%2FCAXoY9fh1woe5%2Ff692%2Btsr5D4Zt4XyYkZFGD%2Fd6Gb3nSkHlCU%2BotLAcWjUAc9TTWXf8KuWGVdzWTdxBwlhyIH3DCSX1HUaBsL6sBN8CKWUXFGALVZa4pFaFjG0gNxs7%2BB2Z%2FaaMJ2cicgGOqUBRTIuUajATqoI7amN7XoqJ7P4qSL1sNqSACB1879VWsNHKXWf%2BnBpL9hXlTo4ROqPcM%2BhPRIUBlivSzH6opw33CsqPWF2ewqDs2Ql6QcwIU9bAJL8B1HpXRUDPJdmYP8CVXzWZWr2b8Q5SVYe6kZ0V1n7Ts6OKCS67eLl9Rs%2FcrbQFBA%2BBBZ4dieo2558cUGxEXsGhx5diUA2WpubPKL2Uh8Uky6x&X-Amz-Signature=1deb6739c54a9d5ad6b09bb421b1cd87ea4e050d4dabe435eae9053a99a5e4f5&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

