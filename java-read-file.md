```java
try(BufferedReader br = new BufferedReader(new FileReader("file.txt"))) {
  StringBuilder sb = new StringBuilder();
  String line = br.readLine();

  while (line != null) {
      sb.append(line);
      sb.append(System.lineSeparator());
      line = br.readLine();
  }
  String everything = sb.toString();
}
```
