使用spring boot解析四种不同的文件

首先都要将文件进行上传保存（大概率上是zip的包）

然后再指定的位置进行解压，并将解压之后的记录保存到数据库中

可以从数据库中相应的字段读取文件然后将文件进行解析（这个操作是异步的还是分开进行操作  先把对应的接口写好吧）

需要使用的接口

1. 上传文件保存到数据库中（这个应该只有一个接口能够上传所有的类型的文件）
2. 解压压缩包（这个应该是需要进行遍历的操作 直接解压到最深的底部）
3. 将压缩包（即便是压缩包套压缩包）中的文件都给解压出来 保存到数据库中相应的记录中
4. 根据条目名称读取相应的文件的位置
5. 四个解析不同的文件的接口
    1. 根据实际的业务逻辑 将解析之后的数据分别放在其他不同的表中
6. 上层的一个方法 判断该文件应该用哪中解析方法进行解析

* txt

* csv

* xlsx

    1. 导入pom文件

        ```xml
        <dependency>
            <groupId>org.apache.poi</groupId>
            <artifactId>poi</artifactId>
            <version>3.16</version>
        </dependency>
         
        <dependency>
            <groupId>org.apache.poi</groupId>
            <artifactId>poi-ooxml</artifactId>
            <version>3.16</version>
        <dependency>
        ```

    2. 设置配置文件 设置上传的最大的限制，默认上是1MB

        ```properties
        # 上传文件总的最大值
        spring.servlet.multipart.max-request-size=10MB
        # 单个文件的最大值
        spring.servlet.multipart.max-file-size=10MB
        ```

    3. 设置解析的工具类