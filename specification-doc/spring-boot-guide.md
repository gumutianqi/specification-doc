# Spring-Boot工程启动参数

### 设置内存占用最大最小和初始值
```java
要加“m”说明是MB，否则就是KB了.
-Xms：初始值
-Xmx：最大值
-Xmn：最小值
java -Xms10m -Xmx80m -jar mod.jar &

时区设置
java  -jar -Duser.timezone=GMT+08 mod.jar &
```