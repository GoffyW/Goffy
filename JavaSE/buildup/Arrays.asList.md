### Arrays.asList()

### Arrays.asList()可以将一个数组转化为集合，但是：

该方法为泛型方法，具体实现为

```Java
public static <T> List<T> asList(T... a) {
    return new ArrayList<>(a);
}
```

使用时必须注意的事项：

1. 传递的必须为对象数组，而不是基本类型。

   ```Java
   int [] myArray1 = { 1, 2, 3 };
   List myList1 = Arrays.asList(myArray1);
   System.out.println(myList1.size());//1
   System.out.println(myList1.get(0));//数组地址值 [I@1b6d3586
   ```

2. 当传入一个基本数据类型数组时，`Arrays.asList()` 的真正得到的参数就不是数组中的元素，而是数组对象本身！此时List 的唯一元素就是这个数组，这也就解释了上面的代码。

   解决这个问题只需要将数组类型变成他的包装类型

   ```Java
   Integer[] myArray1 = { 1, 2, 3 };
   ```

3. 使用集合的修改方法:add()、remove()、clear()会抛出异常。

   ```Java
   List myList1 = Arrays.asList(1, 2, 3);
   myList1.add(4);//运行时报错：UnsupportedOperationException
   myList1.remove(1);//运行时报错：UnsupportedOperationException
   myList1.clear();//运行时报错：UnsupportedOperationException
   ```

   因为：Arrays.asList()的返回值是 `java.util.Arrays` 的一个内部类，并不是java.util.ArrayList

   ```Java
   System.out.println("-----------"+myList1.getClass());
   //-----------class java.util.Arrays$ArrayList
   ```

下图是`java.util.Arrays$ArrayList`的简易源码，我们可以看到这个类重写的方法有哪些

```Java
 private static class ArrayList<E> extends AbstractList<E>
        implements RandomAccess, java.io.Serializable
    {
        ...

        @Override
        public E get(int index) {
          ...
        }

        @Override
        public E set(int index, E element) {
          ...
        }

        @Override
        public int indexOf(Object o) {
          ...
        }

        @Override
        public boolean contains(Object o) {
           ...
        }

        @Override
        public void forEach(Consumer<? super E> action) {
          ...
        }

        @Override
        public void replaceAll(UnaryOperator<E> operator) {
          ...
        }

        @Override
        public void sort(Comparator<? super E> c) {
          ...
        }
    }
```

`java.util.AbstractList`的`remove()`,理解为什么会抛出UnsupportedOperationException()异常

```Java
public E remove(int index) {
    throw new UnsupportedOperationException();
}
```

### 如何正确的将数组转换为ArrayList?

1. **最简便的方法(推荐)**

   ```Java
   List list = new ArrayList<>(Arrays.asList("a", "b", "c"))
   ```

2. **使用 Java8 的Stream(推荐)**

   ```Java
   Integer [] myArray = { 1, 2, 3 };
   List myList = Arrays.stream(myArray).collect(Collectors.toList());
   //基本类型也可以实现转换（依赖boxed的装箱操作）
   int [] myArray2 = { 1, 2, 3 };
   List myList = Arrays.stream(myArray2).boxed().collect(Collectors.toList());
   ```

3. **使用 Guava(推荐)**

   1. 对于不可变集合，你可以使用[`ImmutableList`](https://github.com/google/guava/blob/master/guava/src/com/google/common/collect/ImmutableList.java)类及其[`of()`](https://github.com/google/guava/blob/master/guava/src/com/google/common/collect/ImmutableList.java#L101)与[`copyOf()`](https://github.com/google/guava/blob/master/guava/src/com/google/common/collect/ImmutableList.java#L225)工厂方法：（参数不能为空）

      ```Java
      List<String> il = ImmutableList.of("string", "elements");  // from varargs
      List<String> il = ImmutableList.copyOf(aStringArray);      // from array
      ```

   2. 对于可变集合，你可以使用[`Lists`](https://github.com/google/guava/blob/master/guava/src/com/google/common/collect/Lists.java)类及其[`newArrayList()`](https://github.com/google/guava/blob/master/guava/src/com/google/common/collect/Lists.java#L87)工厂方法：

      ```Java
      List<String> l1 = Lists.newArrayList(anotherListOrCollection);    // from collection
      List<String> l2 = Lists.newArrayList(aStringArray);               // from array
      List<String> l3 = Lists.newArrayList("or", "string", "elements"); // from varargs
      ```

4. **使用 Apache Commons Collections**

   ```Java
   List<String> list = new ArrayList<String>();
   CollectionUtils.addAll(list, str);
   ```

   