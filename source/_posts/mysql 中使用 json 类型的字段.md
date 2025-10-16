---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664LVP3YP5%2F20251016%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251016T110048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIDBCJm2d2L0%2FZm5%2BqOMevPMb%2BkHVps4l7z5WeLF17qr4AiEAtbJ9TCyVxh2SNYkCods2gt7Ccy7TNkHXiv3tBqqGr3kqiAQIiv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDJohYy7YV9lgedFXryrcA05sNCO4B7lI3t%2FA%2BeCPhJMNCnKYUy%2FGHcdr1LDYXJIL2fZyuz5sxVIHugp1eqxQtX5skGrsVPleg58dEKXRlwTOpZfLZKBupij%2FgcTpiaBOsji807nnPcbPz3zwkGuQkBxH7cAAdfdDEHfnaR7BDNAMTgq3nnoWD73ED6W3J88HkKCt8LgUh33LcEUr6KHOReWaP7GMK09ZQ3SykqJkbMzcZ7lzIxlY7zeu0mZooQMSMV0oCj8NBRwEdqfWVsO4Zdm5I86hK3A8XdqPAmhWkCDLVt3%2FCBxJKQvktRY5A%2F%2BVRuzLtQmzBNJ1i2QZN55Mli%2FNBOD%2Fs%2FcC9WrYdXzPH1a5rakV7S9mrd3Aq%2FzFxc055S1zXqRdX%2B8N%2FLGUsuIIpG36qXVLgbuuEhaUU2QUXoGkKo3YVZgjszWSja99vlOUOxdav3eeKSYYL2bWqVLYB2TNxd%2FoftRFTfiyvnlIO2G0vow8SYWy2vbMW%2BODNYGXUZaz0Qn7Oz%2FOIyLoWomMhEBuYJSQkCU%2FXs5VvWxyqrCmBp8xRpADfFQBIWpyHjhgJIoz8jQy%2B%2FSSocRUpoHnzmPD9zAbt7j%2BRb7tX0npNbuYzbq0Qyv%2BgUoxUCs2gozDuhkY22Edi2ADJczqMKXawscGOqUBdGsi%2FWvFKWd3N0yYDuBvRpDLr4Il5MzjINBqzGvI%2FlPOdKwSu0nLOpxC3bN11n5oEY7MmLO3N%2FsYX%2BM7%2BdvWNHqjvpdIpCTX3n9tcb%2B1r2FeVETolG%2FzF73uuwihjwl6lagTawsWOEqA9gp%2FugEXInPo5goZlce%2BYH1U7CfqeksX%2FApNOsgl0sttErtMD1T62aQM2nUaI9atva5V%2B9IuOqndKyMi&X-Amz-Signature=df848db78949bf4d7224e53d905d826040d2bfb5ea217c7e2717685be41917b1&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

