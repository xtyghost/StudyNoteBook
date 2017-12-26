### 题1

```java
byte b = 1;
byte b2 = 2;
b = b1 + b; //b1加b值为int类型。 b += b1;可以，为赋值运算，自动转换。
System.out.println(b);
```

### 题2

```java
int a = 0;
a = a++; //先将值0临时储存，然后运算a++，a为1，然后将储存的值0赋给a，a为0；
System.out.println(a);
```

题3

```java
class Fu{
  int num = 3;
  void show(){
    System.out.println("Fu show");
  }
  static void method(){
    System.out.println("Fu static method");
  }
}
class Zi extends Fu{
  int num = 4;
  void show(){
    System.out.println("Zi show");
  }
  static void method(){
    System.out.println("Zi static method");
  }
}
class DuoTaiDemo3{
  public static void main(String[] args){
    Fu z = new Zi();
    System.out.println(z.num); // 3
    z.show(); // Zi show
    z.method(); // Fu static method
  }
}
```

题4 （多态）

```java
class A {
	 public String show(D obj){
			return ("A and D");
	 }
	 public String show(A obj){
			return ("A and A");
	 }
}
class B extends A{
	 public String show(B obj){
			return ("B and B");
	 }
	 public String show(A obj){
			return ("B and A");
	 }
}
class C extends B{}
class D extends B{}

class  DynamicTest
{
	public static void main(String[] args){

	A a1 = new A();
	A a2 = new B();
	B b = new B();
	C c = new C();
	D d = new D();
	System.out.println(a1.show(b));  // A and A
	System.out.println(a1.show(c));  // A and A
	System.out.println(a1.show(d));  // A and D
	System.out.println(a2.show(b));  // B and A
	System.out.println(a2.show(c));  // B and A
	System.out.println(a2.show(d));  // A and D
	System.out.println(b.show(b));   // B and B
	System.out.println(b.show(c));   // B and B
	System.out.println(b.show(d)); 	 // A and D

	}
}
```

