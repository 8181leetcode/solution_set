# LC347 前K个高频元素 (Top K Frequent Elements)

## 题目描述：
给你一个整数数组 nums 和一个整数 k ，请你返回其中出现频率前 k 高的元素。你可以按 任意顺序 返回答案。
Given an integer array nums and an integer k, return the k most frequent elements. You may return the answer in any order.

####限制
1. 1 <= nums.length <= 105
2. k is in the range [1, the number of unique elements in the array]
3. It is guaranteed that the answer is unique

##题解1-heap O(nlogk)
使用哈希表统计每个数字出现的个数，然后维护一个大小为k的小顶堆。

####代码（C++）
```cpp
vector<int> topKFrequent(vector<int>& nums, int k) {
    // 统计出现次数
    unordered_map<int, int> freq;
    for (int &x : nums) ++freq[x];

    // min heap
    priority_queue<pair<int,int>, vector<pair<int,int>>, greater<pair<int,int>>> heap;

    // 创建并维护堆，保证存有最多k个最大的数
    for (auto &it : freq) {
        heap.push({it.second, it.first});
        if (heap.size() > k) heap.pop();
    }

    // 拿出答案
    vector<int> res;
    while (k--) {
        res.push_back(heap.top().second);
        heap.pop();
    }

    return res;
}
```

## 题解2-哈希表+计数排序 O(n)
第一步用哈希表统计出所有数出现的次数。
因为所有数出现的次数都在[1, n]范围之内，可以用计数排序。
这里开一个长度为n + 1的数组，第i个位置存出现频率为i的的数的个数。
这样就可以算出前k个元素的下界。然后添加出现次数大于等于下界的数。

**时间复杂度分析**
1. 一次扫描统计每个数出现次数 O(n)
2. 计数排序 O(n)
3. 确认下界 O(n)
4. 过滤结果 O(n)

因此，总时间复杂度是 O(n)。

####代码 (C++)
```cpp
vector<int> topKFrequent(vector<int>& nums, int k) {
    // 统计出现次数
    unordered_map<int, int> freq;
    for (int &x : nums) ++freq[x];

    // 用数组记录出现次数为i的数的个数
    vector<int> count(nums.size() + 1, 0);
    for (auto &it : freq) ++count[it.second];
    
    // 找到下界
    int bound = nums.size();
    while (k) k -= count[bound--];
    
    // 加入大于下界的结果
    vector<int> res;
    for (auto &it : freq)
        if (it.second > bound)
            res.push_back(it.first);
    
    return res;
}
```
