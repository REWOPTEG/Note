# 一、双色球红球选择

```java
// 双色球彩票：在33个红球(redBall[33])中，随机选择6个不重复的红球(balls[6])。
public static void computerSelection(int[] balls, int[] redBall) {
    Random r = new Random();

    int index;
    for (int i = 0; i < balls.length; i++) {
        index = r.nextInt(redBall.length - i);
        balls[i] = redBall[index];

        int temp = redBall[index];
        redBall[index] = redBall[redBall.length-1-i];
        redBall[redBall.length-1-i] = temp;
    }
}
```

# 二、插入排序

```java
public static void chaRu(int[] p) {
    int len = p.length;
    for (int i = 1; i < len; i++) {
        int temp = p[i];
        int j = i - 1;
        for (; j >= 0; j--) {
            if (temp < p[j]) {
                p[j+1] = p[j];
            } else {
                break;
            }
        }
        if (j+1 != i) {
            p[j+1] = temp;
        }
    }
    for (int x:p) {
        System.out.printf("%d  ", x);
    }
    System.out.println();
}
```

# 三、选择排序

```java
public static void xuanZe(int[] p) {
    int len = p.length;
    for (int i = 0; i < len - 1; i++) {
        int index = i;
        for (int j = i+1; j < len; j++) {
            if (p[index] < p[j]) {
                index = j;
            }
        }
        if (i != index) {
            int temp;
            temp = p[index];
            p[index] = p[i];
            p[i] = temp;
        }
    }
    for (int x:p) {
        System.out.printf("%d  ", x);
    }
    System.out.println();
}
```

# 四、冒泡排序

```java
public static void maoPao(int[] p) {
    int len = p.length;
    for (int i = 0; i < len - 1; i++) {
        for (int j = 0; j < len - i - 1; j++) {
            if (p[j] > p[j+1]) {
                int temp;
                temp = p[j];
                p[j] = p[j+1];
                p[j+1] = temp;
            }
        }
    }
    for (int x:p) {
        System.out.printf("%d  ", x);
    }
    System.out.println();
}
```

# 五、二分查找

```java
public static void erFen(int[] p, int s) {
    int start = 0;
    int end = p.length - 1;
    while (start <= end) {
        int middle = (start + end) / 2;
        if (s > p[middle]) {
            start = middle + 1;
        } else if (s < p[middle]) {
            end = middle - 1;
        } else {
            System.out.println(middle);
            return;
        }
    }
    System.out.println(-1);
}
```
