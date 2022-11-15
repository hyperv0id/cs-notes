
## 题解

```C++
class Solution {
public:
    bool searchMatrix(vector<vector<int>>& matrix, int target) {
        int x =matrix.size()-1, y = 0;
        while(x>=0 && y< matrix[0].size()){
            if(matrix[x][y] > target){x--;}
            else if(matrix[x][y] == target){return true;}
            else{y++;}
            // cout<<matrix[x][y]<<endl;
        }
        return false;
    }
};
```
![](https://pic-1257412153.cos.ap-nanjing.myqcloud.com/images/images/2022/11/02/20221102230454-3c6e9f.png)