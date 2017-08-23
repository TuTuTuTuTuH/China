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
//提供一个矩阵matrix，和一个楼梯的size
vector<int> findStairs(vector<vector<int>> &matrix, int size) {     
	int l = matrix.size();
	//保存答案
	vector<int> res(l, 0);                                            
	//遍历，注意边界
	for (int i = 0; i < l - size + 1; ++i) {                          
		for (int j = 0; j < l - 2 * size + 2; ++j) {
			//当为零或已作为起始点的时候，跳出到下一点
			if (matrix[i][j] == 0 || matrix[i][j] == 2) continue;         
			//steps表示步数，now_size表示横向或纵向移动的步长，dir表示移动的方向(为偶数时向右，为奇数时向下)，ni,nj表示当前坐标
			int steps = 0, now_size = size - 1, dir = 0, ni = i, nj = j;  
			while (ni < l && nj < l && matrix[ni][nj] != 0) {
				//判断是否转向
				if (now_size == 0) {
					now_size = size - 1;
					if (dir == 2) {
						dir = 1;
						++steps;
						matrix[ni][nj - size + 1] = 2;
					}
					else ++dir;
				}
				//根据不同方向取得下一位置
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
			//当前步长楼梯数加一
			++res[steps];
		}
	}
	return res;
}
```
