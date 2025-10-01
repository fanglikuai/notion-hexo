---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZTFGAQBQ%2F20251001%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251001T090046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHgaCXVzLXdlc3QtMiJHMEUCID2xhC04ggTQn3XDMGCS0LYVj0o0vn2ERFZ5wEW3EeyUAiEAhE2pnG1518iJNJfbgYufta2gOvTJKVnXkxJi2P23lAwq%2FwMIERAAGgw2Mzc0MjMxODM4MDUiDHpK2239eMCCLUZAgSrcAwJLMkWCbdR9Fnabbh9Y8Bq5XCDQWpmY1BRzYRF%2BGmH%2FQRIsmpxEFR7tFoVmo0R9nwLBBpjxHNR1hEyX%2FLq5iOGuq%2BH3HN52%2BQpielqwkkFjFKGpQfNFG%2FfzuE4vli2wk%2F3IG0wPFMKjg%2B8DmNy91ZoKkd%2FZVkE%2BxByn8IgU%2Bv8KNAL78%2F6%2FImNRjVE4QMn3hJbfhVQK3pUYHV9AFCcgjLYzkP95lXuh2VdbyDjwSdMwwNmRRR4gBe87voRPdJsYp7X5%2Fv9vhEzzF1ym9c3O1kqf7FhYVNQS5oXhae%2BtHB2SbSKTSRgBPc7uAkCmxosNcB9TVN3E2sSyuTahDrrlnyHczhHbokmu43xK07e2%2F6eDZuQP3p%2FjUtf0Ea096jxWd0%2FQAuRqTuQkcd4lUjc%2BdqYI4g5RPyqcySa6HRoW9o4sbXXicdpGZcwfHLgN6IvXviS4zPDbWOpHJbEkpCPpJI9GrBXIix1RTyLczgw74aAf%2F5ohauvBTA6rwqxr2TGJk01DjMJxl6g46EhAzbhdyEeBRtyxMMH7ufv%2BNI%2Fuf1Lpr0SKyYrpwcTyu9zR2RYPgxkPrTa7snGXglnJad83pLJLXeMSy21sTwbK6b08WLzz9WSBzG%2BYGRZNm%2Bk3MOSy88YGOqUBrfrrG%2FA4rR1Nf2MP1TLjRpY2DHV1LOXdc7gVEPWjFcJku1px08M6fnS3zNAHVG3RBZc0e2iELV9zvxjbyRZUojrLTKglsscgwcMJ3Hqo%2BJR1Cjd7bTOrcRf%2FhDNEWDR7KMCsujnbpYa5ea8KL1YmR4PLA1TCtp473WMlRrcWC6QVDcbRYNtwtSbc9faF4nIWt2hU9RevFi03PD2fS9pyFhCh0E7J&X-Amz-Signature=8f9c3b26251bda92646d096336467c621ba19909297a9d1053e76871f70ea09c&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

