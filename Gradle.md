### Groovy

```groovy
// 可选类型
def a = 1

// 普通字符串
def s1 = 'a'
// 可使用变量
def s2 = "a ${value}"
// 可换行
def s3 = '''a
b
c'''

// list集合 (ArrayList)
def list = ['a','b','c']
// 追加元素
list << 'd'

// map集合 (LinkedHashMap)
def map = ['a':1,'b':2,'c':3]
// 追加元素
map.d = 4

// 闭包
// 带参数闭包
def c1 = {
    v ->
    	println v
}
// 不带参数闭包
def c2 = {
    println 'hello'
}
// 使用闭包
def method1(Closure closure) {
    closure('hi')
}
def method2(Closure closure) {
    closure()
}
method1(c1) // hi
method2(c2) // hello
```

