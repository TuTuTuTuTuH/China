# Stairs

- 样例：   
$ python quiz_3.py  
Enter three nonnegative integers: 0 3 9  
Here is the grid that has been generated:  
1 1 0 1 1 1 1 1 1  
1 1 1 1 0 1 1 1 0  
0 1 1 0 1 1 1 1 1  
1 1 0 0 0 1 0 1 1  
1 1 0 1 1 1 1 1 0  
0 1 1 0 1 1 0 1 1  
1 1 0 1 1 1 1 1 1  
1 0 1 1 0 0 1 1 0  
0 1 1 1 1 1 1 1 1  
For steps of size 2, we have:  
5 stairs with 1 step  
1 stair with 2 steps  
1 stair with 3 steps  
1 stair with 4 steps  
For steps of size 3, we have:  
4 stairs with 1 step  

```cpp
vector<int> findStairs(vector<vector<int>> &matrix, int size) {     //提供一个矩阵matrix，和一个楼梯的size
	int l = matrix.size();
	vector<int> res(l, 0);                                            //保存答案
	for (int i = 0; i < l - size + 1; ++i) {                          //遍历，注意边界
		for (int j = 0; j < l - 2 * size + 2; ++j) {
			if (matrix[i][j] == 0 || matrix[i][j] == 2) continue;         //当为零或已作为起始点的时候，跳出到下一点
			int steps = 0, now_size = size - 1, dir = 0, ni = i, nj = j;  //steps表示步数，now_size
			while (ni < l && nj < l && matrix[ni][nj] != 0) {
				if (now_size == 0) {
					now_size = size - 1;
					if (dir == 2) {
						dir = 1;
						++steps;
						matrix[ni][nj - size + 1] = 2;
					}
					else ++dir;
				}
				if (dir == 0) {
					++nj;
					--now_size;
				}
				else if (dir == 1) {
					++ni;
					--now_size;
				}
				else if (dir == 2) {
					++nj;
					--now_size;
				}
			}
			++res[steps];
		}
	}
	return res;
}
```
