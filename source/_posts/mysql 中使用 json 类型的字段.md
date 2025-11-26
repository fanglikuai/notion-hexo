---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SEVMUU7Z%2F20251126%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251126T030052Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCpgMfU0SC7zmhsVee7hgAVyj3kJHJxTciErXnGrx7yLgIgHOug1NYeWazbtzSkp7qchamAkU2Pe1wZTCfwUloogZYq%2FwMIexAAGgw2Mzc0MjMxODM4MDUiDMTrhHGkBnGbRitkCyrcA%2FX%2F942yHV%2Bp5j2YAkObrAr1rbr4s6aZmdvwWQkL%2FzTMIMEjpRpp6tV1oQzeWRlyd9TmxsUDaCqsX0O71g3HqqyJ2SRX2JcBo97F%2FqG0NzcVSeCSkntfrhlzPY8EFGpDEWpMAOkK98hJIiPKQ9ZshpdBc5UVRCwoJDALsRBhZRmQst4zjsOzJ364T%2BcBNtwFrsx8nfwQvr9GTmrIM2rxgbb2OURpso%2Bq5LkiSWIb%2FP0Is%2BZyzgdmBktJPusnwPyYxceEJVSX6DbKxuvj8KDbPzyzaoO1W61aNd5d7WQiBlya666Y1G9vCN9aSX7gV96vdPEG2vhYZ6LtOsmhCBb4hMur1vDgG22V1fHb5m42zcadXa01lwG7xjzg6Igdu2Ql%2FbHCEaL8Ja0Nrb6yjqkVZ0OWmNHOikMrvpXn7agO%2B6rlkB78IknwzzKeCyNA%2Ble3D%2BvkWi9l2BzYqaWY6%2Fd9XaF9Eg8rygFDhH8ldPdC2p3Ruzh6rj7f7dQrNzSyHGsreiEbAkbxlaVK6vlzNv5ktMBWQ9%2Fgmef5%2FqVS7X%2B65v98Hs1xOYjmUaq%2B4Ntba0uqMNolpvDbseeou1WmjVWxBt2DY4FQx%2FI%2FsUq%2BQtF0BqJ6xQqyxVZlojJYI%2FkQMOjImckGOqUBuYQq%2FGdFCUgztiYU%2FD363qivioLG4PAv7kAbGj3tIB5Xaph7ODNtuucsbeNNF4N0JzVLE7hz32YHdAuGxIXcOvBqaj%2BEDFYqnHQkC%2BCVAY6tJ%2FyC72bCW1crvAozhKHZO5pnzNsF36dZ3hIExdyKuOHtqpcDXKRkUUYooIjre0Co%2FqryfiCVs18%2BFsVVT7hrq50FcR48Q7UUG0BHDqeuYXKEY%2FG3&X-Amz-Signature=5ff6405b87b58f1f5100a22d29215c72240d555f67fcd76f69382473a7f03e84&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

