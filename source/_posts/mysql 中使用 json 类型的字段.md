---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YDO4BY3I%2F20251009%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251009T000057Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDAaCXVzLXdlc3QtMiJHMEUCIQCTLGaSLpfjJoeTlZKzMoN3Zk0wNn3siIiBo9DT0oT8sQIgebc5OcTAx0A9wZuUSVULWih%2Bjl3v8slq369q8DnU2n8qiAQIyP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDE%2F0%2Fmgg%2Fh%2F9Y5G3FircA61O60EOOIE72%2BdNH52eN%2F3WKuwdBBvTk%2FPe3hXVAf2Cplzb7y18112hfgruQ%2BrPAC0ahEPC6R%2F%2F5KVcXMIHeu%2FSpqWM7tfqOuN%2FW2NmACLKbO%2BX3LphvJsvh43JK073NN9zEFUr1tusosbzk0LJZB0Ig%2FAMYBrUpG2p2QzvALfv2h%2F3ZO7lL1CNVbbTNJpVQr3eyL2K2zhZH2qGjBYywzuSBuxPkG%2BgWO%2FMsgFhQagCEJl1RlZi%2BmLxTFtQZfGhw9AMbovlOIINcZ%2FxkrwJ0%2BxVrzxNMHW%2Bsh8bvlsnNOgG9DXHu1ZGnNnIeUN%2Bvf5I9UpHZ%2BMPQDE8%2F5rRzU%2Br1Ggc4c3Z%2BinnR3p5ovqoo%2FmTFqKXUb9Qtmeenq5BQKqQgQPfHLpsSIcNYwA4OKvugub%2BYfXSCZ3iXi30kuzsmrirSWXxkvCKFv6rHtd4NuaNpOKWPKoajKWqEPfwkEJIPcyCPqbR5ge6uqQkpEdv0I%2BfrdRJWI55Hzm3Xzuu%2FNm3jzNJGRVs%2FUXYQGlyYfz2n8YopWlbYik4liXytbmkmw7glt7VwztweZhkOeax7vhG8uwmAwyXScfOHtouBKkKOO7QQ%2FMHN%2BJ1QE6w8Ca6z7bnWJdylDweebyvWGEdMKLlm8cGOqUBhsJRQ9it71q%2B5eFYNe6s63sppyVWC8wLksCEsmvDiThs%2F6cbq%2B1IblmOYWLi3AeUaIhww7DqfL%2FJ%2FNcZiAiRCdwlVMNtaDwtcUGMgnLjdTNqGGYZyCts%2B%2B0Uw5ti18Gm6tMVUpQ55FjTJUTF%2FX5LRz2BQKcHYitAdmeMuIHrHLXCQqQziVd%2BIjU1bf3ddL9SCA4vp1brUiugz6N1AKdUl3kVxJk6&X-Amz-Signature=feeaf740012549b951988bc5166dbe68f8ba8e63bf574ff5427c6cb336d85eb8&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

