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
#### `ArrayList<>`
```java
List<Character> list = new ArrayList<>();
for (char c : text.toCharArray()) {
    list.add(c);
}
```
### To be modified chars
#### `char[i]`
```java
char[] chars = text.toCharArray();
chars[0] = Character.toUpperCase(chars[0]);
String modified = new String(chars);
```
#### `new ArrayList<>(...)`
```java
List<String> list = new ArrayList<>(Arrays.asList("J", "a", "v", "a"));
list.add("!");    
list.set(0, "X"); 
list.remove(1);   

System.out.println(list); // [X, v, a, !]
```
