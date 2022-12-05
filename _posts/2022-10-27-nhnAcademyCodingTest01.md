---
layout: post
title: NhnAcademy 선발교육과정 4일차 코딩 테스트 (정렬)
date: 2022-10-27 09:00:00 +0800
last_modified_at: 2022-10-27 18:00:00 +0800
tags: [nhn]
toc:  true
published : true
---

### 비교정렬 알고리즘 비교


| 정렬 종류 | 시간                                     |주요 전략|비고|
|-------|----------------------------------------|---|---|
| 선택 정렬 | Best : O(n<sup>2</sup>) / Avg : O(n<sup>2</sup>) |(무순) 우선순위 큐|제자리|
| 삽입 정렬 | Best : O(n) / Avg : O(n<sup>2</sup>)            |(순서) 우선순위 큐|제자리|
| 버블 정렬 | Best : O(n<sup>2</sup>) / Avg : O(n<sup>2</sup>)           |(순서) 우선순위 큐|제자리|


### 선택 정렬
1. 주어진 리스트에서 최솟값을 가지는 자리를 찾는다.
2. 1번에서 찾은 자리를 첫번째 자리와 스왑한다.
3. 첫번째를 제외한 나머지 값들 중 최솟값을 찾고 1,2번을 반복한다.

```java
import java.util.Arrays;

public class SelectionSort {
  public static void main(String[] args) {
    int[] array = new int[10];
    for(int i = 0; i < array.length; i++){
      array[i] = (int)(Math.random() * 100);
    }
    sort(array);
  }
  static void sort(int[] array) {
    printArray(array);
    for(int i = 0; i < array.length-1; i++) {
      int min_index = i;

      for(int j = i + 1; j < array.length; j++) {
        if(array[j] < array[min_index]) {
          min_index = j;
        }
      }
      swap(array, min_index, i);
    }
    printArray(array);
  }
  static void swap(int[] a, int i, int j) {
    int temp = a[i];
    a[i] = a[j];
    a[j] = temp;
  }
  static void printArray(int[] array){
    System.out.println(Arrays.toString(array));
  }
}
```

### 삽입 정렬
1. 현재 타겟이 되는 숫자와 이전 위치에 있는 원소들을 비교한다. (첫 번째 타겟은 두 번째 원소부터 시작한다.)
2. 타겟이 되는 숫자가 이전 위치에 있던 원소보다 작다면 위치를 서로 교환한다.
3. 그 다음 타겟을 찾아 위와 같은 방법으로 반복한다.

```java
public class Insertion {
    public static void sort(int[] arr) {
        for (int i = 1; i < arr.length; i++) {
            int toInsert = arr[i];
            int j = i;
            while (j > 0 && arr[j - 1] > toInsert) {
                arr[j] = arr[j - 1];
                j--;
            }
            arr[j] = toInsert;
        }
    }
}
```
```java
public class Insertion {
    public static void sort(int[] arr) {
        for (int end = 1; end < arr.length; end++) {
            int i = end;
            while (i > 0 && arr[i - 1] > arr[i]) {
                swap(arr, i - 1, i);
                i--;
            }
        }
    }

    private static void swap(int[] arr, int a, int b) {
        int tmp = arr[a];
        arr[a] = arr[b];
        arr[b] = tmp;
    }
}
```
## 주말 과제( 퀵, 머지 정렬 추가 정리)
### 퀵 정렬
1. 기준이 되는 피벗을 하나 선택한다.
2. 피벗을 기준으로 양쪽에서 피벗보다 큰 값, 혹은 작은 값을 찾는다. 왼쪽에서부터는 피벗보다 큰 값을 찾고, 오른쪽에서부터는 피벗보다 작은 값을 찾는다.
3. 양 방향에서 찾은 두 원소를 교환한다.
4. 왼쪽에서 탐색하는 위치와 오른쪽에서 탐색하는 위치가 엇갈리지 않을 때 까지 2번으로 돌아가 위 과정을 반복한다.
5. 엇갈린 기점을 기준으로 두 개의 부분리스트로 나누어 1번으로 돌아가 해당 부분리스트의 길이가 1이 아닐 때 까지 1번 과정을 반복한다. (Divide : 분할)
6. 인접한 부분리스트끼리 합친다. (Conqure : 정복)

```java
import java.util.Arrays;

public class QuickSort {

  public static void main(String[] args) {
    int[] array = new int[10];
    for(int i = 0; i < array.length; i++){
      array[i] = (int)(Math.random() * 100);
    }
    printArray(array);
    sort(array, 0, array.length - 1);
    printArray(array);
  }

  private static void sort(int[] a, int lo, int hi) {
    if(lo >= hi) {
      return;
    }

    int pivot = partition(a, lo, hi);

    sort(a, lo, pivot);
    sort(a, pivot + 1, hi);
  }

  private static int partition(int[] array, int left, int right) {
    int lo = left - 1;
    int hi = right + 1;
    int pivot = array[(left + right) / 2];		// 부분리스트의 중간 요소를 피벗으로 설정


    while(true) {
      do {lo++;
      } while(array[lo] < pivot);
      do {hi--;
      } while(array[hi] > pivot && lo <= hi);
      if(lo >= hi) {
        return hi;
      }
      swap(array, lo, hi);
    }
  }
  private static void swap(int[] array, int i, int j) {
    int temp = array[i];
    array[i] = array[j];
    array[j] = temp;
  }
  private static void printArray(int[] array){
    System.out.println(Arrays.toString(array));
  }
}
```

### 합병정렬
1. 주어진 리스트를 절반으로 분할하여 부분리스트로 나눈다. (Divide : 분할)
2. 해당 부분리스트의 길이가 1이 아니라면 1번 과정을 되풀이한다.
3. 인접한 부분리스트끼리 정렬하여 합친다. (Conqure : 정복)
