---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664XUNUB3S%2F20250923%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250923T080043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCanQY%2Fsp1g9IWdhEm2%2FtpdYDnfBvB3levGoYBTU2odVQIgQiGDz7U4V8u%2F3Kdou5LMvhjSGURb%2BSE74iGU5uUVK1Mq%2FwMIQRAAGgw2Mzc0MjMxODM4MDUiDAe%2BKzFEL8gcWLa0ByrcA5vAQeSIzk22QTjqum%2Fs5t5FoAHluYmEi9urxGq0Kwf6Xv%2Bxlma91XrhZwhLraUOXR5FVSkCXPu7w78I1St8nIrYlnNDqOOkr%2BAlRTImFb6b6iE3ED90dW95vLMy7559Mv1CePAkwcsuA8s7Vjr2rVtwgwSbSxA6r0AuWozQpKylFAEsGKw4dZqbqGuhBoAySHenae8MhDdySyNIy9oBY53pUvmZEbK%2Fm%2BmWsQ27uzxLZaT9dPGLAaso5GDkvIswuQI0O2cKBwifaEeX851xAm0VNhUOylSescxQBeSe9VQlX7qiFkx0Rmy7sIeA4mM1FSkAlFoZwqPFYqlmwp9H7D%2FAGNUCux5JB423iigZKwXnkJHg9Rfh5FtyTWGrVo%2FvjOVGXBRxKpyLcxzjqTmWUdosXVFbFMI%2FoS0NnIvDvwehLC8BfkMVdzqkcyKsCqzyXmbKBPOlzXGvy5xZgg1d7Ii8HNzjpRgVDz908am%2F05EdK8243kO%2FGVXbbaPlUXldKAn8VmS8OAesaMgK85aNzBo7Z9%2F5fKfhSSJg%2F7c4e73TMzD1gNYHbG4mgObh0PjqXsn2RjXFWxcQvicRDNwxR69LXvikt%2FIoSEQ%2Btt%2B%2BnwWteZz6lfAbMNSIw0%2BYMIufycYGOqUB1TjwjISG5iTzNAylGreuyrgRD7qsDjGClLewSoJ5INmqxG7JcnIoUp%2BW7a4Wf3gf171CrUsjCcH1kppRHFKn%2F7gxiQoyy2bSIZg%2F%2B7oUMFnNeOKYj5QuUKxXaQZd730iFHVRv%2FCOkCPIpz52JHW6lXGajTCxVaHKG%2Bo06bWj6%2BJxegqY8pGe%2FqqbM%2BNOGbSdRgokvYymfq8inZ9qqAdm9SHry%2B8I&X-Amz-Signature=a45bf458fd399332106a8e6fd05db14e6909e10e9cdcf9b42aff60bb8d358263&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

