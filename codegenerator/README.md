# 海南征信类型项目代码生成器

## 项目编辑项说明

### Java 后端

- **项目名称**

    项目名称在海南征信类型的项目中，只是用于识别项目名称，不会对生成的代码产生任何影响。

- **项目代码名**

    项目代码名是用于生成代码的项目名称，项目代码名会影响生成的代码的包名，类名等。下面的例子假设当前的项目代码名为```creditwzs```

    - 用于生成代码文件的基础路径
    
    ```
    hncrc-module-ProjectPathString-biz\src\main\java\com\hainancrc\module\creditwzs
    ```

    - 生成 Java 代码时，项目代码名会影响生成的 Java 类的包名，类名等。

    ```java
    package com.hainancrc.module.creditwzs.entity;

    import java.math.BigDecimal;
    import lombok.*;
    import java.util.*;
    import com.baomidou.mybatisplus.annotation.*;
    import com.hainancrc.framework.mybatis.core.dataobject.BaseDO;
    
    ...
    ```

    - **表名前缀**

    表名前缀是用于生成代码的表名前缀。

    - **项目配置**

        - ProjectPathString

        项目代码名对应的项目路径字符串，用于生成代码文件的基础路径。下面的例子假设当前的项目ProjectPathString配置名为 ```credit-wuzhishan```

        用于生成代码文件的基础路径

        ```
        hncrc-module-credit-wuzhishan-biz\src\main\java\com\hainancrc\module
        ```


> ref codegenerator/webui/Standardizations/Hncrc/README.md