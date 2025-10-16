---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662FFICMOW%2F20251016%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251016T040046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQC6bSwJMr93rBSr32EMDyDQ%2F%2BMbVeiGlUQgAvLIF1I8zgIgPyWZf1rzBgP5ypyPzeGRwtWSdh2ooPoYpe0oPZ9bx7cqiAQIhP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDJh84FUlx5TyDbojUCrcA%2Fk8X4pMsXs7H9HJ5Wfdu7WIze6w4esOzHX8JIWUte9WZ%2F9Pv1FZF3CGNXo6KecyOwhhzdRTEp3w6K1xc8a56uYWJz8LBmgAkMBzTmE86uGDAdH7K6smku6CvDQxFVtlljJDzfwaUyFKVs9UFE4j%2F6N7NsE0ZaXssB0XMmhxDQEST2zzqtUgABJVH73ByerG92CzSe7WxYZOLltFcGLS7W9y7I4n2GUHVH4Sair59vAwo4BrzT0ahXtgjTCHu2Ur9fjZicv5ouvDRt4Sa517Un57DgO%2BEvmG%2BKW%2FJF8ZLzSXSUEyiatRczUoMLRtPx2je%2FKgvq2rXXvFyfPyPhOWo6kfZ1Dm8laSyt4s7NnaIt%2F1d8wkKHYJVn2KnjlAdkNW8kxrnsQSJ7NXsbVSBIN833J2k%2B%2BE0pOE6a0m0kU1amTwzkTAkRwYEd1nW54tgr7IRtd02xtXTuuhqjHVjGFrrrOfhkgADyfMtcPA63Le9WZI7Vx9%2BBEp4EWZd1sr3Y77zJrLUdIgYZujfPg%2Fhb1VyDB87x7W86PRAaDgeEPpEp%2BR7ExPP5rRd1Nom9TiMIkmXpUhMi2Fr3Vlmm74dCn58gPE9l952660DBLTZzXhe5UIOg%2FOCLfxTM7mKrG1MMHAwccGOqUBZ4C8XHLMAkKTC7dFoZiCgGTc9MfDzvb%2BTLsNIGlCotLZbBv%2BCAd01Dyr%2BKQEYtzNVdkhtbMcl9URsUFBUBGMrKg01MdAY09V8sFQf4O8%2F8q9NGhJhbnjOS%2FzoCOknoMF2M926Dejk4naDtqWWwYoBKoRE2VbfnXHH1jga%2BNy16sCXMYUSwi%2FZ3iwc7KiW97Q%2BVF%2F8gLfEkCV%2Fl%2FAz9MofYgLzqbX&X-Amz-Signature=2e307c378d9fc216a153f6fbe29a7d1d30d8bcb09d1f5367f805fd113cd45236&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

