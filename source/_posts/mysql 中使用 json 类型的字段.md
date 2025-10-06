---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SA5CNBHT%2F20251006%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251006T110044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCRRZIm%2BTQWkWWU%2BXJGAoK2Sb5EEFtacD0qi8bGsoJuHAIgIIVcpDlW%2BH9ePmO69U1v59ejHqmEIUKs3qjyk0DovFcqiAQIi%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDPphmo2Mrvn3ZMfNvSrcA74QLQFKuxgjOjmv7OslC1lyZlgSSlm4%2BESXE1dI6iDrP2VJI7rE%2BZKARbLvUDD8YQJEbRlGSB1cduSsLmirffyZSBgnSBSPRKzx%2BEevGVjzS%2F3iZ7Bn%2BSHYsXUEhOqjjbztL4WygUfr2yb1%2FewKQvTx55CxqKBtA8mk1%2BpbBxpSqExk7lVO79r%2BelUxWNWsJKBofA8jAZ02CNhpNrKv4ozTXSmTvuu2kxoAMxYf4qlBAMKFS52ySpQpwe%2BF787kGEmTVXMzlMnNTomnDUXN4Fnl%2F2NET2%2BQnOm%2FQ3%2BvkrxCldCjXERYYC007O4UL2Ly5xddRM6Joma%2BofdNXksfLM%2FZbi3xdS0yTbnlxw7zE7BOL%2Fa1yAUnd%2Faav145WBU%2BvfLzecEsyn75wzCMNDM7hMelgw6cgR1BIlcrftPDesCKtH6zorkgQsklaIZVfveCP6XefgRU9IFcpNeDr%2BaVofyOG%2FgDo%2FO%2BhBvJUUF%2Fb2fWsQZaqdPSmZ1InNcObzubMpoI5L6N6PuXGojhVPoRm%2Ff6raJYkvPFGt969gjqfljH69HNoJ2WIPDzHRHKXhQmMbu9ZlRnxu1tt94BqRsiBG9AFNAVCrX4nQXLLJo%2BC3D41YGt%2FwW7XYmgCWSsMPuvjscGOqUB36IGWTfM7BtzhcO3acCwRD1KAY2NA9Is8QRkdnLCLPUpk7W5DlsSrH1wGyjvQUB18cICUXuhJyoMifhY1DG1M%2BaD4pXJElkzZRXiVfZpn%2Bm58s90FBSLJvxmR3xKqM8bEWZ32a%2FxymuIZOdCJ59weG1XumIpOcR2iUoQoUUyswWfrHoJmAVU7TywWj16S%2Fp%2FtVgj01e45EGQf15iZUkWMj3xWsUS&X-Amz-Signature=d4f51f7fd2a18d9dafea6e4ba393e3fbb85e33740e835de4029f32871248911e&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

