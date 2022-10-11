## 풀이시간 1시간 넘은듯, 풀이봄 근데 풀이방식은 같았음
```java
import java.io.BufferedReader;
import java.io.InputStreamReader;

public class Main {
    public static void main(String[] args) throws Exception {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        int N = Integer.parseInt(br.readLine());
        long[] arr = new long[N];
        arr[0] = 1;

        if(N > 1) arr[1] = 2;
        
        for(int i = 2; i < N; i++){
            arr[i] = (arr[i-1] + arr[i-2]) % 10007;
        }

        System.out.println(arr[N-1]);
        br.close();   
    }
}
```
### 아래는 내 풀이인데 왜 틀린지 이해가 안됨
```
import java.io.BufferedReader;
import java.io.InputStreamReader;

public class Main {
    public static void main(String[] args) throws Exception {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        int N = Integer.parseInt(br.readLine());
        long[] arr = new long[N];
        arr[0] = 1; arr[1] = 2;
        for(int i = 2; i < N; i++){
            arr[i] = arr[i-1] + arr[i-2];
        }

        System.out.println(arr[N-1] % 10007);
        br.close();   
    }
}
```
