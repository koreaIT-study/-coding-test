## 풀이시간 약 40분 풀이봄(문제를 이해못해서 그냥 풀이를 봤음)
![image](https://user-images.githubusercontent.com/92290312/192807335-5443786a-93c7-4043-9a8c-20527138e6fb.png)
```java
import java.io.BufferedReader;
import java.io.InputStreamReader;
import java.util.StringTokenizer;

public class Main {
    
    public static void main(String[] args) throws Exception {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        int n = Integer.parseInt(br.readLine());
        int m = 0;
        StringTokenizer st = null;
        long[] arr = null;
        StringBuilder sb = new StringBuilder();
        
        for(int i = 0; i < n; i++) {
        	st = new StringTokenizer(br.readLine());
        	m = Integer.parseInt(st.nextToken());
        	
        	arr = new long[m];
        	for(int j = 0; j < m; j++) {
        		arr[j] = Integer.parseInt(st.nextToken());
        	}
        	
        	sb.append(getSumGCD(arr)).append("\n");
        }
        
        System.out.println(sb);
        br.close();   
    }
    
    public static long getSumGCD(long[] arr) {
    	long sum = 0;
    	for(int i = 0; i < arr.length - 1; i++) {
    		for(int j = i+1; j < arr.length; j++) {
    			sum += GCD(arr[i], arr[j]);
    		}
    	}
    	
    	return sum;
    }
    
    // 유클리드 호제법 구현
    // 큰 수를 작을 수로 나눴을때 나누어 떨어지면 그 작은 수가 최대 공약수임
    // 나누어 떨어지지 않으면 나눈 작은 수와 나눴을때의 나머지를 가지고 계속 
    public static long GCD(long a, long b) {
    	if(b == 0) return a;
    	else return GCD(b, a % b);
    }
    
}
```
