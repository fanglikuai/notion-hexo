---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466THJZCJO5%2F20251024%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251024T170055Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKn%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQChdtgX4iZHi2PQHRAzLCfOPjEfWjTBSB4rZ0E7pyVS1QIgFs1Y4DWso%2FXZ5qGWRsOOnHW8nyDars7822D3%2B19MPtUq%2FwMIYRAAGgw2Mzc0MjMxODM4MDUiDJNSmDbWjvgAGJKTWyrcAy6RljDyOSd7XTL0TMBpsVQrPJ0uoQVJa4LjjNe36BPelNVuKk%2BgjhYmvLm3k4rqkENU%2Bqri5j1SgLi%2F6kdp9W2Xi6UgHQn%2F0cmCPDPeJ1QLNvQ3JgcurXJ9lW8lZf4PfOYL5fwikAnlKwh7ksBhoH7u2PX1yQqMbiVONYltVxJoG4sbtwcvmzjDLY7OVjCpr5Lq8wL9zCSzyksF%2BnkVQ6h6uPY745zlvpDk5NGuUM5wmy%2BmEEelNnxppENlRrGXlcQVbST%2BkfM1o%2F8Fbtk0yahGtYaDdO6uFca%2FhDeucrZo36hnzKCTBBtNMSe8d9L2U6zIXJVOGsVMaianBgeQ7Fr63YorrFZLHv8BJwYT4VWR6NnzGCxZI6HvRLS4Ptnm5nObHK0F2wVncF%2BeaUf2efs4zsVZPMtnW3t8JGiFufPq85IpK4DL9bOSSXiij2QR%2FyZMgW9ODWDqKJhnzvw5hFxFUIhUQpGj4b2EWOcemJwgvEyCQ338C%2FkCQ5sajfVlVMjrLhHa%2F8%2B1cVeGZrW7%2BSXtokFvkQ01w%2Fp1greysCbg0J6%2BFVlXO5%2BOKaJRHYFnKiyIzXRgE9Mf3aXjJ0IZCpdL9v79rtTmJjcWZ3KJoT0J%2FZ8rtgXgi5OSGjJvMODN7scGOqUBMj0ObujOWuTDTphYbUpz9Xbju1ByKi0wThG2PT06guArWgBB0bSFPD5EG0QPxgCvqEQ5pVoWnu4htJmmtaH%2FRmehJmCvCYxfZ7od2VBIRtTgLYdxUonFGvreR0PxqjcQfqbS7rWeoLTf8jFOJN8nmL0WEWDmD04hb7tcWp3XzEqdlndBZSmlnBBMRY%2Bhts6tSL14LnxiVVw8mVmpAFRYFK6jh4L4&X-Amz-Signature=f94d613dccbd7bb6540d976512d084767fa14f2c2eb0f1282ee0521472e45fbe&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

