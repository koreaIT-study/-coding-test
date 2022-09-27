풀이 시간 : 20분  

![image](https://user-images.githubusercontent.com/67637716/192450818-bfc36d7d-00a8-4290-8b2c-a5ce5dcda760.png)  

long형을 써야하는데 int로 써서 계속 틀림  

```  java
package cote.baekjun.math1;

import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.StringTokenizer;

public class GCDSum {

    public static void main(String[] args) throws NumberFormatException, IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        final StringBuilder sb = new StringBuilder();

        int n = Integer.parseInt(br.readLine());

        while (n-- > 0) {
            StringTokenizer st = new StringTokenizer(br.readLine());
            int[] arr = new int[Integer.parseInt(st.nextToken())];

            for (int i = 0; i < arr.length; i++) {
                arr[i] = Integer.parseInt(st.nextToken());
            }

            sb.append(getGCDSum(arr)).append("\n");
        }
        System.out.println(sb);
    }

    private static long getGCDSum(int[] arr) {
        if (arr.length == 1)
            return arr[0];
        long sum = 0;
        for (int i = 0; i < arr.length; i++) {
            for (int j = i + 1; j < arr.length; j++) {
                sum += getGCD(arr[i], arr[j]);
            }
        }

        return sum;
    }

    private static long getGCD(int x, int y) {
        if (y == 0)
            return x;
        return getGCD(y, x % y);
    }
}

```
