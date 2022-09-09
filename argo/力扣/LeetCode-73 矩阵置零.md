### 原地算法



二维数组的长度获取
```java
	matrix.length  //  数组的行数  row
	matrix[0].length  //  数组的列数  col
```

```java
class Solution {  
public void setZeroes(int[][] matrix) {  
  
//定义横竖的长度  
int m = matrix.length;  
int n = matrix[0].length;  
  
//从matrix[0][0]开始，最顶层和最左层  
boolean heng = false;  
boolean shu = false;  
  
//判断（heng,shu）横竖是否有为零的地方  
for (int i = 0; i < m; i++) {  
	if (matrix[i][0] == 0) {  
		heng = true;  
	}  
}  
for (int j = 0; j < n; j++){  
	if (matrix[0][j] == 0){  
		shu = true;  
	}  
}  
  
//@@@@@@由左第二行和上第二行开始判断是否为零  
for (int i = 1; i < m; i++) {  
	for (int j = 1; j < n; j++) {  
		if (matrix[i][j] == 0) {  
			matrix[i][0] = matrix[0][j] = 0;  
		}  
	}  
}  

for (int i = 1; i < m; i++) {  
	for (int j = 1; j < n; j++) {  
		if (matrix[i][0] == 0 || matrix[0][j] == 0){  
			matrix[i][j] = 0;  
		}
	}  
}  
  
//至于为什么最后操作边界的值为0，如果提前操作会影响@@@@@@这一步  
if (heng){  
	for (int i = 0; i < m; i++){  
	matrix[i][0] = 0;  
	}
}  
if (shu){
	for (int j = 0; j < n; j++){  
	matrix[0][j] = 0;  
	}  
}  
  
}  
     }
```

```java
class Solution {  
    public void setZeroes(int[][] matrix) {  
        HashSet<Integer> row = new HashSet();  
        HashSet<Integer> col = new HashSet();  
        for (int i = 0;i<matrix.length; i++){  
            for (int j = 0; j<matrix[0].length; j++){  
                if (matrix[i][j] == 0){  
                row.add(i);  
                col.add(j);  
            }  
            }  
        }  
        for (int tmp : row) {  
            setrowZero(tmp,matrix);  
        }  
        for (int tmp : col) {  
            setcolZero(tmp,matrix);  
        }  
  
    }  
    public void setrowZero(int i,int[][] matrix){  
        for (int m = 0 ; m<matrix[0].length;m++)  
            matrix[i][m] = 0;  
    }  
    public void setcolZero(int j,int[][] matrix){  
        for (int n = 0;n<matrix.length;n++)  
            matrix[n][j] = 0;  
    }  
}
```