# [Java] Hexadecimal Color Code
> 파이차트를 그리기 위한 데이터를 줄 때 랜덤한 초록색을 같이 보내주려 한다.

## Convert a RGB Color Value to a Hexadecimal String
```java  
Color greenColor = new Color(0, random.nextInt(10, 100), 0);
String colorHex = String.format("#%06x", greenColor.getRGB() & 0xFFFFFF)
```

###### 참고
- [stackoverflow comment](https://stackoverflow.com/a/3607942)