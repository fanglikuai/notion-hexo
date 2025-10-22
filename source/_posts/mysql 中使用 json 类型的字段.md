---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZQL5FDMW%2F20251022%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251022T010046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGgaCXVzLXdlc3QtMiJIMEYCIQDemhQnmDe4v4AmeOywA2h22WWtP2wOTt3%2BWo1jGqsZyAIhALMu%2BtF9XgxgZJSrWPGIY%2Blh9Oh7fk7nJplWLYtAan%2BOKv8DCCEQABoMNjM3NDIzMTgzODA1IgxYIHxC5nl%2Bed2zyMUq3AN2Ln5Rk3xlNm0ZelhaTZhVazEmn82BUdRo6M0bgThQ1E6Yz8wKiWf%2BPSiTW165h95PqQsC1qFE%2BvmvtAHILyuEm%2FJ1cGDyRkoTKfwHsfattVN1BJvfP53PvaEYZ3M4l8Kjv8prfllV6RCpljGoFdkfbDGuVwEoFIhtpBnxUrp9dLgzMtkMerF0RTnVulLPnnmLP9rEZcBqjC7PQPSUtsu7FQn9T1yja%2BoPR6eghdyYwoKZ%2BqcCHKAS2LHdTIsLz%2FsJLHkeXQpAJrWduIql%2FsF%2BIPiV7V9k4s%2BqK%2BpWaqKfDgqTXUcMG67H3QQ%2BBJfh%2FnNJmRsybNOo7obpsy7FlvjRDGOTeM95wGI4xNtHoCdYZeu58JvSYpp62uRyxMW2H32VOF%2FPJLU0dTmPtjvFdj4BTcTnbCxRhUt43xzLZHTQLMoMSIpWxfCfdcSbglJIsO18guSiVk8ljxil9nwacquvUAKcQkdm3RxlbIcfms9biv2wPpQMHYlYO%2BeDGPaVkerGBVWoJF1xh3gfo9Q4mpvNDk7cL2PEz7P5SsnZ3ejUBvlj3yzagh%2BkR8u6YG%2BhnX1vaDRJS3X30ipl1TTtmXrWLie2CL%2Bo%2FnM64YtFC7p%2Bhf4Oxb191Z%2FdEwcbgTD2tuDHBjqkAX2yUIxo7wMmbELu%2B36L8i9ey5lgILj5HRwRBoQX0Pz7%2FbFOzFLMLInAIO0wGVLGIBDMsT%2F1qwlr%2BGXA98JJCGUTQUUtQi5do5UsWXlXva6sKpvOBSRyT10SOLOgKKhVPCPwBxe2VABt%2FFDRLv3WaRs8dqAFf48phZgi6YeBAEW67VDhwgVHZLrAx1ucf%2BDl9zItDhGbALhtoK8FWw7XFnUin9TF&X-Amz-Signature=9def01c20a52c2da24f8c8a031cc970793dd3f53db9cc961e6ff1c2c763e1ddc&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

