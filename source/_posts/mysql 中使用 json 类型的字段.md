---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466T55NRDDI%2F20251028%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251028T120053Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAMaCXVzLXdlc3QtMiJHMEUCIQCuEqbPzlIyl%2F8N0eS5kH7A87owTKf7ZxyiLer2zaR9EgIgZLe5zpZ8iEV4hGPYON44VgkpRsP4QSaurwYrrEEt1ZkqiAQIvP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDA%2BBHV%2Fq7eBahkER2CrcAy9nLltRlb4uPmrEVR%2BOcEUk7UB0sJKXeQOWKR%2FFNe8BG5h0rxXfgtf7gUtjWq4Nv2HVxRKS3TKy%2FZYukdn2vzKcMmBAr5W5r0NUH2JdsddxopzRX3kkksoi2O0QQ17g8dqSwpfuTzwMy56AgRNr6QAeQBX5mlWpCOs29OVew9tQ1QWBaR%2BdegRbLmm0Eps1Ksy9yWfSRShdJfWpgGmdZeg5l%2FCt%2FFIHN%2B%2BPLH1zUi%2BPaBgcJqQ7FZE0sXd1wqzagXmA9o77dyOQGJJ%2BMegvnLm3A2JnJUfvAuJXUBuj%2FPAoNdsgjag1nm4aJaFy88iv7bdPEhIbCp1iAkr8A8f1HzBCX6%2FfYHH40jt2wD7hAmD9fv5KyP%2FEPR4Iqoa52xh%2B5HJ7Ag3%2BhEcA%2BFyAuJ%2BF7xBdhfSCOgV%2BfVhHHLiPypizRg3CXAajbGTGCvmZlYKmZje8ca%2BACo%2BNiRfQhpqn6YbDqdXYmqV2o41cvZhkroxU3uRpasAq46WoqWk2JsVCrm3cR3lSJZ0%2B0MgvdNt5Kja33cKbfJ6TelrkUS9t9RuX%2F%2BrcDOqxcYKkuZraqev3zBXW3VXLe%2FWrNdOQAoYgzZr7N1nMDS8GoicncdzoovUYoAWq5zdNi%2Bk%2BI6dMMMfIgsgGOqUBwTYL9Ay0kJP1J3n0%2F0aOXMtMftbjo9QmdFTbQNQB8QXDLHUo3sknDZIsUJNq4PjGAETgvCkolRroEIOFtFFLyCsTN9UjxygkDIlDWWJTaw0eKRaii1Wm5CHyE41QL7%2FfGdWP%2FtDztCgFJv4E52Oa2VkCeQk0popfQVZ2t5mWrY1h%2FqUvCIg%2F%2Bj0tRD%2BnVOoOwUeIx%2FvorkrGoH%2F3TFXL4Qpso77w&X-Amz-Signature=5f7f0318556848e23bb8438ec024390469803a29269448a52b04ba67126e90c9&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

