---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664WQVVCHI%2F20251120%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251120T070039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECcaCXVzLXdlc3QtMiJHMEUCIDUul6by0v3AQKSzJfzU83HVWx%2FiYPfN4MXorDtTTIAAAiEA8dOXgCFyjpn0Sjxr3Pdd6A%2FHsp4c1yIVE9y9kDMlhZAqiAQI8P%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDJ3%2BnzUUsdPeiKtttCrcA7fufBYN9NR6nuuqwIeWkP%2FfJp%2FRlWulqcpq8F5fnmycvJ%2BnHd3UbHW5%2F%2F8OjrazAkBeSzzuc4s%2By0fijhqOrPqBVoNOcmW%2BxhVOBwKiUSZC8J1xhhuqBYwxfA4g%2BPAnnyi2h5tz35hBzZNhYlcgaDQpjGxGOpDOdnMFJOGaiL78265uCfmwSSZdIzEnoAWVFP6ZNRiQCtfMJQjWPl1g1DOxyeDH4XGVBjXiB8Z05XDkLHclJcFia4liydAc8W18T1nxZcrDw%2B3O5lHoPI38YirvJqpxLZijI4gadEthGe2qBcfKi%2BwoEHgxV9xtF%2F3qe3OpWPgCfTb2Hx0FPR08ijPRV%2BQj8whdvi7mJGjoU2nBI8bkNhcdMYHmpz1nnuFG2hChoOcHU208%2BI1j0eiL%2FtEM4IeOVyu%2F5qg%2FlWAQ3zNGuFDB3RdwzKXr2t9T8gfggXR%2FJq%2FxKepBlRr0WXxxolkGCl4exSQjeK9o%2BaCa%2BrGB6G5%2FbKury5dTeLIfsSM3d5dCSyvd8eM9foh4q1NRJp%2BxgDVlJ5Cddsp2SUxIyP8biGtGG0oS1%2BTsA1G%2BgQQq2yUP6Ea%2B0OmJVRs11%2FIRStcNzMnh0Vx%2FF3vTqaNX1xybZFM3P1nrCrYDVIXYMJ32%2BsgGOqUBuzp%2BtnVLWX1Vy4skyljww2L7KXWivYyrRGl%2F1xY8sWm%2FYaBRtBozxTEWlCsvSB6dmv%2Bd87ws3TuZhERnSDZetp2xHBP70VvA13zK0z5Oj2e9ORilKOccnDzt9XpRNzD8N5mqN1dX%2FYLQGv7JDuJz%2FkbNwQaHYmGRprqicjpoxmGxCzuzMQ1E4jmiRLhKeTSarm8TZRLDmU3ANQp1MaxVD%2FYGFevs&X-Amz-Signature=c8f87095a74f40c7b357ef307bcffa46f0cfb8c430938bdc6c5061d931ccc89b&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

