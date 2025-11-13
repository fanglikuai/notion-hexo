---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QHDWAQDP%2F20251113%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251113T010039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHkaCXVzLXdlc3QtMiJGMEQCIEpgg0KmTVrHUn49wCGDi4ckxgZ0Qpmd1XHvLrJDzrmKAiBLqKdj%2F3gLwyY6aFdd%2BQxqNqrx3kkwmU8bMrhwW4fp5yr%2FAwhCEAAaDDYzNzQyMzE4MzgwNSIMNtt%2FaSBDwn45Zks9KtwDqvjLGzYYuiMtrbGsQRYMJtpkbmNHrpttM7woOX%2FcugVrp5FzGKMwQdn9KFnSY%2FNXSfIUZr25BH9JkQ3IT%2BJqZ4JlUUapNBc%2FNvldR9WU6cyYXBv4vzOipKRv8YoWfDL8cbj45s7dbDu2CgA9PDUBRZQ70TI%2BFJk7RI9%2FxaA0GNoXwk%2Fb6eHUu2NQbNnJ5JjYBPAYjmfUZYraNUmgT6t28OpG%2BIysZFzVObOz5qtY1xUVDmZKoSsVfVvLJ74G5Cha1L%2BCbjHf%2BdLtUL2xcr5zor27W1r%2BoZEZV%2BLS%2B5yE%2F2BNiquVNyMYBRr%2FfSCqfgp%2BQ5Gscfp7g3KeBf0Jif4UKlAhgB79u9cQngeymnf76JWc0t7XE0jSKEGI5%2FCUiaRAukYwlQYgln59PCHZalMGqZTDu47Gqo4FPQBvx4%2F3eHs4jH4HrFPahKhcTq8JyIM4zSl%2FHjUGlVFL0MEOjk6NEwsz0UdfbCn0eR4i201C7opcVor3yoa02P78E%2BjkHACjqM4SJcJGvMcVfZYljZQchgpxgItQBnWvnqlMZU3VE9gP%2BpKlUiKQYSB%2FFAG%2BLoqSKvgIi%2BN%2BS8bL7Jiw15NtiNyFEkXX2AIKMGUaoIJa4DWn3oKRoafUdiNNxm4w69nUyAY6pgEL%2B9R5m3XImowJIydFRoT%2BmRSrNX0XhhKY6k5n5bFSRWoBjHI%2BtUiwdvNNm4gFEOs%2BgUAJp3JR4GfA3zXi67OOk2%2Fm5UGAjreHyZJbE2wSHLu75CKyGQfyURXBPnJ00WTt7TbdpkE0mgAnqMsVeLgOxtyiIok4r%2FvungAIQa8GP%2FTFpKZadfl9xtxuInJ3L7XSEUReXpabcTNWrDRrQ8lEmzPkKF16&X-Amz-Signature=0b12acc57a8ccade97d85eef7114146256a3e381a7c2ae346bc03333d01a4db6&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

