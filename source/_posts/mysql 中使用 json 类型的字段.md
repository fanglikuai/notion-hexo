---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UIV2EQK7%2F20251006%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251006T220040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEP7%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIFZ2vLUgFCDpncRsV13SuVnl1FkyEg4vFXYJ2NDBLWHRAiEAoAh9xKcpiMR9EzZdFhWxV20YYBQ0IzgbXSSmKx1rqOgqiAQIl%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDL%2BlmowX2Z7kSjMaTCrcA5xS3KhzX%2FT1mzMZ4OtjPQwrJg2w2V7HM4%2FegGZ4b3AysC2kiXvel7T2etgSdvBnRIZrKJpc%2Bdky%2B%2FersRtN7r6gYJDjmQ00MnPePDB6%2BYvRqoiKRHpuYdTx67HjCy8JLrDNHX%2B7MADF79yfkSTCKDC1AtFgofUrZEMH6Q%2BxavBIym0CAwMqIIIL26FZuWbROnrwTc%2BmFUzfXga4TaCxYAIrNismZwPY1t3vDE7H5dBM7PeZtVMEq0WHPv7dhKN85vUGSc1Z%2B7e%2BIJ1we%2BK0dIbMd4ICcsBsY2sdIgoUL%2BkcjhJc7Wl%2FZ9TWjrwvipaxZFP%2F3N3F%2Bof8tmG7AdMqYUxmVWfvFdAerdC%2BpzXjnlnVR85QFVLccrRkB%2B08H6ACecddZ%2F%2B1u6peEgZBgEt5fPNi9Uul6viKrMlFXWdd66tCJ8PDysskrRjLdsTgwjt4%2F5OHMVB4CkP%2FOqyod1oMEZbH1%2BsYJXFeHbMSa%2By1zlxlrEEHL%2FGIaAmpGodyuZP01HjM%2F%2FELBsqbd%2BQK4wiRH3doH1hrUL8wjs655Zc7WZBeKz79viZsYIXBBfufdP7UKyIaUQC29kmUBkfASmDTt22xlgDp%2FCastZrvTZ8EqVONROlU0HC50Vk4rXPrMPb0kMcGOqUBP2tc1QtTTrYGUSgNdlZ2guif1JxreTBlAgLCDqjnOzAOkBlVeyqVC5e8a%2B9AcOWxlJ4%2BykyXNLlLcvF63ZA2soNV16QNO2z%2BpfIZ6ouZnDSlLSn9Dtiks4kTEScXB70r2DTN7eq346vxw%2BhtTrCdp4KtgrvfUmfuQ4E1NWHlVG2nO06tEPpGsuMrGzy90bpfwkn5dglfGPyVilH57aNSXYbWlreQ&X-Amz-Signature=1fb511594a0144d3232502125190a39a0059d241e48d12b4cc18d81d61e34e7d&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

