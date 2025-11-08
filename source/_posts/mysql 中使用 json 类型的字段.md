---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WWSFCUMV%2F20251108%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251108T020207Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAIaCXVzLXdlc3QtMiJHMEUCIQDDC%2B1tkAVdJ%2Fgf12Sf88A0%2FGFh6VOEP%2FTBYxV10TAbWgIgK5Ebf5AabhSYK9hoRXxLemUHIrK4WDtk7ovSwLdPoDsqiAQIy%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDGfaf9iWe6W6XrdUJCrcAzyg63kF%2Fv0yEN76MnwhIteRNfRxq11G2LXi0w5H7m9c%2BdIyjVZxl7hrIpV9h6dIioWSFpKeDr1nXJYjVcqdoHgujYzIYXdi9Hl1jzTOWFswF47kur1tcIXoiPSGwiMWOLH4Ti0fDIwppkZaQiEhQ6FcHh2e6cP%2FgFDgV66PIj4HgzQzeM%2FcZhgWKhIQcPxSX3cJCxj1bvHqEugeENLzxLtlfFEaeFtQewo%2BAhkgixu%2B7blX7TgbbEmiQJBQZonbL08tQ0Yt6V3GSe%2FBSGUic2zrPM76pr9ryaPHtM7BWCl2doK3zkSEvybBR3br6pvtMUDIHuQlglosvsAbuo%2B%2BAhzKxPGhlJTr7U34pdhk7DRQxtKiCFq3tOllsRwf%2BHZC8sTd9zyGBMQ7Vy5x8J4pR1Xjd%2B74KzBnlsCLqbBFbTpPXWdk679qfXjktq4w6wA%2FB%2FiJAfsWOj8%2F3ObjEFTCuD%2FmG83Bmyeeo2ENxTiM72gQ3T1VbWVIgjym9p2SYqvk7wC5MNHrozi232AIELmhUucuZ7hud0s2lEz2AZD6mnOgRskKkNpVuntQrftxyghgUmpYMCGKlLH2fLRhE%2BoQUF9igsm89CpnJVnkwLTbTBgXFrQflVDFSuBmHFn9MIi5usgGOqUBU8BwBFxKQ9c9P57kWh075JsqxySVFUEAtx5Y5f2M5l3FFkFNdw9QQzxmISgaO02%2FOwCNt88aIAppxiXzACx2psQ5RaSRLeJ8SQTeXYN5MYNqF88sjrJ6OaMtknCBE7LbUKHjrJElyiCQ0DNiX24ywTRJTgD9SMJGFw46cQWecjOShNvbVWBriAItvAH27Kfq%2BnomYjVaPgomkE6FDPz1%2Bm9z0rlL&X-Amz-Signature=9f866140a87bcac63f33f04787ebddd17f1cd4546743bfdd132de208643fe156&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

