# String
## String iteration
### Not reusable chars
#### `for` + `chartAt()`
```java
String text = "Hello";
for (int i = 0; i < text.length(); i++) {
    char c = text.charAt(i);
    System.out.println(c);
}
```
#### `for-each` on char array
```java
for (char c : text.toCharArray()) {
    System.out.println(c);
}
```
#### Stream API + `String.chars()`
```java
text.chars().forEach(c -> System.out.println((char) c));
```
### Reusable chars
#### `for` + `char[]`
```java
char[] chars = text.toCharArray();
for (char c : chars) {
    ...
}
```
#### 