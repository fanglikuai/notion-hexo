---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4665OH5X4KM%2F20251124%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251124T210054Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJT%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCICSOnr8dchXqynrdLKafOZYh%2FQiHadANCuYTFaWR8iLKAiEA%2BTwAQb9V8A%2FvR93KdAlLr6LvJhjxi4PqVYf5iZWbuSYq%2FwMIXRAAGgw2Mzc0MjMxODM4MDUiDIJMMQu%2Bsqf6KiNpASrcA0uUNFBGgOVR28KyXKz5WHscKbp2%2B8kWzPXrRZtQuo%2BHPdsKDsgdPH1vnYQ7vJye8fNaMFh%2BcSegwOcr8Q7G%2FL7Kry3HN8LFfzBsS%2FKYPN0jxmV6iAxEt1sJfl%2FCG3hcxnWhqMpIY1ANmCmjXUUtvBmhfLVfZNyAIMJ6yGzjrZ%2FDOMCjQ5xuRRdsP7zMCybmlc19zwSUa7KWRCGlLFWgoMiaDJK3SZvlzW1Nn0Vl1qAvGr5cxtrFdGnP6TcntEVDxWhVBNqPx1zcCosfiwURm%2BtCJdWbOkfuSYneL4KHwTrFXLL3cjezS1OUEzljhnNP9AvdxMajbqZceVJc9%2BSjoCEm2JBcJ3wdgNV36QClWBmk7jFvAnMp6ZDQfubRYacunIKXxN3gAbv4J5cwSi0GuKJVAkGt3Kr%2F%2BS4xfpCApV46BO%2BblpMRGUhWE3%2FZxVCNHz5qVA5ITnja2c8iQL2%2BVUrBKZJ2pLUNfTBJQvPeHsdrEchMB%2BVsrxrWMA1JdjvsDN0Lzr%2BREIdlYILtPDlXMG6uSGRjhSHFZ4xxRxnG3OgeJDBUHB5ts5AwywJI2F7Hn4wWxrfu0HVbr8Ee4PBeA6osndj7JUrJu230CpstACdOqtSnQcvJRwygweuJMLH2kskGOqUBOOwN5y%2FNag%2FENjuuLwex%2BCeAQG2X0PhoIxT0vtU2Bu39ZzErjTOSgMvaD75pnSKl428H8xkBY%2FXo1m9sj%2F6HJOBUOVwo0UflXMxS8aUtNmcL%2FMlUBzS1YT6g58TrJ0fhlesZrq%2Fv2dd3HPsvK%2FCqYe%2FFPfhpDlGSA3VG8ftxMfMoHLoan%2FZgtTA3tcfcHLNXt2uYFVrAGNsq3LhBi%2FlOihriWr2K&X-Amz-Signature=483612b83d6be0993a43223549398ad315f6f4f976091ab58a146c29d6f23f8c&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

