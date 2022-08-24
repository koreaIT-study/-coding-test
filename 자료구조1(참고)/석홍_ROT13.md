풀이 시간 : 10분  

![image](https://user-images.githubusercontent.com/67637716/186374246-02a5209a-2edc-4138-a98f-c6ac7d1e2615.png)  


``` java
package cote.baekjun.dataStructure;

import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;

public class Rot13 {
    public static void main(String[] args) throws IOException {

        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        StringBuilder sb = new StringBuilder();
        String cmd = br.readLine();

        for (int i = 0; i < cmd.length(); i++) {
            char c = cmd.charAt(i);

            if (c >= 65 && c <= 90) {
                sb.append(c + 13 > 90 ? (char) (c + 13 - 26) : (char) (c + 13));
            } else if (c >= 97 && c <= 122) {
                sb.append(c + 13 > 122 ? (char) (c + 13 - 26) : (char) (c + 13));
            } else {
                sb.append((char) c);
            }
        }

        System.out.println(sb);
    }
}

```  
