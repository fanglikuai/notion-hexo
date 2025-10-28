---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TG52J7ZL%2F20251028%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251028T000054Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIBNeaaatuQkQeHVCj7FGwMHxh808kJAm5okXwvs9Dwh3AiEA3MIPiyGik8PhxdEfYFrTvj%2Bm1SfkNetXGC6Nuat1QUMqiAQIsP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDDLg9YWpg3uoVp1cYCrcAxKAE1SZWLIxmU4SdBZIZAFXu%2BnGMKqQDrRBQcLqcGs5Rr0UL0ncSYC2IjWvTjPjJvPgM4dA0lF2lA%2Bk7z8r41TtkSrPe6d4QWjTsp80ShQG6JIiiTWpKOxG5KoRziZA7PWxteXg12otwOgEzNNVe5ndH0phLtvRdLGqmhXnEsCXFlrMfny3kIQcreg9RGL%2Bvu6eAXiJVMRtPObPjEtH2vkrm%2FZW3ZliMK7XmosDNKCyRTk1%2F6pFNijaIKFBlV8ZHwEHjl4Z2EbDTwMEjgoUEpRz6%2FV98IeYHefrmRoZd4eeHVGHrFp%2F4wLu1SEe28JxVftbOBoYfcdzU%2BLvoj8264nllWdMIPMJHfwUwZHECk7Kg6VuXidBFUWSD9nPMGDKpb4HWjUYN9mCaxvT0w2p0b%2BKCidX6ywKU2amoKzmBXBZYmWYcOWutd1sCmFkH%2FdUM03VAkYq6U4ljcEMJLI%2BaeO8y6uoZLam6BRGvlbLtjud4FxrgA30nzsFpmq6I8umzlO3hAZKsh0hGpqB5pHGcC8WAIQhU%2F7o6ZPbHMUPtesmt6o%2FOvwhM9lZHwvY0gDxwH4bX48BIbnQbYcBjbobhmlHdWHiRrck6BMnnayoe%2BMhE8A1i3pkrRdCrsXQMPD7%2F8cGOqUB9bKF937J9f12RKo2Ak08Rs63e0rrkF7VR0FlF45fr9Hl%2BiecIYNpRT45Qhz0ZSQnvt9nBSxzaU92k5kPtLPxzLASEy%2FkNq2eNimqXl2aDcqwdjpEnacLN9EyIgum%2FjiZMqrpVJXQyCyInfjozfZEu76AtY1Pa4Z81whnRqmZlUYPFxxzDI2rblzIO72%2B%2BfGUqkDklFd9FRzFMTPFPjNo3JIPn0zU&X-Amz-Signature=716e7d866ac53b25a30deaf8004373f3f9c448fefaffbb337f4e1edbcfd580f0&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

