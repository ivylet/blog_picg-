### 开发步骤
创建对象时,通过`getbean(id)`方法获取所需的对象,
`getbean(id)`通过读取`XML`文件以及id来获得对象的全名,返回对应对象来完成对象创建.`XML`配置文件中的内容指向目的对象类.

解耦就是改变类时无序更改全部代码,只需更改`XML`中配置文件指向内容即可.

1. 导入Spring开发基本包坐标
2. 编写Dao接口和实现类
3. 创建Spring核心配置文件
4. 在Spring配置文件中配置`UserDaompl`
5. 使用Spring的API获得Bean实例

1. 导入坐标
2. 创建Bean
3. 创建`applicationContext.xml`
4. 在配置文件中进行配置
5. 创建`ApplicationContext`对象`geiBean`


