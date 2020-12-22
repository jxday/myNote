# **org.springframework.util常用方法**

#### **ClassUtils**

```java
public static Class<?> getUserClass(Object instance) {
    Assert.notNull(instance, "Instance must not be null");
    return getUserClass(instance.getClass());
}
```

获取类