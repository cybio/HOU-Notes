# Java

### 浮点数的比较

浮点数存在舍入误差, 数字不能精确表示.   
如果需要进行不产生舍入误差的精确数字计算, 需要使用__BigDecimal__类.

```Java
import java.math.BigDecimal;

public class Main {
    public static void main(String[] args) {
		double a = 1.0;
		BigDecimal bd = BigDecimal.valueOf(1.0);
		bd = bd.subtract(BigDecimal.valueOf(0.1));
		bd = bd.subtract(BigDecimal.valueOf(0.1));
		bd = bd.subtract(BigDecimal.valueOf(0.1));
		bd = bd.subtract(BigDecimal.valueOf(0.1));
		bd = bd.subtract(BigDecimal.valueOf(0.1));
		
		System.out.println(bd);	// 0.5
		System.out.println(a - 0.1 - 0.1 - 0.1 - 0.1 - 0.1); // 0.5000000000000001
		System.out.println(BigDecimal.valueOf(a) == bd); // false
    } 
}

```