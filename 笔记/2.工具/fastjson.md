# fastjson

#### SerializeWriter	相当于	StringBuffer

#### JSONArry	相当于	List<Object>

#### JSONObject	相当于	Map<String,Object>

#### JSON反序列化没有真正的数组，本质类型都是List<Object>

#### 

### 主要的使用入口

```java
public static final Object parse(String text); // 把JSON文本parse为JSONObject或者JSONArray 
public static final JSONObject parseObject(String text)； // 把JSON文本parse成JSONObject    
public static final <T> T parseObject(String text, Class<T> clazz); // 把JSON文本parse为JavaBean 
public static final JSONArray parseArray(String text); // 把JSON文本parse成JSONArray 
public static final <T> List<T> parseArray(String text, Class<T> clazz); //把JSON文本parse成JavaBean集合 
public static final String toJSONString(Object object); // 将JavaBean序列化为JSON文本 
public static final String toJSONString(Object object, boolean prettyFormat); // 将JavaBean序列化为带格式的JSON文本 
public static final Object toJSON(Object javaObject); 将JavaBean转换为JSONObject或者JSONArray。
```



#### List与JSON

```java
/**
List转json
*/
List<Student> students = new ArraryList();
String str = JSON.toJSONString(students);
/**
Json转list
*/
String json = "";
List<Student> students = JSON.praseArray(json,Student.class);
List<Student> students = JSON.praseObject(json,new TypeReference<List<Student>>(){});
```

