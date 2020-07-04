java注解

#### @Target

java.lang.annotation.Target 设定自定义注解范围

java.lang.annotation.ElementYype   Target通过ElementType来指定注解适用范围的枚举集合

ElementType用法

| 取值            | 注解适用范围                                 |
| --------------- | -------------------------------------------- |
| METHOD          | 此自定义注解在方法上面声明                   |
| TYPE            | 用于描述类、接口（包括注解类型）或enum上声明 |
| ANNOTATION_TYPE | 用于注解类型上（被@interface修饰的类型）     |
| CONSTRUCTOR     | 用于描述构造器上                             |
| FIELD           | 用于描述域                                   |
| LOCAL_VARIABLE  | 用于描述局部变量                             |
| Package         | 用于记录java文件的package信息                |
| PARAMETER       | 可用于参数上                                 |



#### @Retention

java.lang.annotation.Retention 定义被它所注解的注解保留多久

java.lang.annotation.RetentionPolicy Retention通过RetentionPolicy 指定被它所注解的注解的生命周期

RetentionPolicy 用法：

| 取值    | 用法                                                         |
| ------- | ------------------------------------------------------------ |
| SOURCE  | 注解只保留在源文件，当Java文件编译成class文件的时候，注解被遗弃；被编译器忽略 |
| CLASS   | 注解被保留到class文件，但jvm加载class文件时候被遗弃，这是默认的生命周期 |
| RUNTIME | 注解不仅被保存到class文件中，jvm加载class文件之后，仍然存在  |

首先要明确生命周期长度 SOURCE < CLASS < RUNTIME ，所以前者能作用的地方后者一定也能作用。

1. 一般如果需要在运行时去动态获取注解信息，那只能用 RUNTIME 注解
2. 要在编译时进行一些预处理操作，比如生成一些辅助代码（如 ButterKnife），就用 CLASS注解
3. 只是做一些检查性的操作，比如 @Override 和 @SuppressWarnings，则可选用 SOURCE 注解









