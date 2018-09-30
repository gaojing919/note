# easy #
----------
## 448. Find All Numbers Disappeared in an Array ##
 
题目链接：[https://leetcode.com/problems/find-all-numbers-disappeared-in-an-array/description/](https://leetcode.com/problems/find-all-numbers-disappeared-in-an-array/description/)  
别人的方法：[http://www.cnblogs.com/grandyang/p/6222149.html](http://www.cnblogs.com/grandyang/p/6222149.html)

注意的问题：
iter = result.erase(iter); erase（）函数执行完了以后会把iter指向下一个元素


**我的代码**  

    class Solution {
    public:
    	vector<int> findDisappearedNumbers(vector<int>& nums) {
    		int n = nums.size();
    		vector<int> result(n);
    		for (int i = 0; i < n; i++){
    			result[i] = i + 1;
    		}
    		//auto b = result.begin(),iter = b;
    		for (int j = 0; j < n; j++){
    			if (nums[j]>0&&nums[j]<=n){
    				result[nums[j]-1] = 0;
    			}
    		}
    		for (auto iter = result.begin(); iter < result.end();){
    			if (*iter == 0){
    				iter = result.erase(iter);\\erase（）函数执行完了以后会把iter指向下一个元素
    			}
    			else{
    				iter++; }
    			
    		}
    		return result;
    	}
    };
    
    int main(){
    
    	
    	Solution s = Solution();
    	vector<int> b = { 4, 3, 2, 7, 8, 2, 3, 1 };
    	vector<int> r = s.findDisappearedNumbers(b);
    	for (auto x : r){
    		cout << x << " ";
    
    	}
    	system("PAUSE");
    	return 0;
    }

我的方法的缺点是：  需要创建一个n这么大的数组，还要对数组进行初始化，每次从数组中删除元素消耗太大

**比较好的方法**  

    class Solution {
    public:
    vector<int> findDisappearedNumbers(vector<int>& nums) {
    vector<int> res;
    for (int i = 0; i < nums.size(); ++i) {
    int idx = abs(nums[i]) - 1;
    nums[idx] = (nums[idx] > 0) ? -nums[idx] : nums[idx];
    }
    for (int i = 0; i < nums.size(); ++i) {
    if (nums[i] > 0) {
    res.push_back(i + 1);
    }
    }
    return res;
    }
    }
   


----------
## 717. 1-bit and 2-bit Characters ##
题目链接：[https://leetcode.com/problems/1-bit-and-2-bit-characters/description/](https://leetcode.com/problems/1-bit-and-2-bit-characters/description/)  
我的代码：  

    class Solution {
    public:
    	bool isOneBitCharacter(vector<int>& bits) {
    		int n = bits.size();
    		if (n < 1){
    			cout << "input error";
    		}
    		if (bits[n - 1] == 1){
    			return false;
    		}
    		for (int i = 0; i < n;){
    			if (bits[i] == 1){
    				i = i + 2;
    			}
    			else{
    				if (i == (n - 1)){
    					return true;
    				}
    				i = i + 1;
    			}
    		}
    		return false;
    
    	}
    };
    int main(){
    
    	
    	Solution s = Solution();
    	vector<int> b = { 1,1,1,0 };
    	cout<<s.isOneBitCharacter(b);
    	system("PAUSE");
    	return 0;
    }

----------
## 169. Majority Element ##
题目链接：[https://leetcode.com/problems/majority-element/description/](https://leetcode.com/problems/majority-element/description/)    
别人的代码：[http://www.cnblogs.com/grandyang/p/4233501.html](http://www.cnblogs.com/grandyang/p/4233501.html)  
**我的代码**  

    class Solution {
    public:
    	int isappear(vector<int>& temp, int x){
    		for (int i = 0; i < temp.size();i++){
    			if (temp[i] == x){
    				return i;
    			}
    		}
    		return -1;
    	}
    	int majorityElement(vector<int>& nums) {
    		int n = (nums.size()) / 2,indx;
    		vector<int> temp;
    		vector<int> count;
    		for (int i = 0; i < nums.size(); i++){
    			indx = isappear(temp, nums[i]);			
    			if (indx>=0){
    				count[indx] += 1;
    			}
    			else{
    				temp.push_back(nums[i]);
    				count.push_back(1);
    			}
    		}
    		for (int i = 0; i < count.size(); i++){
    			if (count[i]>n){
    				return temp[i];
    			}
    		}
    		return 0;
    
    	}
    };
    int main(){
    
    	
    	Solution s = Solution();
    	vector<int> b = { 3,2,3 };
    	cout<<s.majorityElement(b);
    	system("PAUSE");
    	return 0;
    }
    
  

我的方法的缺点：需要O(n)时间和O(n)的空间，需呀新建数组  
**好的方法**  

    class Solution {
    public:
    int majorityElement(vector<int>& nums) {
    int res = 0, cnt = 0;
    for (int num : nums) {
    if (cnt == 0) {res = num; ++cnt;}
    else (num == res) ? ++cnt : --cnt;
    }
    return res;
    }
    };  

----------
## 122. Best Time to Buy and Sell Stock II ##
题目链接：[https://leetcode.com/problems/best-time-to-buy-and-sell-stock-ii/description/](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-ii/description/)  
**我的代码**   

    class Solution {
    public:
    	int maxProfit(vector<int>& prices) {
    		int buyPrice;
    		int profit=0;
    		bool haveStock=false;
    		for (int p =0; p<(prices.size()); p++){
    			if (p>0 && p < (prices.size() - 1) && prices[p] <= prices[p - 1] && prices[p] < prices[p + 1]){
    			//buy
    				profit -= prices[p];
    				buyPrice = prices[p];
    				haveStock = true;
    				continue;
    			}
    			if (p>0 && p < (prices.size() - 1)&&haveStock&&prices[p]>buyPrice&& prices[p] > prices[p - 1] && prices[p] >= prices[p + 1]){
    			//sell
    				profit += prices[p];
    				haveStock = false;
    				continue;
    			}
    			if (p ==0){
    				if (!haveStock&&p < (prices.size() - 1) && prices[p] <= prices[p + 1]){
    				//buy
    					profit -= prices[p];
    					buyPrice = prices[p];
    					haveStock = true;
    				}
    				continue;
    			}
    			if (haveStock&&p == (prices.size() - 1)){
    				profit += prices[p];
    				haveStock = false;
    				continue;
    			}
    		}
    		return profit;
    
    	}
    };
    
**注意的问题**：  
买入时小于等于前一天的价格，不需要严格小于。 卖出时，大于等于后一天的价格，不需要严格大于


----------
## 217. Contains Duplicate ##
问题连接：[https://leetcode.com/problems/contains-duplicate/description/](https://leetcode.com/problems/contains-duplicate/description/)  
**我的代码**  

    class Solution {
    public:
    	bool containsDuplicate(vector<int>& nums) {
    if (nums.size() == 0){
    			return false;
    		}
    		sort(nums.begin(), nums.end());
    		for (auto i = nums.begin(); i < (nums.end()-1); i++){
    			if (*i == *(i + 1)){
    				return true;
    			}
    	
    		}
    		return false;
    
    	}
    };

**注意的问题**  
注意对空数组的处理  
不能直接比较，这样的话时间复杂度是$$O(n^2)$$,先排序再比较，只耗费排序的时间：$$O(nlgn)$$

## 167. Two Sum II - Input array is sorted ##
题目链接：[https://leetcode.com/problems/two-sum-ii-input-array-is-sorted/description/](https://leetcode.com/problems/two-sum-ii-input-array-is-sorted/description/)  
**我的代码**  

    class Solution {
    public:
    	vector<int> twoSum(vector<int>& numbers, int target) {
    		//int first, second;
    		int sub;
    		vector<int> r;
    		for (int i = 0; i < numbers.size(); i++){
    				sub = target - numbers[i];
    				for (int j = i + 1; j < numbers.size(); j++){
    					if (numbers[j] == sub){
    						r = { i+1, j+1 };
    						return r;
    					}
    					if (numbers[j]>sub){
    						break;
    					}
    				}
    		}
    return r;
    
    	}
    };  
  
**注意的问题：**  
数据中可能会存在负数  

----------
## 697. Degree of an Array ##
问题连接：[https://leetcode.com/problems/degree-of-an-array/description/](https://leetcode.com/problems/degree-of-an-array/description/)  
**我的代码**  

    class Solution {
    public:
    	int findShortestSubArray(vector<int>& nums) {
    		int s = nums.size();
    		int d = 0;
    		int maxd = 0, r = 0;
    		int e;
    		//vector<int> result;
    		if (s<2){
    			return s;
    		}
    		for (int i = 0; i < (s - 1); i++){
    			if (nums[i]>=0){
    				d = 1;
    				e = i;
    				for (int j = i + 1; j < s; j++){
    					if (nums[i] == nums[j]){
    						d += 1;
    						nums[j] = -nums[j];
    						e = j;
    					}
    				}
    				if (d > maxd){
    					maxd = d;
    					r = e - i + 1;
    
    				}
    				if (d == maxd){
    					if ((e - i + 1) < r){
    						maxd = d;
    						r = e - i + 1;
    					}
    				}
    
    
    			}
    
    		}
    		return r;
    	}
    };
 
**注意的问题**  
注意数组中可能存在0，可能存在两个数的出现次数都是最大的，要选产生的子数组最短的  
我的时间是$$O(n^2)$$，空间是$O(1)$，存在时间和空间都是：$O(n)$的方法  ，需要使用到关联容器map,代码现在还看不懂  

**别人的O(n)时间复杂度的方法：**  

    class Solution {
    public:
    int findShortestSubArray(vector<int> &nums) {
    unordered_map<int, int> counts;
    unordered_map<int, int> starts;
    unordered_map<int, int> ends;
    int maxCount = INT_MIN;
    for (int i = 0; i < nums.size(); i++) {
    int num = nums[i];
    if (starts.count(num) == 0) {
    starts[num] = i;
    counts[num] = 0;
    }
    counts[num]++;
    ends[num] = i;
    maxCount = max(maxCount, counts[num]);
    }
    int result = INT_MAX;
    for (auto &item : counts) {
    if (item.second == maxCount) {
    result = min(result, ends[item.first] - starts[item.first] + 1);
    }
    }
    return result;
    }
    };


----------
## 661. Image Smoother ##
问题链接：[https://leetcode.com/problems/image-smoother/description/](https://leetcode.com/problems/image-smoother/description/)  
**我的代码：**  

    class Solution {
    public:
    	vector<vector<int>> imageSmoother(vector<vector<int>>& M) {
    		int m = M.size(), n = M[0].size();
    		int c = 0;
    		vector < vector < int >> r(m,vector<int>(n));
    		//计算积分图
    		for (int i = 0; i < m; i++){
    			for (int j = 0; j < n; j++){
    				
    				if (i > 0){
    					M[i][j] += M[i - 1][j];
    				}
    				if (j > 0){
    					M[i][j] += M[i][j - 1];
    				}
    				if (i > 0 && j > 0){
    					M[i][j] -= M[i - 1][j - 1];
    				}
    				
    			}
    		}
    		int indi, indj;
    		for (int i = 0; i < m; i++){
    			for (int j = 0; j < n; j++){
    				c = 1;
    				indi = i; indj = j;
    				if (i > 0){
    					c += 1;
    					if (j > 0){
    						c += 1;
    					}
    					if (j < (n - 1)){
    						c += 1;
    					}
    				}
    				if (j > 0){
    					c += 1;
    				}
    				if (i < (m - 1)){
    					indi = i + 1;
    					c += 1;
    					if (j > 0){
    						c += 1;
    					}if (j < (n - 1)){
    						c += 1;
    					}
    				}
    				if (j < (n - 1)){
    					indj = j + 1;
    					c += 1;
    				}
    				r[i][j] = M[indi][indj];
    				if (i > 1){
    					r[i][j] -= M[i - 2][indj];
    					if (j>1){
    						r[i][j] -= M[indi][j-2];
    						r[i][j] += M[i-2][j - 2];
    					}
    				}
    				else{
    					if (j > 1){
    						r[i][j] -= M[indi][j - 2];
    					}
    				
    				}
    				r[i][j] = int(r[i][j] / c);
    		
    			}
    		}
    
    		return r;
    	}
    };


**需要注意的问题**  
根据积分图算平均值时比较困难  
直接算的方法时间更短   
**别人的代码**  

    class Solution
    {
    public:
    vector<vector<int>> imageSmoother(vector<vector<int>>& M)
    {
    int rows = M.size();
    int cols = M[0].size();
    vector<vector<int>> result(rows,vector<int>(cols, 0));
    for(int i = 0; i < rows; ++i)
    {
    for(int j = 0; j < cols; ++j)
    {
    int count = 1;
    int sum = M[i][j];
    
    if(i - 1 >= 0)
    {
    sum += M[i-1][j];
    ++count;
    if(j - 1 >= 0)
    {
    sum += M[i-1][j-1];
    ++count;
    }
    if(j + 1 < cols)
    {
    sum += M[i-1][j+1];
    ++count;
    }
    }
    if(i + 1 < rows)
    {
    sum += M[i+1][j];
    ++count;
    if(j - 1 >= 0)
    {
    sum += M[i+1][j-1];
    ++count;
    }
    if(j + 1 < cols)
    {
    sum += M[i+1][j+1];
    ++count;
    }
    }
    if(j - 1 >= 0)
    {
    sum += M[i][j-1];
    ++count;
    }
    if(j + 1 < cols)
    {
    sum += M[i][j+1];
    ++count;
    }
    result[i][j] = sum / count;
    }
    }
    return result;
    }
    };

----------
## 268. Missing Number ##
题目链接：[https://leetcode.com/problems/missing-number/description/](https://leetcode.com/problems/missing-number/description/)  
**我的代码**  

    class Solution {
    public:
    	int missingNumber(vector<int>& nums) {
    		int s = nums.size();
    		int temp=nums[0];
    		int indx = 0;
    		for (auto x : nums){
    			 if (abs(x) < s && nums[abs(x)]>=0){
    				if (nums[abs(x)] == 0){
    					nums[abs(x)] = -(s + 1);
    				}
    				else{
    					nums[abs(x)] = -nums[abs(x)];
    				}
    				
    			}
    			 if (x == -(s + 1)){
    				 nums[0] = -nums[0];
    			 }
    		}
    		int i = 0;
    		while (i<s&&nums[i]<0){
    			i++;
    		}
    		return i;
    	}
    };

**注意的问题：**
注意对0的处理  
**别人的方法**：直接求和，看缺少几  

    class Solution {
    public:
    int missingNumber(vector<int>& nums) {
    if(!nums.size()) return 0;
    int sum =0;
    for(int each : nums) sum += each;
    return nums.size()*(1 + nums.size())/2 - sum;
    }
    };

----------
## 628. Maximum Product of Three Numbers ##
题目链接：[https://leetcode.com/problems/maximum-product-of-three-numbers/description/](https://leetcode.com/problems/maximum-product-of-three-numbers/description/)  
**我的方法**

    class Solution {
    public:
    	int maximumProduct(vector<int>& nums) {
    		vector<int> positive, negative;
    		bool zero;
    		int s, ps, ns;
    		int result=0;
    		int temp1, temp2,temp3;
    		for (auto x : nums){
    			if (x > 0){
    				positive.push_back(x);
    			}
    			else if(x<0){
    				negative.push_back(x);
    			
    			}
    		}
    		s = nums.size();
    		ps = positive.size();
    		ns = negative.size();
    		sort(negative.begin(), negative.end());
    		sort(positive.begin(), positive.end());
    		if ((ps + ns) != s && (ps < 3 && ns < 2)){
    			return 0;
    
    		}
    		if (ps == 0){
    				result = (*(negative.end() - 1))*(*(negative.end() - 2))*(*(negative.end() - 3));
    				return result;
    		}
    		else{
    			if (ns>1){
    				temp1 = (*(positive.end() - 1))*(*(negative.begin()))*(*(negative.begin()+1));
    			}
    			if (ps>2){
    				temp2 = (*(positive.end() - 1))*(*(positive.end() - 2))*(*(positive.end() - 3));
    			}
    			result = max(temp1, temp2);
    		}
    		return result;
    	}
    };

**注意问题**
注意对变量的初始化，注意对0的处理  

**别人的方法**  


    class Solution {
    public:
    int maximumProduct(vector<int>& nums) {
    int max1 = INT_MIN, max2 = INT_MIN, max3 = INT_MIN;
    int min1 = INT_MAX, min2 = INT_MAX;
    
    for (auto &i: nums) {
    if (i > max1) {
    max3 = max2, max2 = max1, max1 = i;
    } else if (i > max2) {
    max3 = max2, max2 = i;
    } else if (i > max3) {
    max3 = i;
    }
    
    if (i < min1) {
    min2 = min1, min1 = i;
    } else if (i < min2) {
    min2 = i;
    }
    }
    
    return max(max1*max2*max3, max1*min1*min2);
    }
    };


----------
## 746. Min Cost Climbing Stairs ##
题目链接：[https://leetcode.com/problems/min-cost-climbing-stairs/description/](https://leetcode.com/problems/min-cost-climbing-stairs/description/)  
**我的方法：**  

    class Solution {
    public:
    	int minCostClimbingStairs(vector<int>& cost) {
    		cost.push_back(0);
    		int s = cost.size();
    		vector<vector<int>> result(2,vector < int > (s));
    		//int result0 = cost[0], result1 = cost[1];
    		//int step1, step2;
    	
    		for (int i = 0; i < 2; i++){
    			for (int j = i; j < s; j++){
    				if (i == j){
    					result[i][j] = cost[i];
    				}
    				else if ((j - i) == 1){
    					result[i][j] = result[i][i] + cost[j];
    				}
    				else{
    					result[i][j] = min(result[i][j - 1] + cost[j], result[i][j - 2] + cost[j]);
    				}
    			}
    		}
    		cout << result[0][s - 1] << "  " << result[1][s - 1];
    		return min(result[0][s - 1], result[1][s - 1]);
    
    	}
    };
**别人的方法**  

    class Solution {
    public:
    int minCostClimbingStairs(vector<int>& cost) {
    vector<int> minCostVec;
    int size = cost.size();
    //cost.push_back(0);
    minCostVec.push_back(cost[0]);
    minCostVec.push_back(cost[1]);
    for(int i = 2; i < size; i++)
    {
    int minCost1 = cost[i] + minCostVec[i - 1];
    int minCost2 = cost[i] + minCostVec[i - 2];
    int minCost = minCost1 > minCost2 ? minCost2 : minCost1;
    minCostVec.push_back(minCost);
    }
    
    return minCostVec[size - 1] > minCostVec[size - 2] ?  minCostVec[size - 2]: minCostVec[size - 1];
    
    
    }
    
    };
    


----------
## 121. Best Time to Buy and Sell Stock ##
题目链接：[https://leetcode.com/problems/best-time-to-buy-and-sell-stock/description/](https://leetcode.com/problems/best-time-to-buy-and-sell-stock/description/)  
**我的方法**  ：需要$O(n^2)$时间

    class Solution {
    public:
    	int maxProfit(vector<int>& prices) {
    		int s = prices.size();
    		int profit = 0;
    		if (s < 2){
    			return 0;
    		}
    		for (int i = 0; i < s; i++){
    			for (int j = i; j < s; j++){
    				if ((prices[j] - prices[i]) > profit){
    					profit = prices[j] - prices[i];
    				}
    			}
    		}
    		return profit;
    	}
    };
**别人的方法** ：只需要O(n)的时间
 
    class Solution {
    public:
    int maxProfit(vector<int>& prices) {
    if ( prices.size() == 0)
    return 0;
    int Suffix[prices.size()];
    Suffix[prices.size()-1] = prices[prices.size()-1];
    for ( int i = prices.size()-2; i >= 0; i--)
    Suffix[i] = max(Suffix[i+1], prices[i]);
    int ans = 0;
    for ( int i = 0; i < prices.size() - 1; i++) {
    	if ( prices[i] < Suffix[i+1])
    ans = max(ans, Suffix[i+1] - prices[i]);
    }   
    return ans;
    }
    }; 
    
  

----------
## 674. Longest Continuous Increasing Subsequence ##
题目链接；[https://leetcode.com/problems/longest-continuous-increasing-subsequence/description/](https://leetcode.com/problems/longest-continuous-increasing-subsequence/description/)  
**我的代码**

    class Solution {
    public:
    	int findLengthOfLCIS(vector<int>& nums) {
    		int temp =1;
    		int max = 1;
    		if (nums.size() == 0){
    			return 0;
    		}
    		for (int i = 1; i < nums.size(); i++){
    			if (nums[i]>nums[i - 1]){
    				temp += 1;
    			}
    			else{
    				if (temp > max){
    					max = temp;					
    				}
    				temp = 1;
    			}
    		}
    		if (temp > max){
    			max = temp;
    		}
    		return max;
    
    	}
    };

----------
## 747. Largest Number At Least Twice of Others ##
题目链接：[https://leetcode.com/problems/largest-number-at-least-twice-of-others/description/](https://leetcode.com/problems/largest-number-at-least-twice-of-others/description/)  
**我的代码**：O(n)时间

    class Solution {
    public:
    	int dominantIndex(vector<int>& nums) {
    		int s = nums.size();
    		int max1=-1,max2=-1;
    		int index1=-1,index2=-1,x;
    		if (s < 2){
    			return 0;
    		}
    		for (int i = 0; i < s;i++){
    			x = nums[i];
    			if (x > max1){
    				max2 = max1;
    				max1 = x;
    				index2 = index1;
    				index1 = i;
    			}
    			else{
    				if (x > max2){
    					max2 = x;
    					index2 = i;
    				}
    			}
    		}
    		if (max1 >= (2 * max2)){
    			return index1;
    		}
    		return -1;
    
    	}
    };

----------
## 27. Remove Element ##
题目链接：[https://leetcode.com/problems/remove-element/description/](https://leetcode.com/problems/remove-element/description/)  
**我的代码**

    class Solution {
    public:
    	int removeElement(vector<int>& nums, int val) {
    		int s = nums.size(),length = -1, temp;
    		if (s < 1){
    			return 0;
    		}
    		for (int j = s-1; j >= 0; j--){
    			if (nums[j] != val){
    				length = j;
    				break;
    			}
    		}
    		for (int i = length; i >=0&&length>=-1&&i<=length;){
    			if (nums[i] == val){
    			//丢弃
    				nums[i] = nums[length];
    				length -= 1;
    			}
    			else{
    				i -= 1;
    			}
    		}
    
    		return length+1;
    
    
    	}
    };
  

----------
## 832. Flipping an Image   ##
题目链接：[https://leetcode.com/problems/flipping-an-image/description/](https://leetcode.com/problems/flipping-an-image/description/)  
**我的代码**

    class Solution {
    public:
    	vector<vector<int>> flipAndInvertImage(vector<vector<int>>& A) {
    		int m = A.size(),n=A[0].size();
    		vector<vector<int>> result( m, vector < int > (n) );
    		for (int i = 0; i < m; i++){
    			for (int j = 0; j < n; j++){
    				result[i][j] = A[i][n - 1 - j] ? 0 : 1;
    			}
    		}
    		return result;
    	
    
    	}
    };


----------
# middle #
## 861. Score After Flipping Matrix ##
题目链接：[https://leetcode.com/problems/score-after-flipping-matrix/description/](https://leetcode.com/problems/score-after-flipping-matrix/description/)
**我的代码**  

    #include<vector>
    #include<iostream>
    #include <algorithm>
    #include<string>
    using namespace std;
    
    class Solution {
    public:
    	int matrixScore(vector<vector<int>>& A) {
    		int m = A.size(), n = A[0].size();
    		vector<bool> exit_one(m, false);
    		if (m == 0 || n == 0){
    			return 0;
    		}
    		int nums_zero = 0, nums_one = 0;
    
    		for (int clo = 0; clo < n; clo++){
    			nums_zero = 0, nums_one = 0;
    			for (int row = 0; row < m; row++){
    				if (A[row][clo] == 0){
    					nums_zero += 1;
    				}
    				else{
    					nums_one += 1;
    				}
    			}
    			if (nums_zero > nums_one){
    			//change this clo
    				for (int row = 0; row < m; row++){
    					if (A[row][clo] == 1){
    						A[row][clo] = 0;
    					}
    					else{
    						A[row][clo] = 1;
    					}
    				}
    
    			}
    			//change 0 row
    			for (int row = 0; row < m; row++){
    				if (A[row][clo] == 0 && !exit_one[row]){
    				//chang this row,change exit_one
    					for (int j = 0; j < n; j++){
    						if (A[row][j] == 0){
    							A[row][j] = 1;
    							exit_one[row] = true;
    						}
    						else{
    							A[row][j] = 0;
    						}
    					}
    
    				}
    				else if (A[row][clo] == 1 && !exit_one[row]){
    				//change exit_one
    					exit_one[row] = true;
    				}
    			}
    		}
    		int result = 0;
    		for (auto iter = A.begin(); iter != A.end(); iter++){
    			for (int i = 0; i < n;i++){
    				if ((*iter)[i] == 1){
    					result += pow(2,(n-1-i));
    				}
    			}
    		}
    		return result;
    	}
    };


----------
## 814. Binary Tree Pruning ##
题目链接：[https://leetcode.com/problems/binary-tree-pruning/description/](https://leetcode.com/problems/binary-tree-pruning/description/)   
**我的代码**

    /**
     * Definition for a binary tree node.
     * struct TreeNode {
     * int val;
     * TreeNode *left;
     * TreeNode *right;
     * TreeNode(int x) : val(x), left(NULL), right(NULL) {}
     * };
     */
    class Solution {
    public:
    	TreeNode* pruneTree(TreeNode* root) {
    		if (root->left!=NULL){
    			root->left = pruneTree(root->left);
    		}
    		if (root->right != NULL){
    			root->right = pruneTree(root->right);
    		}
    		if (root->val == 0 && root->left == NULL && root->right == NULL){
    			root = NULL;
    		}
    		return root;
    	}
    };


----------
## 763. Partition Labels ##
题目链接：[https://leetcode.com/problems/partition-labels/description/](https://leetcode.com/problems/partition-labels/description/)

**我的代码**

    class Solution {
    public:
    	vector<int> partitionLabels(string S) {
    		vector<int> result;
    		int m = S.size();
    		int j, end, temp;
    		for (int i = 0; i < m;){
    			if (i == (m - 1)){
    				result.push_back(1);
    				break;
    			}
    			j = i + 1;
    			//cout << S.substr(i + 1, m - i - 1);
    			end = i+1+(S.substr(i+1,m-i-1)).find(S[i]);
    			if (end != S.npos){
    				for (; j <= end; j++){
    					temp = j+1+(S.substr(j + 1, m - j-1)).find(S[j]);
    					if (temp > end){
    						end = temp;
    					}
    				}
    				result.push_back(end - i + 1);
    				i = end + 1;
    				
    			}
    			else{
    				i = i + 1;
    				result.push_back(1);
    			}
    			
    			   
    		}
    		return result;
    
    	}
    };


----------
## 537. Complex Number Multiplication ##
题目链接：[https://leetcode.com/problems/complex-number-multiplication/description/](https://leetcode.com/problems/complex-number-multiplication/description/)  

**我的代码**  

    class Solution {
    public:
    	string complexNumberMultiply(string a, string b) {
    		int a_index = a.find("+"), b_index = b.find("+");
    		int i_a=0, i_b=0, i_c=0, i_d=0;
    		int i=0,j;
    		bool exit_c = false;
    		i = 0;
    		if ((a.substr(0, a_index).find('-')) != a.npos){
    			i = 1;
    			exit_c = true;
    
    		}
    		for (; i < a_index; i++){
    			i_a += int(a[i] - '0')*pow(10, (a_index - 1-i));
    		}
    		if (exit_c){		
    			i_a = -i_a;
    			exit_c = false;
    		}
    		i = 0;
    		if ((b.substr(0, b_index).find('-')) != b.npos){
    			i = 1;
    			exit_c = true;
    
    		}		
    		for (; i < b_index; i++){
    			i_c += int(b[i] - '0')*pow(10, (b_index - 1 - i));
    		}
    		if (exit_c){
    			i_c = -i_c;
    			exit_c = false;
    		}
    		j = a_index + 1;
    		if ((a.substr(a_index+1, a.size()-1-a_index).find('-')) != a.npos){
    			j = a_index + 2;
    			exit_c = true;
    
    		}
    		for (; j < a.size() - 1; j++){
    			i_b += int(a[j] - '0')*pow(10, a.size() - 2 - j);
    		}
    		if (exit_c){
    			i_b = -i_b;
    			exit_c = false;
    		}
    		j = b_index + 1;
    		if ((b.substr(b_index + 1, b.size() - 1 - b_index).find("-")) != b.npos){
    			j = b_index + 2;
    			exit_c = true;
    		}
    		for (; j < b.size() - 1; j++){
    			i_d += int(b[j] - '0')*pow(10, b.size() - 2 - j);
    		}
    		if (exit_c){
    			i_d = -i_d;
    			exit_c = false;
    		}
    		string r = to_string(i_a*i_c - i_b*i_d) + "+" + to_string(i_a*i_d + i_b*i_c) + "i";
    		return r;
    
    	}
    };


----------
## 845. Longest Mountain in Array ##
题目链接：[https://leetcode.com/problems/longest-mountain-in-array/description/](https://leetcode.com/problems/longest-mountain-in-array/description/)


思路：  
用isup表示是否在上升，如果一直上升就一直加1，如果上升到了最后一个或者遇到相等的了就跳到下一个重新开始。否则等到上升完毕，记录看是否一直在下降，下降就加一，注意山的长度是要比上升次数和下降次数多1的，所以在开始的时候就给templen加一了。

    class Solution {
    public:
    	int longestMountain(vector<int>& A) {
    		int s = A.size();
    		if (s < 3){
    			return 0;
    		}
    		bool isup = false;
    		int maxlen=0,templen=0;
    		for (int i = 1; i < s; ){
    			templen = 0;
    			if (A[i]>A[i - 1]){
    				isup = true;
    				templen = 1;
    			}
    			while (isup&&i<s&&A[i]>A[i - 1]){
    				//isup = true;
    				templen += 1;
    				i++;
    			}
    			isup = false;
    			if (i == s || A[i] == A[i - 1]){
    				i++;
    				continue;
    			}
    			while (templen&&i<s&&A[i] < A[i - 1]){
    				templen += 1;
    				i++;
    			}
    			if (templen == 0){
    				i++;
    			}
    			if (templen > maxlen){
    				maxlen = templen;
    			}
    		}
    		return maxlen;
    	}
    };



----------
## 754. Reach a Number-拼多多二维生物旅行问题 ##
题目链接：	[https://leetcode.com/problems/reach-a-number/description/](https://leetcode.com/problems/reach-a-number/description/)

解题思路：根据数学原理找到一个最多需要的步数和最少需要的步数，从最少需要的步数开始查看能否到达，如果能到达就返回，否则加一，继续判断。  
端端能否到达的方法是：能到的最左和最右很容易算出，能够到达的位置列表是以2共差的等差数列，所以只需要判断(target - templ) % 2 ==  0 就可以了。

    class Solution {
    public:
    	int reachNumber(int target) {
    		int maxm = 2 * abs(target) - 1;//最多需要的步数
    		int minm = ceil(-0.5 + sqrt(2 * abs(target) + 0.25));//最少需要的步数
    		int templ, tempr;//保存minm能够到达的最左和最右的位置
    		while (minm <= maxm){
    			tempr = (minm*(minm + 1)) / 2;
    			templ = -tempr;
    			if (target >= templ&&target <= tempr && (target - templ) % 2 == 0){
    				return minm;
    			}
    			else{
    				minm += 1;
    			}
    
    		}
    		return minm;
    
    	}
    };


----------
## 网易测试 ##
题目：小W有一个电子时钟用于显示时间，显示的格式为HH:MM:SS，HH，MM，SS分别表示时，分，秒。其中时的范围为[‘00’,‘01’…‘23’]，分的范围为[‘00’,‘01’…‘59’]，秒的范围为[‘00’,‘01’…‘59’]。

但是有一天小W发现钟表似乎坏了，显示了一个不可能存在的时间“98:23:00”，小W希望改变最少的数字，使得电子时钟显示的时间为一个真实存在的时间，譬如“98:23:00”通过修改第一个’9’为’1’，即可成为一个真实存在的时间“18:23:00”。修改的方法可能有很多，小W想知道，在满足改变最少的数字的前提下，符合条件的字典序最小的时间是多少。其中字典序比较为用“HHMMSS”的6位字符串进行比较。


输入描述:
每个输入数据包含多个测试点。每个测试点后有一个空行。 第一行为测试点的个数T(T<=100)。 每个测试点包含1行，为一个字符串”HH:MM:SS”，表示钟表显示的时间。


输出描述:
对于每个测试点，输出一行。如果钟表显示的时间为真实存在的时间，则不做改动输出该时间，否则输出一个新的”HH:MM:SS”，表示修改最少的数字情况下，字典序最小的真实存在的时间。

输入例子1:
2
19:90:23
23:59:59

输出例子1:
19:00:23
23:59:59

    #include <iostream>
    #include<vector>
    #include<algorithm>
    #include<string>
    using namespace std;
    
    
    void split(const std::string &s, char delimiter, std::vector<std::string> &v)
    {
    	std::string::size_type i = 0;
    	std::string::size_type j = s.find(delimiter);
    	while (j != std::string::npos)
    	{
    		v.push_back(s.substr(i, j - i));
    		i = ++j;
    		j = s.find(delimiter, i);
    	}
    	if (j == std::string::npos)
    		v.push_back(s.substr(i, s.length()));
    }
    
    
    void correcttime(int n,vector<string> & times){
    	vector<string> temptime;
    	int hh, mm, ss;
    	string temp;
    	for (int i = 0; i < n; i++){
    		temp = "";
    		temptime = {};
    		split(times[i], ':', temptime);
    		hh = stoi(temptime[0]);
    		mm = stoi(temptime[1]);
    		ss = stoi(temptime[2]);
    		if (hh >= 24){
    			temp += "0";
    			temp += temptime[0][1];
    		}
    		else{
    			temp += temptime[0];
    		}
    		temp += ":";
    		if (mm >= 60){
    			temp += "0";
    			temp += temptime[1][1];
    		}
    		else{
    			temp += temptime[1];
    		}
    		temp += ":";
    		if (ss >= 60){
    			temp += "0";
    			temp += temptime[2][1];
    		}
    		else{
    			temp += temptime[2];
    		}
    		times[i] = temp;	
    		
    	}
    
    }
    int main()
    {
    	int n;
    	vector<string> inputtimes;
    	string s;
    	getline(cin, s);
    	n = stoi(s);
    	for (int i = 0; i < n; i++){
    		getline(cin, s);
    		inputtimes.push_back(s);
    	}
    	correcttime(n, inputtimes);
    	for (auto iter = inputtimes.begin(); iter != inputtimes.end(); iter++){
    		cout << *iter << endl;
    	}
    	system("pause");
    	return 0;
    }
    
    


----------
## 网易测试 ##

题目：小云正在参与开发一个即时聊天工具，他负责其中的会话列表部分。

会话列表为显示为一个从上到下的多行控件，其中每一行表示一个会话，每一个会话都可以以一个唯一正整数id表示。

当用户在一个会话中发送或接收信息时，如果该会话已经在会话列表中，则会从原来的位置移到列表的最上方；如果没有在会话列表中，则在会话列表最上方插入该会话。

小云在现在要做的工作是测试，他会先把会话列表清空等待接收信息。当接收完大量来自不同会话的信息后，就输出当前的会话列表，以检查其中是否有bug。 
输入描述:
输入的第一行为一个正整数T（T<=10），表示测试数据组数。
接下来有T组数据。每组数据的第一行为一个正整数N（1<=N<=200），表示接收到信息的次数。第二行为N个正整数，按时间从先到后的顺序表示接收到信息的会话id。会话id不大于1000000000。


输出描述:
对于每一组数据，输出一行，按会话列表从上到下的顺序，输出会话id。
相邻的会话id以一个空格分隔，行末没有空格。

输入例子1:
3
5
1 2 3 4 5
6
1 100 1000 1000 100 1
7
1 6 3 3 1 8 1

输出例子1:
5 4 3 2 1
1 100 1000
1 8 3 6



    #include <iostream>
    #include<vector>
    #include<algorithm>
    #include<string>
    using namespace std;
    
    
    void split(const std::string &s, char delimiter, std::vector<std::string> &v)
    {
    	std::string::size_type i = 0;
    	std::string::size_type j = s.find(delimiter);
    	while (j != std::string::npos)
    	{
    		v.push_back(s.substr(i, j - i));
    		i = ++j;
    		j = s.find(delimiter, i);
    	}
    	if (j == std::string::npos)
    		v.push_back(s.substr(i, s.length()));
    }
    
    
    
    void slist(vector<vector<int>> &sessionlist){
    	vector<int> temp;
    	for (int i = 0; i < sessionlist.size(); i++){
    		temp = {};
    		for (int j = 1; j < sessionlist[i].size(); j++){
    			if (find(temp.begin(), temp.end(), sessionlist[i][j]) == temp.end()){
    				temp.push_back(sessionlist[i][j]);
    			}
    		}
    		sessionlist[i] = temp;
    	
    	}
    }
    
    int main()
    {
    	int n;
    	vector<string> inputtimes;
    	string s;
    	vector<vector<int>> sessionlist;
    	getline(cin, s);
    	n = stoi(s);
    	vector<int> tempsession;
    	vector<string> a;
    	for (int i = 0; i < n; i++){
    		tempsession = {};
    		getline(cin, s);
    		tempsession.push_back(stoi(s));
    		a = {};
    		getline(cin, s);
    		split(s, ' ', a);
    		for (int ii = a.size() - 1; ii >= 0;ii--){
    			tempsession.push_back(stoi(a[ii]));
    		}
    		sessionlist.push_back(tempsession);
    	}
    
    	slist(sessionlist);
    	for (auto iter:sessionlist){
    		for (auto iter2 : iter){
    			cout << iter2 << " ";
    		}
    		cout << endl;
    	}
    	system("pause");
    	return 0;
    }


----------
## 网易测试 ##

