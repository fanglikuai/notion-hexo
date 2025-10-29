---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XXEHYUPL%2F20251029%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251029T080041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBgaCXVzLXdlc3QtMiJHMEUCIFu4V7oQE9QqVYWMGVx2H2%2FYll%2Fjt4ZUFOGAqNiL1SZJAiEA4UdSVcvQXHd%2FazVTIjAOHfrCPFg91Hn8spHUcHt1BDUqiAQI0f%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDCMAO9CDTuiHIDhu2CrcA0CVoPqz2KbbtRB09kpNrwxw2r0rx7TW823tocDqHiGxdcYcoPkdER9qJoF0mh4XsHuCPYzb1KJXNVv%2BlDdty57Rt06pikS8IeqvxGCLF7uKIZondcX1alGVGTfT%2B%2B4B2SGu%2BQtmmVlwGPV8MAErK1QD2439Ef5%2FpIZH2i5hdU8qK4q6nFmSrqVCY54efszGxTKAKD8IIhzobF4qkj4lhSru9IMn5TEqF6a9zev9neHt3gZBXSuEhx2CUIwxvD0duA%2Frm30m9cn0D4LREIh3zvXELbbi%2FTlJbhVsmfNosAVHcfySES3YDQ6UiRmmz9glO7RHgU1IwwNuRh%2BRnbn5ul2rDFbuNrKgt2MCJbUxoBRqqcAYnKBs1nzE35YK19ucjSeZz3q1PjH3sjHXot5tODmiadUibPpiHry1XlGKbw274uVEa4xWizMzuRTFDB5jsT3W80aDvIEU1aogsisDMP2pbv6mb8F6J5BjejGIDW90CTgKUuVc5%2BQsxK6ai9BNsVVmqU8qdW7lxv5uOwfr5VnCuRiNJSbNH%2B1KXAvBqFvsfW7U0plYJzllgLd9MGIz%2FWb87%2FsMAUaG6nZ4YFKiGsUZT8cXBDRWV%2FMK%2BBrV17G4DccBuDLd2802jkbDMKGJh8gGOqUBPjiG3KjpSa5o0vWuUbO4qPxH1197BLIsCotDz8l8O7B%2FnCwp%2BUmmnE%2BpjpT1vqnRoE0sA4WH0IDLrrHycdYZqJSzQ8Rck8qL4pzJJkDgsrRx1EMpaaFGghBClHzfpYlqxeH3gHUcIbaKLDHJ4dKuY6yaz0ra9S8eOI0o74CLYDTHGc7yCOGjAFC9B6HPN03%2FWyHlN8q3nuN7kc3JRqe0xL9d6rQX&X-Amz-Signature=539b0dd0518b570bdd62dd6d6fcd97b76ec48306bbd4346ef663f9f09bb5b0ca&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

