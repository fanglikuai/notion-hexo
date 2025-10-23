---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QP5N6TKB%2F20251023%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251023T010049Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCqRlBmgg7c%2BhN%2BJJ0Qmn%2BXMQA9mehDi3UQa4R%2Bvkg8wQIgUyAr5skWULV%2BrqiKXCUNBDofCRGB1BgysEgsg3UuL48q%2FwMIOhAAGgw2Mzc0MjMxODM4MDUiDGqv7vN37wnOFUbfnSrcA7g%2BSlJcILr1Mx9mTXPbrJ0CAttaO7yN%2FX%2BC407CAh12qTnG1LlLNYlVPEZsY0u8wSJdYboYa657CpdLO6QZJUiu3bpjfBlnbUSQuSdx5SYNZuwPaK%2FRyFKoIYFL82voE%2Bt4Wn0kLPUbjOGbXNkt%2FrHCLYBHbOM5HMGAfT%2B0uhzab6p751hzs6LlTGJHUeayKXFrEZldZdtCd6wvl3UPTQnlv01KwZ8lt7DVKAz9f9N46mFMzF0LFijxjK30WYsjndUEfuhX4EiiTHHalAWlRio2HfJL%2BAOzYwweq9UgBk5gh4X004W%2BrtcKgiRztj9%2FvTMMtNJp6m3MSCvWcCqyjREvBpPdvf17rhxs%2Bc793Hwf5ejTgMWR1gWLrztDQC%2Byva5ZryVFsnA7mg%2BH2hOgAy26kh0Lzl8R%2F%2BxeuXuADR1JeYm3GZatqezeGaSWhYNh3s88TQJpqKPzCiGpfAUjcPUUQfMAyR0B1%2FqXmuz1RugcvNhDeYUNJbmxj1OXetzBJzuRaOOKf50j%2Fq%2BWGcQKu0KJwUV4fQxuv8RRvDJTjEk2R7%2F9bk5c6CLKmJnz8x86rHTLFKesjZPmpGtirN73L0VjersZV9MXKO9Y0Ef1fCjy0B5%2F%2F37uMPQWZkWgMOb75ccGOqUBeMHmUS93RCL7cnJNqxt5%2BaKLfWwLZyJAlXablxUP86GiBKmRAakoRdQDdPZvzNaxT168%2FZ1brTioGQUgWkkNEfrpsQvhX0PceB2w0lAU2Y24zxHHm2ype5juX5efBayagpngm2nJ9gu7BafpTvjNDlafzXVVGuPbnaHhrw2l%2BLbmvQ3RvXic94odJOzBe3Fl%2FZ5WZr8jW%2BH%2FNK5nqlJqlCZGprWJ&X-Amz-Signature=b6766eeb46ab667c88d7f20fc37aa2297b2da2128e2fcee9d05bff45c8f5add2&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

