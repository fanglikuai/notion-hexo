---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RZSTALEX%2F20251120%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251120T150050Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEC4aCXVzLXdlc3QtMiJIMEYCIQDabK0AbR0Pq52f5r50nMBrDrgZUts7KOGwPWL%2F2Wlj%2BQIhAKeewh9gmLFRrN21sJzoKA6m9LJR9m762%2BENKOe83KBBKogECPf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igz27TjCEWNNiAufud8q3ANPCAaAhfvFSSKR4A8Q7CPMx%2FF%2BcLZaI9sxBpjJPKki1cgB3znmGJrKAQROQo4iwS%2B%2B7b%2Fgt4%2BN6lCq%2FbjB4QpgGfdZj449Xb8BAxdvACP023beipo8SA3x3mWYdb5PcNoSknpub%2B0MDaDQ0XQ10nO9Nr9xu9kzq7MiMDLcr%2BAJH6rUH4aMCZQAZn%2B3deoRBLDTTG9x8M0QCwxJKZ59FXQFopBg9qemaIMjeuH9%2BB3qGt0ZI1zqBn2MFh2VGJ3iAis8Ku3XU10l8WOngbfKCBd604gAY7sIHrEU4LLQu0j6Do0Dfp6mQ0e7ys0Yh2mvr9HVIT1FuLthTRPC1cGY%2BMUI%2FW%2Fi6Hp%2B4vmtzVVDY4KsyuPV2VGyMwFavWX5jkJBVeT%2BCb2NFZtwUA0Nl3zClwv7YKDKqjkvyXR9yX9FG1Yg4rMbcI8KWpL5ADFwWIw%2FBZOGDBWH6zzbEe%2B2K5eX9xuZV%2BxuNiGvgrhKtcDsQC1xfa6xEyB505wxfsfWBjnQ6YrLHBZfdQ5SOxcxWkFMzgidCQkCZcJtFMg%2Byx9QmRPz9o0hJP7WbZ7nBhaRgokDtMYYiMVvsVQ8%2BbqzBm7v%2BUCZl2m6ZOWvKR%2Bl31uyYWfxOMZHIZBQ0dVDk7R3SDC6xPzIBjqkAS2ahj0KKDOeJhEGtPbv2y%2F7%2B7VhmT%2BXcTO8CQP9CFgcyjcmAsmca9fHqAroJxFqa7QCKwnHwA6ixjNG%2Ft4hf2YWf3x6%2FpGHBha1J40mfE5w41UiT%2F8Yu9KSGPyZBbR8Q9dBfdNY1LlsWzGAsQNNOm1qqKgp2tEtj85bRlLl6uW%2FpOGnWJECRcvZOHHFkltPgw0iKqcz9RpeMEE9SpSUe2N1scm5&X-Amz-Signature=24d567bb49b976d58e8a6231ed6e2303a16ecc9e9fca144118e44730da2954f8&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

