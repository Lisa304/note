---
title: 資料結構與演算法2筆記
tags: 筆記
---
# 資料結構與演算法2筆記
>這學期用js寫!
>
## 2/25
1. 這學期會從 dynamic programming 開始，有四個方向。
2. 今天點名有點到。

## 3/4


### Dynamic Programming
> Divid and Conger
> 是一種解決「可以拆解」的問題的策略，D.P.是他其中一種應用。
> 
>D.P.=>
>1. 將問題拆解到最小的程度，然後求解<mark>並將該解保留下來。</mark>
>2. 接下來對上一層的問題求所有可能的拆解方式，用拆出的子問題的答案組合、比較已找出最佳解並記錄。
>3. 不斷重複，直到找出母問題的最佳解。


### Knapsack(背包) Problem
> 如何在下述 Knapsack 放進總價值最高的items:
> a knapsack with M capacity, N items with different size, value
> a[M] => size[N] value[N]

| item  | A   | B   | D   | C   | E   |
|:----- | --- | --- | --- | --- | --- |
| size  | 3   | 4   | 8   | 7   | 9   |

``` c++=
for (int j = 1 ; j<=N ; j++){ // j是第幾個物件
    for (int i = 1; i<=M ; i++){ // i是第幾格背包
	if ((i-size[j])>=0){ //背包放得進去嗎?這個到後面都一定是true 
	    if (cost[i] < (cost[i-size[j]]+ value[j])){
		cost[i] = cost[i-size[j]]+value[j];
		best[i] = j;
	    }
        }
    }
}
```

說明: cost[i] 是原來的最佳解；cost[ i-size[ j ] ]是扣掉一個j的容量剩下的總量(目前最佳解)；第二個 if 是 triple operation。

![](https://i.imgur.com/NmEkPmJ.jpg)

1. 當碰到一樣會留下舊的(在這邊的程式碼)
2. 背包可沒有第零格阿

- [name=盧韋彤]
將上一次產生的cost值，跟這輪新產生的值比大小，取cost較大值，假如一樣就用上輪的cost值。
產生新cost值的方法:往前找第size個值，將其的best值加上value
![](https://i.imgur.com/sG9Fa9Q.jpg)
### Matric Chain Production 矩陣連乘
> 矩陣:
> 1. 有結合率( A * B * C == (A*B)*C)
> 2. 沒有交換率(|A| * |B| != |B| * |A|)

``` javaspript=
var i,j,k;
// var cost[N][N];
for ( i = 1; i<=N ; i++) {
  for ( j = i+1; j<=N ; j++) {
    cost[i][j] = Max;
  }
}
for ( i=1 ; i<=N ; i++) {
  cost[i][i] = 0;
}
// above is initialization
for ( j=1; j<=N-1 ; j++) {
  for ( i=1 ; i<=N-j ; i++){
    for ( k=i+1 ; k<=i+j ; k++ ){
	  t = cost[i][k-1] + cost[k][i+j]+r[i]*r[k]*r[i+j+1];
	  // r是row
	  if ( t < cost[i][i+j]){
	    cost[i][i+j] = t;
		best[i][i+j] = k;
	  }
	}
  }
}
```
3. 前提
   - ![](https://i.imgur.com/34m16tE.png)
   - r那邊儲存了行列數，兩個矩陣的四個行列，不用用四格來儲存，因為他們會共用一格
4. 看A-C 
   - ![](https://i.imgur.com/eqsGgQz.png)
   - 看A-C代表A.B.c的連乘
   - 4\*2*3 => 做24次運算
     - 4行2列乘上2行3列 => 變成4X3的陣列了(每個元素做兩次運算)
   - 上面是(4\*2=8)，然後A(B C)，6+8=14
   - 有36跟14，選擇比較小的14
5. 看 A-F
   - ![](https://i.imgur.com/5klBk0o.png)

6. 結論 
   - ![](https://i.imgur.com/P5aopJK.png)
   - ![](https://i.imgur.com/V0mNU8P.png)
   - 紅色的括號，是看右邊的表格，整個表格都可以看，( A B C )那就對照表格，找到橘色框框的地方，看到左下角寫B，那就是會切在B前面，得到( A (B C))
 
### TODO
## 3/11
### Matric Chain Production 矩陣連乘
> 繼續說這個
> 
> 要怎麼應用呢?

### Optimal Binary Searching Tree
#### 前提
 - 一株二元樹
 - 對每個節點(i)來說:
    - 左子樹的key值 <= i的key值
    - 右子樹的key值 > i的key值
    - 就是只能放一邊，這樣樹比較漂亮，這堂課上講，左邊放小的
 - 情景: 如果每個節點的出現頻度都知道......
 - ![](https://i.imgur.com/sghwUME.png)
 - 想要求: 如何配置二元搜尋樹各節點的位置，使得內部路徑長最小?
 - 應用?

#### 說明 
1. 一開始
   - ![](https://i.imgur.com/xyVdnjk.png)
   - 最上面的應該要寫4X1才對
   - 2X2是代表，要走到B，要經過兩個節點，而B的頻率是2
   - Internal length of path 內部路徑長
2. 想要得到最小勢必跟排的方法有關:
   - 三個節點有五個排法
3. code
``` javascript=
for(i=1;i<=N;i++){
  for(j=i+1;k<=N+1;j++){
    cost[i][j]=Max;
  }
} //初始化
for(var i=1;i<=N;i++){
  cost[i][j]=freq[i];
}//把freq填進去
for(var i=1; i<=N; i++){
  cost[i][i-1] = 0;
}//全部矩陣都填上0 把對角線左邊的藍色填進去???
for( var j=1 ;j<=N-1;j++){ //j表要看幾個節點
  for( var i=1; i <= N - j; i++){//i表從誰開始看
    for( var k=i; k<=j; k++){ //k表示root
	  tmp = cost[i][k-1]+ cost[k+1][i+j];// 佐子數的內部路徑長+ 右子樹的內部路徑長
	  if (tmp < [i][i+j]){
	    cost[i][i+j] = tmp;
		best[i][i+j] = k;
	  }
	}// triple operation
	tmp = 0;
	for (var k=i; k<= i+j; k++){
	  tmp = tmp + freq[k];
	  //i~i+j的freq累加變成第26行的tmp
	}// 左子樹接到k下面
	cost[i][i+j] = cost[i][i+j] + tmp;
	// 左、右子樹獨立時的cost
  }
}
```
4. A當root跟B當root
   - ![](https://i.imgur.com/3FrRJVB.png)
   - 框框左下角代表誰當root
5. 算C
   - ![](https://i.imgur.com/Hdp3k7c.png)

6. 

### TODO

## 3/18
> comic + real face