---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XJLSJWCY%2F20250916%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250916T230042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEB4aCXVzLXdlc3QtMiJIMEYCIQDc%2Fflo00taVOfLDleI8gwWqz%2Btf2%2Fcteg4GWLvaAUMuAIhAK8OB61aWzTvVxUEFsiMTnv3uPtg14eoY4L%2BCwD7b50PKogECJf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igw9NMgXchCsN1vsnA8q3AOWfaGAYhStN79GruO3DIYUQO6dRlzN3wCsSygwlJX7morTY%2BjWqoK%2FNEgm%2Fl4Tad95iPl9DgNF1ycYQfN02ye1ygjpp7SJWQF46f6ti1GVy8g64eqOH75Gwm%2BKujL9CLP9eOWO%2FZ2xBLNbNHhKM4CIP9fU%2FTRYIcCAH0jTNKaDtDnzuUZOGcSYaNnguWj5oxQvip%2FHgyJJKMwdoyHPk4Hwk46w%2FUQ8E0Da%2BICt%2B33C8x5pafmyxncJ36fLrAAuan0ILiiEJGUJmtFPffS2vZHJT3dTLVGMwrB2TjIeY2y%2FIdIKBgI5TBqvtUSwTqESUGBt%2FpKm8c7KBOv%2BFVFPQoQdA5mj3nMQ53%2BUitPbDWFKMB2%2B4s2U3zApjFNewqmOebSpqfGLEkwDSAps6Wt2bWoBeMZQ71%2B0d0tDdkgT3x0tA0p1mMipsbMg05%2B2QgzAU1P4vVHWuor8UOrdeN4SuVT9JnzK5l5NlQGxV9qrOX2bQB371iHQDTR8cYMVfHr0%2BIWBagG9pjBJJqPyP7RND9ZBJG56OY%2Ft%2B%2BwdQq9UL84FXuiIYAEGJPSoy2ltV%2FgN0FTFRC42Zvr%2Bxav8Nfjs9mWThkJNukJcp4SnaeONoJTGTJ%2BeugYvsidcGqlgRTD0vqfGBjqkAa93djRxCN9004MpC9Kcp8kZi6Ogjm8BEAgAIS4dK1bKu%2FALgXr4xugE2whKUypxBuEnkD3juVUeIVQQM1wHYYWvY3p593ekgkws7kLuxApLNKawMTQDUm0lpC2mcK8C8DA0Ls15r7ATVhgKhMeiMtEPO8ebZu%2BKZ2k7AmSEcXNXIsqE6kKuhpno7RV6cjH681yVCASvHMd9Az6DDo%2BZSXvRbrEi&X-Amz-Signature=21d4eee44d0507b791802df38f270640ac79ecf87fa7c20d62bdc51267f8e6eb&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

