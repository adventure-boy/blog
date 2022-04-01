### 一，双层循环，逐一对比，删除重复

```java
public   static   List  removeDuplicate(List list)  {       
  for  ( int  i  =   0 ; i  <  list.size()  -   1 ; i ++ )  {       
      for  ( int  j  =  list.size()  -   1 ; j  >  i; j -- )  {       
           if  (list.get(j).equals(list.get(i)))  {       
              list.remove(j);       
            }        
        }        
      }        
    return list;       
}
```

# 二，通过HashSet踢除重复元素

```java
public static List removeDuplicate(List list) {   
    HashSet h = new HashSet(list);   
    list.clear();   
    list.addAll(h);   
    return list;   
}
```

# 三，通过Java8 Stream流去重

```java
List arrayList = list.stream()
        .distinct()
        .collect(Collectors.toList());

```

### 四，利用List的contains方法循环遍历,重新排序,只添加一次数据,避免重复

```java
private static void removeDuplicate(List<String> list) {
    List<String> result = new ArrayList<String>(list.size());
    for (String str : list) {
        if (!result.contains(str)) {
            result.add(str);
        }
    }
    list.clear();
    list.addAll(result);

```

