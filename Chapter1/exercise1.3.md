

# 思考题1.3

**P45.** Q1.将上述例题1.3的线性复杂度算法写成伪代码。P45

答：答案见[例题1.3](./0.summary.md#例题13)

**P45.** Q2.在一个数组中寻找一个区间，使得区间内的数字之和等于某个事先给定的数字。( AB、FB、LK等公司的面试题，后面会解答。P45

答：给定 `target`可以有两种方式获取：
- 通过直接遍历`[0, k]`求和得到；
- 通过遍历的两个以`0`为左边界区间和之差`S(p, q) = S(0, q) - S(0, p)`得到。
- 通过字典存储区间数字和及对应的右边界；

```cpp
#include <iostream>
#include <vector>
#include <cfloat>
#include <unordered_map>

using namespace std;

class Solution{
public:
    pair<int, int> findRange(double *nums, int size, int target) {
        unordered_map<double, int> map;
        int sum = 0.0;
        for(int i = 0; i < size; i++) {
            sum += nums[i];
            if(sum == target) {
                return make_pair(0, i);
            }if(map.find(sum - target) != map.end()) //字典中有sum - target
                return make_pair(map.find(sum - target)->second + 1, i);

            if(map.find(sum) == map.end()) //sum不存在于字典中
                map.insert({sum, i});
        }
    }
};


int main() {
    Solution solution;
    int _size = 12;
    double target = 18;
    double _nums[] = {1, -12,3,-5,23,-1,-12,34,5,-7,1,-4};
    auto res = solution.findRange(_nums,_size, target);
    cout << res.first <<" " <<res.second <<endl;
}

```

**P45.** Q3.在一个二维矩阵中，寻找一个矩形的区域，使其中的数字之和达到最大值。(例题1.3的变种，硅谷公司真实的面试题。） 

答： 
- 假设第一个元素为最大值；
- 将第一行(列)作为基准遍历整个矩阵；
  - 将第一行(列)作为基准，看作一个一维数组进行遍历，找到最大子序列；
  - 将第一行(列)和第二行(列)看作一个一维数组，通过逐项相加，将两行(列)合并为一行(列)；
  - 按照上述步骤循环遍历；
- 将第二行(列)作为基准遍历整个矩阵，直至遍历整个矩阵；

```cpp
#include <iostream>
#include <vector>

using namespace std;

class Solution{
public:
    vector<int> getMaxMatrix(double matrix[][2], int m, int n) {
        int r1 = 0, r2 = 0, c1 = 0, c2= 0;
        double maxValue = matrix[0][0];

        for(int i = 1; i < n; i++) {
            double matrix_list[m];
            for(int j = i; j < n; j++) {
                double temp = 0;
                // 与原矩阵相加
                for(int k = 0; k < m; k++) {
                    matrix_list[k] += matrix[j][k];
                    if(temp > 0) {
                        temp += matrix_list[k];
                    }else{
                        temp = matrix_list[k];
                        r1 = i;
                        c1 = k;
                    }
                    if(temp > maxValue) {
                        maxValue = temp;
                        r2 = j;
                        c2 = k;
                    }
                }
            }
        }
        cout<< maxValue << endl;
        return vector<int>{r1, c1, r2, c2};
    }
};


int main() {
    Solution solution;
    int _m = 2;
    int _n = 2;
    double _nums[2][2] = {1.5, -12.3,3.2,5.5};
    auto res = solution.getMaxMatrix(_nums,_m, _n);
    cout << res[0] <<" " <<res[1] <<" "<<res[2] <<" "<<res[3] <<" " <<endl;
}
```


