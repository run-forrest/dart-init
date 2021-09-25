其他的太基础，记录下需要学习的
### 级联符号
级联运算符 (.., ?..) 可以让你在同一个对象上连续调用多个对象的变量或方法。除了方法调用，你也可以访问同一个对象的属性。这样通常可以让你免于创建临时变量并且写出更加顺畅的代码。

```
var paint = Paint()
  ..color = Colors.black
  ..strokeCap = StrokeCap.round
  ..strokeWidth = 5.0;
  
var paint = Paint();
paint.color = Colors.black;
paint.strokeCap = StrokeCap.round;
paint.strokeWidth = 5.0;
```
上面两者的代码是相等的

级联运算符可以嵌套，例如：
```
final addressBook = (AddressBookBuilder()
      ..name = 'jenny'
      ..email = 'jenny@example.com'
      ..phone = (PhoneNumberBuilder()
            ..number = '415-555-0100'
            ..label = 'home')
          .build())
    .build();
```
在返回对象的函数中谨慎使用级联操作符。例如，下面的代码是错误的：
```
var sb = StringBuffer();
sb.write('foo')
  ..write('bar'); // Error: method 'write' isn't defined for 'void'.
```
严格来说 .. 级联操作并非一个运算符而是 Dart 的特殊语法。
把..等价于上一句语法的返回值，可以这么简单理解

### 捕获异常
对于可以抛出多种异常类型的代码，也可以指定多个 catch 语句，每个语句分别对应一个异常类型，如果 catch 语句没有指定异常类型则表示可以捕获任意异常类型：
```
try {
  breedMoreLlamas();
} on OutOfLlamasException {
  // A specific exception
  buyMoreLlamas();
} on Exception catch (e) {
  // Anything else that is an exception
  print('Unknown exception: $e');
} catch (e) {
  // No specified type, handles all
  print('Something really unknown: $e');
}
```
关键字 rethrow 可以将捕获的异常再次抛出：
```
void misbehave() {
  try {
    dynamic foo = true;
    print(foo++); // Runtime error
  } catch (e) {
    print('misbehave() partially handled ${e.runtimeType}.');
    rethrow; // Allow callers to see the exception.
  }
}

void main() {
  try {
    misbehave();
  } catch (e) {
    print('main() finished handling ${e.runtimeType}.');
  }
}
```
