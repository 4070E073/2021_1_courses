#
```


```
# 填充題
```
public class Angle {
	public static void main(String[] args) {
		double angle = 60;				
                double radian = _____________________;   
		
		double sin = Math.sin(radian); 		
                System.out.println("sin=" + sin);	
		double cos = Math.cos(radian); 		
                System.out.println("cos=" + cos);
	 
	}

}
```
# 問答題
```
[1]舉例說明　傳值呼叫(call by value)　與　參考呼叫(call by reference)
[2]舉例說明　方法多載
```
# 程式設計題 四星彩開獎系統
```
程式執行時會顯示0~9重複亂數號碼。
本期四星彩開獎號碼如下：
9  7  1  8  
```
```
public class EX_4StarKiJiang {

	// getRnd靜態方法可用來取得n~m之間的num個亂數，並傳給所設定的陣列
	static void getRnd(int[] vArray, int min, int max, int num) {
		int max_dim, rem_num, choice;
		max_dim = max - min + 1;
		int[] t = new int[max_dim];
		for (int i = 0; i <= max_dim - 1; i++) {
			t[i] = min + i;
		}
		rem_num = max_dim;
		for (int i = 0; i <= num - 1; i++) {
			choice = (int) (Math.random() * rem_num);
			vArray[i] = t[choice];
			for (int j = choice; j < rem_num - 1; j++) {
				t[j] = t[j + 1];
			}
			rem_num--;
		}
	}

	public static void main(String[] args) {
		int[] lot = new int[4];
    
		getRnd(lot, 1, 9, 4);	//呼叫靜態方法getRnd
		
    System.out.println("Lotto number is:");
    
		for (int i = 0; i < lot.length; i++)
			System.out.print("  " + lot[i]);
	}
}
```
```
使用產生1-100的亂數
   int i;
   i = (int) (Math.random() * 100) +1;
```
# n!:遞迴方法 vs iterative
## iterative
```
ublic class Method1 {
	public static void main(String[] args) {
		factorial(6);
		factorial(9);
	}

	static void factorial(int x) {
		int i = x, j = 1;
		while(i > 0)
			j *= i--;
		System.out.println(x + "! = " + j);
	}
```
## 遞迴方法
```
終止式　==> 0!=1  1!=1
遞迴式  ==> f(n) = n*(n-1)*(n-2)*....*2*1
                 = n*f(n-1);

static void factorial(int x) {
	if (x == 1)
           return 1;
	else		
	  return (x*factorial(x-1));
	}
```

# 費氏數列:遞迴方法 vs iterative  ==> 再算　時間複雜度
```
費氏數列值第1及第2 項皆為1，
第3項之後的公式為f(n)= f(n-1) + f(n-2)。

1, 1, 2, 3, 5, 8, 13, 21, 34, 55, 89, 144, 233

使用者輸入一個正整數X，計算出費氏數列的第X項並輸出。

計算費氏數列的第X項，請輸入X= 12  
==>  費氏數列的第12項= 144

(1)請使用靜態遞迴方法算出費氏數列。
(2)請使用 iterative方法算出費氏數列。
```

```


```
