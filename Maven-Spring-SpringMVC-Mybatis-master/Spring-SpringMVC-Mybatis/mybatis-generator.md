# Mybatis 逆向工程的基本流程
## 1. 配置逆向工程的配置文件 generatorConfig.xml
```
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE generatorConfiguration
        PUBLIC "-//mybatis.org//DTD MyBatis Generator Configuration 1.0//EN"
        "http://mybatis.org/dtd/mybatis-generator-config_1_0.dtd">

<generatorConfiguration>
    <!-- <context> 元素用于指定生成一组对象的环境。 子元素用于指定要连接到的数据库、 要生成对象的类型和要内省的表 -->
    <context id="testTables" targetRuntime="MyBatis3">
        <commentGenerator>
            <!-- 是否去除自动生成的注释 true：是 ： false:否 -->
            <property name="suppressAllComments" value="true"/>
        </commentGenerator>
        <!--数据库连接的信息：驱动类、连接地址、用户名、密码 -->
        <jdbcConnection driverClass="com.mysql.jdbc.Driver"
                        connectionURL="jdbc:mysql://localhost:3306/springmvc_mybatis?useUnicode=true&amp;characterEncoding=UTF-8"
                        userId="root"
                        password="992101">
        </jdbcConnection>

        <!-- 默认false，把JDBC DECIMAL 和 NUMERIC 类型解析为 Integer，为 true时把JDBC DECIMAL 和
            NUMERIC 类型解析为java.math.BigDecimal -->
        <javaTypeResolver>
            <property name="forceBigDecimals" value="false"/>
        </javaTypeResolver>

        <!-- targetProject:生成PO类的位置
         注意对于targetProject：In other environments（other than Eclipse）,
         this value should be an existing directory on the local file system.
         即对于非eclipse项目需要指定绝对路径
         -->
        <javaModelGenerator targetPackage="com.markliu.ssm.po"
                            targetProject="/media/markliu/Entertainment/Linux/SoftwareInformation/java/My_IntelliJ_projects/Spring-SpringMVC-Mybatis/src/main/java">
            <!-- enableSubPackages:是否让schema作为包的后缀 -->
            <property name="enableSubPackages" value="false"/>
            <!-- 从数据库返回的值被清理前后的空格 -->
            <property name="trimStrings" value="true"/>
        </javaModelGenerator>

        <!-- targetProject:mapper映射文件生成的位置 -->
        <sqlMapGenerator targetPackage="com.markliu.ssm.mapper"
                         targetProject="/media/markliu/Entertainment/Linux/SoftwareInformation/java/My_IntelliJ_projects/Spring-SpringMVC-Mybatis/src/main/java">
            <!-- enableSubPackages:是否让schema作为包的后缀 -->
            <property name="enableSubPackages" value="false"/>
        </sqlMapGenerator>

        <!-- targetPackage：mapper接口生成的位置 -->
        <javaClientGenerator type="XMLMAPPER"
                             targetPackage="com.markliu.ssm.mapper"
                             targetProject="/media/markliu/Entertainment/Linux/SoftwareInformation/java/My_IntelliJ_projects/Spring-SpringMVC-Mybatis/src/main/java">
            <!-- enableSubPackages:是否让schema作为包的后缀 -->
            <property name="enableSubPackages" value="false"/>
        </javaClientGenerator>

        <!-- 指定数据库表 -->
        <table tableName="items"></table>
        <table tableName="orders"></table>
        <table tableName="order_detail"></table>
        <table tableName="user"></table>

        <!-- <table schema="" tableName="sys_user"></table>
        <table schema="" tableName="sys_role"></table>
        <table schema="" tableName="sys_permission"></table>
        <table schema="" tableName="sys_user_role"></table>
        <table schema="" tableName="sys_role_permission"></table> -->

        <!-- 有些表的字段需要指定java类型
         <table schema="" tableName="">
            <columnOverride column="" javaType="" />
        </table> -->
    </context>
</generatorConfiguration>

```
## 2. 编写运行逆向工程的程序
```
public class MybatisGenerator {

	// 加载 log4j 配置文件
	static {
		try {
			String resource = "config/log4j.properties";
			InputStream inputStream = Resources.getResourceAsStream(resource);
			PropertyConfigurator.configure(inputStream);
		} catch (IOException e) {
			e.printStackTrace();
		}
	}

	@Test
	public void generator() throws Exception {
		List<String> warnings = new ArrayList<String>();
		File configFile = new File("/media/markliu/Entertainment/Linux/SoftwareInformation/java/My_IntelliJ_projects/Spring-SpringMVC-Mybatis/src/test/resources/generatorConfig.xml");
		ConfigurationParser cp = new ConfigurationParser(warnings);
		Configuration config = cp.parseConfiguration(configFile);
		DefaultShellCallback callback = new DefaultShellCallback(true);
		MyBatisGenerator myBatisGenerator = new MyBatisGenerator(config, callback, warnings);
		myBatisGenerator.generate(null);
	}
}

```
## 3. 生成的 po 类、mapper 类配置文件的目录结构
![生成的 po 类、mapper 类配置文件的目录结构](http://img.blog.csdn.net/20160823114143531)