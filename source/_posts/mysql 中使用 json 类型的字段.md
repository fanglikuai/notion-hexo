---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664IVW2PLT%2F20251006%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251006T080043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEO%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQD3qX62uVohAFoK5hj3kdjDnOxiIzlfAR8FceGhLvpFFQIhAJUunVFnMgUCNTF%2BNf1Ad88TDjXvKXnhlL1LNP7qRUJ2KogECIf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgwMlFFYuytUgNWHV54q3AOvZh2ljOQssSbrjl3LWCYiaN4JkBKKIGP%2FKWgAGXLQQgJbZpMTNIfzu2LopMNXTnVhafrSKsy1%2F%2FVXJyTPCl3REudMNL1Dvw%2BHPsPMMIzXZhwK19n975FP%2F7zh5I5dGWz7t5e25%2BiC1zuGvI3ZhDdGeYioW2zsSSVBAuxxKY0ZHd58%2FzTDxNIF8dh8weG9mouwMcG24XfVczaLbULWEiOkdv%2Bmu2J%2BYgCBJ3MvUnxhib0ev0%2BtVREYKrrb0atWmGziLb29YI7mEISJJe0KxxHv84WTMgbprWqVw8MFqOkrN7ReUBDT0OyIqKP67P%2FBLj3dvrBDMSIsqoYMdTx%2BFVRegAyBEyNN6j0gzshLHEaG%2B4oB%2Bpzk1QX7VxB4yE7pe%2Fk3dfKDZ1pDQd9OrA06TDeYH0UfymjKgjL%2BlDxwZpB8hZCwCYUTKtUoWkRE%2Btcz007Zs7k5A7AKg2dLnmGENWwgfBvXfJ0BuLuwYse3x5y4HNIr7fxBl7LxJtJrTfqUndtiFzv%2FpSY%2FFLcQcX7xVjiZuAefcPje3M7HWv4tqEyacqdgjQOttudtB0ovPLC7ZWXFnf6ale9Zmfa0Mcxy61P7KgBBmVdK7QXhx3KWBjVOyhQQIFH9GnyOI8w7YTDWvo3HBjqkAXeNc2d06V1WtC%2FhO2CTf8X9%2BsM85N%2B%2Bwj%2BtiC8BICT5%2FDc6DCGIe8U%2Fx6qxmkGkUkcmhFEnpbGHlJBGtud9QfbxPmHlTuz1IgPf16EGe2OKjTTo%2F80TDFO%2B%2Fb98qlzpaL%2BJJwPNm%2Fh2Py58GjNRhcg9aS3xwvMsJgpH0qHle2zUQHWC4ye26tRGLfZLHHtRz00LC2udGJMjJIEN3qbfROQXF4mI&X-Amz-Signature=027be0032b0ede481ce57b373cd93a324040274f26fc89b4594b59b37497d52b&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

