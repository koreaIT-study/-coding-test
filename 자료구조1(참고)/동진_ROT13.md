<img width="432" alt="스크린샷 2022-08-27 오후 9 14 28" src="https://user-images.githubusercontent.com/82895809/187029805-2a1b097e-75cc-441a-b6de-a0cceaaf7fcd.png">

풀이시간 : 15분

```java
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;

public class Main {
    public static void main(String[] args) throws IOException {
        final BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        String str = null;


        while ((str = br.readLine()) != null) {
            final StringBuilder sb = new StringBuilder();

            for (int i = 0; i < str.length(); i++) {
                char charAt = str.charAt(i);
                final boolean l = charAt >= 65 && charAt <= 90;
                final boolean s = charAt >= 97 && charAt <= 122;

                if (l || s) {
                    if (l && charAt + 13 > 90) {
                        charAt = (char) (charAt - 13);
                    } else if (s && charAt + 13 > 122) {
                        charAt = (char) (charAt - 13);
                    } else {
                        charAt = (char) (charAt + 13);
                    }
                    sb.append(charAt);
                } else {
                    sb.append(charAt);
                }
            }

            System.out.println(sb);
        }
    }
}


```
