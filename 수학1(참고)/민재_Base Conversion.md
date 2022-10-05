## 풀이시간 약 50분
![image](https://user-images.githubusercontent.com/92290312/194018796-1c5f47fc-973f-4416-8051-1b5d6c828a17.png)

```java
import java.io.BufferedReader;
import java.io.InputStreamReader;
import java.util.StringTokenizer;

public class Main {
    public static void main(String[] args) throws Exception {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        StringTokenizer st = new StringTokenizer(br.readLine());
        int A = Integer.parseInt(st.nextToken());
        int B = Integer.parseInt(st.nextToken());
        int N = Integer.parseInt(br.readLine());
        int exponent = N - 1;
        StringBuilder sb = new StringBuilder();
        long temp = 0;
        
        st = new StringTokenizer(br.readLine());

        for(int i = 0; i < N; i++){
            temp += Integer.parseInt(st.nextToken()) * Math.pow(A, exponent);
            exponent--;
        }
        
        while(temp / B != 0){
            insertSb(sb, temp, B);
            temp /= B;
        }
        insertSb(sb, temp, B);
        System.out.println(sb);
        br.close();   
    }

    static void insertSb(StringBuilder sb, long temp, int B){
        StringBuilder tempSb = new StringBuilder();
        tempSb.append(temp % B).append(' ');
        sb.insert(0, tempSb);
    }
}
```
