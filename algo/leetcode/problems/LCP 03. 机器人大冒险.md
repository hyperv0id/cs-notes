---
tags: [" #leetcode/medium  "]
---
> [LCP 03. 机器人大冒险](https://leetcode.cn/problems/programmable-robot/description/?envType=study-plan&id=lccup-2019-fall&plan=lccup&plan_progress=yhx1jp2)

## Solution

1. 先看看能不能到达终点，不能就直接返回`false`
1. 再逐个判断能不能到达障碍物
	- 如果能够说明挡住啦，直接返回`false`
	- 特判：障碍物在终点后面

```C++

class Solution {
public:
    bool robot(string command, vector<vector<int>>& obstacles, int x, int y) {
        bool reach = true;
        if(!can(command, x,y)){return false;}

        for(auto&&vec:obstacles){
            if(vec[0] > x || vec[1]>y){continue;}
            if(can(command,vec[0], vec[1])){return false;}
        }
        return true;
    }
    bool can(string command, int x,int y){
        int u=0,r=0;
        for(char c:command){
            if(c=='U')u++;
            else r++;
        }
        int m = min(x/r, y/u);
        x -= m*r;
        y -= m*u;

        cout<<x<<"--"<<y<<endl;
        if(x==0 && y==0){return true;}
        
        for(char c:command){
            if(c=='U')y--;
            else x--;
            
        cout<<x<<"--"<<y<<endl;
            if(x==0 && y==0){return true;}
        }
        return false;
    }
};

```