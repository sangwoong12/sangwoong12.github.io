---
layout: post
title: Homework
date: 2022-10-27 09:00:00 +0800
last_modified_at: 2022-10-27 18:00:00 +0800
tags: [nhn,homework]
toc:  true
---

### 구구단 - while

```java
public class GugudanToWhile {

  public static void main(String[] args) {
    int i=1;
    while(i<10){
      int j=1;
      while(j<10){
        System.out.printf("%d * %d = %d\n",i,j,i*j);
        j++;
      }
      i++;
    }
  }
}
```

### 구구단 - for

```java
public class GugudanToFor {

  public static void main(String[] args) {
    for(int i=1; i<10; i++){
      for(int j=1;j<10;j++){
        System.out.printf("%d * %d = %d\n",i,j,i*j);
      }
    }
  }
}
```

### 구구단 - while-for

```java
public class GugudanToWhileFor {

  public static void main(String[] args) {
    int i=1;
    while(i<10){
      for(int j=0; j<10; j++){
        System.out.printf("%d * %d = %d\n",i,j,i*j);
      }
      i++;
    }
  }
}
```

### 구구단 - for-while

```java
public class GugudanToForWhile {

  public static void main(String[] args) {
    for(int i=0;i<10;i++){
      int j=1;
      while(j<10){
        System.out.printf("%d * %d = %d\n",i,j,i*j);
        j++;
      }
    }
  }
}
```

### 무한루프


```java
import java.util.Scanner;

public class InfiniteLoop {

  public static void main(String[] args) {
    Scanner scanner = new Scanner(System.in);
    int result = 0;
    boolean stop = true;
    while(stop){
      System.out.println("숫자를 입력하세요 (종료 \"c\"):");
      String number = scanner.next();
      if(number.equals("c")){
        System.out.println("System exit");
        scanner.close();
        stop = false;
      }
      else{
        try{
          result+=Integer.parseInt(number);
        }catch (NumberFormatException e){
          System.out.println("Please Input Integer or c");
        }
        System.out.printf("result:%d\n",result);
      }
    }
  }

}
```

### 배열 정렬

```java
import java.util.Arrays;

public class SortArray {

  public static void main(String[] args) {
    int[] array = {3,9,4,2,6,22,7};
    int tmp;
    for(int i = 0; i < array.length; i++){
      for(int j = i+1; j < array.length; j++){
        if(array[i] > array[j]){
          tmp = array[i];
          array[i] = array[j];
          array[j] = tmp;
        }
      }
    }
    System.out.println(Arrays.toString(array));
  }
}

```
