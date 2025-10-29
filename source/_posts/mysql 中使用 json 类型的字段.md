---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UPVR7K7K%2F20251029%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251029T100041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBoaCXVzLXdlc3QtMiJHMEUCIQCHHKjwtr%2BuNnZO3%2Fhz9N7BRAo8jTnznlGPywyM2xg3dgIgBNf8BEVtd2biJ4hjfbh5OneMTXRn4FYNViY30YDi31wqiAQI0%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDM7TK25QDzpuDtng3CrcA%2FBg%2FPlijp2fFBeEGt%2BDG76tM8mo9sHucJjpfoVYHp1rbMOO4CjpZ%2B8Eu5dKthAFOKe1Af7ayoUd4f1z7Keb4mBpwkNiVUTRxMWFKMghtpAp2SdAjUorPwij%2BRi%2BkJgg9ymonvmlhyR6BImBG0778WgTEyuEplxK1aUBJSel3NZvpWLLCgWw%2BANlK7AekP02b19hRCR0EDVLHEKP7PW3sg5WaijBLbKTaq56Tqeuti2FYnQxWuMWSEkuPM56U23b9auvu3Ad9aZkhSHXKG5sYcbhcnWqC%2F7K%2FHKlGdHFFiV3BFTzPthVVDrhWOLdcHZnkA%2FnlRBe4b9YMQYB2pzEwr%2BpHoKfjdnIEccCy4zrClUx61D2a6xFCx3Hes8Ev93%2BF7zZkq6TjDNzex8Puv6nz39EpSvc%2BFt6UyTJuLdSKTSm5PY8%2FwPuFeMubaLTXh1yCiPB6imiIo2k2DyDeSigTyF8o97Py0Z2yemEOUjOJW%2FUkOKe1OHHT8vTeL8wKtX5Eie%2BSbRZpb%2B5zRMLTQn0mir6HJ98CIhuNDjYfnzMuopB825TraC2DEIpBt47IdQxqu4G7jYD2cNOKo9u6GMO538MTk%2BcFHEJ4w9eKRajhjMFIQPay1Q4HFbou1iXMILKh8gGOqUBcSB7NaI5mb4HuFcZ265zPHZIs1EewyJ5BPO%2B1VX2MOEQZUcWqN3sxLslxrVjgxgbTrGh4PvGsNAtaR97W%2Fp2k2NMymVHOsNYO1cwSlWxUPRjY0e7vCEK7UPZch5%2FT09SbndaH1Pi%2BGtE%2B6Pyz6nB9LuYks0NQg9s5y9AULxL2r787ia2EV5luW%2FWcbzUZtWZlWOjQZzntirgEcsWf1%2BrR4o5cZZV&X-Amz-Signature=9c9a96a8d956b26596072a6f9ae2ed92a3764b8c289abac556b969215b7557bf&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

