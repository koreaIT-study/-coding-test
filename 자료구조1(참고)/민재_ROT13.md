## 풀이시간약 25분
![image](https://user-images.githubusercontent.com/92290312/186900320-87657fc7-8882-4e0b-8100-1f79acc866d1.png)

```java
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;

public class Main {    
	 
    public static void main(String[] args) throws IOException{
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        char[] cArr = br.readLine().toCharArray();
        StringBuilder sb = new StringBuilder();
        
        for(char c : cArr) {
        	sb.append(encriptROT13(c));
        }
       
    	System.out.println(sb);
        br.close();
    }
    
    public static char encriptROT13(char c) {
    	if(c <= 90 && c >= 65) {
    		c += 13;
    		if(!(c <= 90 && c >= 65)) c -= 26;
    	}else if (c <= 122 && c >= 97) {
    		c+= 13;
    		if(!(c <= 122 && c >= 97)) c-=26;
    	}
    	return c;
    }
}

```
