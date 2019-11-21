# 算法刷题篇
## 动态规划算法
### 专题1：区间dp
#### 例题1：编辑距离
给定两个单词 word1 和 word2，计算出将 word1 转换成 word2 所使用的最少操作数 。
你可以对一个单词进行如下三种操作：
插入一个字符
删除一个字符
替换一个字符
```
public int minDistance(String word1, String word2) {
        int n1 = word1.length(), n2 = word2.length();
        int [][] dp = new int[n1+1][n2+1];
        for(int i=0;i<=n1;i++)
            dp[i][0] = i;
        for(int i=0;i<=n2;i++)
            dp[0][i] = i;
        for(int i=1;i<=n1;i++){
            for(int j=1;j<=n2;j++){
                if(word1.charAt(i-1)==word2.charAt(j-1))
                    dp[i][j] = dp[i-1][j-1];
                else
                    dp[i][j] = Math.min(dp[i-1][j-1],Math.min(dp[i-1][j],dp[i][j-1]))+1;
            }
        }
        return dp[n1][n2];        
    }
```
## 递归与回溯算法

### 专题1：排列问题

#### 例题1：全排列（46）

给定一个**没有重复**数字的序列，返回其所有可能的全排列。

```C++
class Solution {
public:
    vector<vector<int> > permute(vector<int> &num) {
        vector<vector<int> > res;
        permuteDFS(num, 0, res);
        return res;
    }
    void permuteDFS(vector<int> &num, int start, vector<vector<int> > &res) {
        if (start >= num.size()) res.push_back(num);
        for (int i = start; i < num.size(); ++i) {
            swap(num[start], num[i]);
            permuteDFS(num, start + 1, res);
            swap(num[start], num[i]);
        }
    }
};
```

```Java
class Solution {
    private ArrayList<List<Integer>> res;
    private boolean[]visited;
    public List<List<Integer>> permute(int[] nums) {
        res = new ArrayList<>();
        visited = new boolean[nums.length];
        dfs(nums, new ArrayList<Integer>());
        return res;
    }
    private void dfs(int[] nums,ArrayList<Integer> temp){
        if(temp.size() == nums.length){
            res.add(new ArrayList(temp));
            return;
        }
        for(int i=0;i<nums.length;i++){
            if(!visited[i]){
                visited[i] = true;
                temp.add(nums[i]);
                dfs(nums,temp);
                visited[i] = false;
                temp.remove(temp.size()-1);
            }
        }
    }
}
```

#### 例题2：全排列2（47）

给定一个可包含重复数字的序列，返回所有不重复的全排列。

```C++
class Solution {
public:
    vector<vector<int>> permuteUnique(vector<int>& nums) {
        sort(nums.begin(),nums.end());
        visited=vector<bool>(nums.size(),false);
        vector<int>vec;
        getPermutation(nums,vec);
        return res;
    }
private:
    vector<vector<int>>res;
    vector<bool>visited;
    void getPermutation(vector<int>&nums,vector<int>&vec){
        if(vec.size()==nums.size()){
            res.push_back(vec);
            return;
        }
        for(int i=0;i<nums.size();i++){
            if(!visited[i]){
                if(i>0&&nums[i]==nums[i-1]&&!visited[i-1])
                    continue;
                visited[i]=true;
                vec.push_back(nums[i]);
                getPermutation(nums,vec);
                visited[i]=false;
                vec.pop_back();
            }
        }
        return;
    }
};
```

```Java
class Solution {
    private ArrayList<List<Integer>> res;
    private boolean []used;
    public List<List<Integer>> permuteUnique(int[] nums) {
        Arrays.sort(nums);
        res = new ArrayList<>();
        used = new boolean[nums.length];
        dfs(nums, new ArrayList<Integer>());
        return res;
    }
    public void dfs(int []nums, ArrayList<Integer>temp){
        if(temp.size() == nums.length){
            res.add(new ArrayList<>(temp));
            return;
        }
        for(int i=0;i<nums.length;i++){
            if(!used[i]){
                if(i>0 && nums[i]==nums[i-1]&&!used[i-1])
                    continue;
                used[i] = true;
                temp.add(nums[i]);
                dfs(nums, temp);
                used[i] = false;
                temp.remove(temp.size()-1);
            }
        }
    }
}
```
## 分治算法
### 专题1：归并排序
#### 例题1：翻转对(493)
给定一个数组 nums ，如果 i < j 且 nums[i] > 2*nums[j] 我们就将 (i, j) 称作一个重要翻转对。
你需要返回给定数组中的重要翻转对的数量。
```
class Solution {
    private int []temp;
    public int reversePairs(int[] nums) {
        int n = nums.length;
        temp = new int[n];
        return mergeCount(nums, 0, n-1);
    }
    private int mergeCount(int []nums, int begin, int end){
        if(begin >= end)
            return 0;
        int mid = begin + end >> 1;
        int leftCnt = mergeCount(nums, begin, mid);
        int rightCnt = mergeCount(nums, mid+1, end);
        int midCnt = 0;
        for(int i = begin; i <= end; i++)
            temp[i] = nums[i];    
        int i = begin, j = mid+1;
        while(i <= mid && j <= end){
            if((long)nums[i] > (long)2 * nums[j]){
                midCnt += mid - i + 1;
                j++;
            }else{
                i++;
            }
        }
        i = begin;
        j = mid+1;
        for(int k = begin; k <= end; k++){
            if(i > mid){
                nums[k] = temp[j++];
            }else if(j > end){
                nums[k] = temp[i++];
            }else if(temp[i] < temp[j]){
                nums[k] = temp[i++];
            }else{
                nums[k] = temp[j++];
            }
        }
        return leftCnt + rightCnt + midCnt;
    }
}
```
#### 例题2：计算右侧小于当前元素的个数（315）
给定一个整数数组 nums，按要求返回一个新数组 counts。数组 counts 有该性质： counts[i] 的值是  nums[i] 右侧小于 nums[i] 的元素的数量。
```
class Solution {
    private int index[];
    private int temp[];
    private int counter[];
    public List<Integer> countSmaller(int[] nums) {
        List<Integer> res = new ArrayList<>();
        int len = nums.length;
        if (len == 0) {
            return res;
        }
        temp = new int[len];
        counter = new int[len];
        index = new int[len];
        for (int i = 0; i < len; i++) {
            index[i] = i;
        }
        mergeAndCountSmaller(nums, 0, len - 1);
        for (int i = 0; i < len; i++) {
            res.add(counter[i]);
        }
        return res;
    }
    private void mergeAndCountSmaller(int[] nums, int begin, int end) {
        if (begin >= end)
            return;
        int mid = begin + end >> 1;
        mergeAndCountSmaller(nums, begin, mid);
        mergeAndCountSmaller(nums, mid + 1, end);
        if (nums[index[mid]] > nums[index[mid + 1]]) {
            merge(nums, begin, mid, end);
        }
    }

    private void merge(int[] nums, int begin, int mid, int end) {
        for (int i = begin; i <= end; i++) {
            temp[i] = index[i];
        }
        int i = begin, j = mid + 1;
        for (int k = begin; k <= end; k++) {
            if (i > mid){
                index[k] = temp[j++];
            }else if(j > end){
                index[k] = temp[i++];
                counter[index[k]] += end - mid;
            }else if (nums[temp[i]] <= nums[temp[j]]){
                index[k] = temp[i++];
                counter[index[k]] += j - mid - 1;
            }else{
                index[k] = temp[j++];
            }
        }
    }
}
```

