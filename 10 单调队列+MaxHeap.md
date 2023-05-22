# Day13 Stack and Queue 03

## Related Questions:
### [239. Sliding Window Maximum](https://leetcode.com/problems/sliding-window-maximum/description/)
```
class MyQueue{
    Deque<Integer> deque = new LinkedList<>();
    public void poll(int val){
        if (!deque.isEmpty() && val == deque.peek()){
            deque.poll();
        }
    }

    public void add(int val){
        while (!deque.isEmpty() && val > deque.getLast()){
            deque.removeLast();
        }
        deque.add(val);
    }

    public int peek(){
        return deque.peek();
    }
}

class Solution {
    public int[] maxSlidingWindow(int[] nums, int k) {
        if (nums.length == 1){
            return nums;
        }

        int len = nums.length - k + 1;
        int[] res = new int[len];

        int num = 0;
        
        MyQueue myqueue = new MyQueue();

        for (int i = 0; i < k; i++){
            myqueue.add(nums[i]);
        }

        res[num++] = myqueue.peek();

        for (int i = k; i < nums.length; i++){
            myqueue.poll(nums[i - k]);
            myqueue.add(nums[i]);
            res[num++ ] = myqueue.peek();
        }

        return res;
    }
}
```
一开始的想法是暴力解法，但是会超时，这个思路是创建一个myqueue, 并且只存储max, 思路如下：
#### nums = 【1, 3，-1， -3， 5， 3， 6， 7】 k = 3
#### 1. 把1放进去， 因为此时myqueue为空 【1】
#### 2. 当想把3放进去时，比较getLast元素，此时为1， 因为3 > 1, 所以1 pop 出去（如果前面还有其他元素，从后往前依次pop出去掉）， 再把3 加进去 【3】
#### 3. 把 -1 放进去时，因为 - 1 < 3, 所以直接加进去 【3， -1】
#### 4. 从这个数字开始，需要pop掉最前面的一个数，然后再加入新的数，但是因为我们存在里面的只有最大值，所以要先判断：
要pop掉nums[0], 但是1不在里面， 所以不用操作，对于-3， 直接放进去 【3， -1， -3】
#### 5. 先pop掉nums[1] = 3， 刚好等于peek()的值，所以我们要把peek() pop掉，加入5， 发现比-3 大， removeLast(), 又发现比-1大，removeLast() 掉， 反正就是要保证里面的数字是单调降序的
#### 。。。。

### [347. Top K Frequent Elements](https://leetcode.com/problems/top-k-frequent-elements/description/)
```
class Solution {
    public int[] topKFrequent(int[] nums, int k) {
        Map<Integer, Integer> map = new HashMap<>();
        for (int num: nums){
            map.put(num, map.getOrDefault(num, 0) + 1);
        }

        PriorityQueue<int[]> pq = new PriorityQueue<>((pair1, pair2) -> pair2[1] - pair1[1]);
        for (Map.Entry<Integer, Integer> entry: map.entrySet()){
            pq.add(new int[]{entry.getKey(), entry.getValue()});
        }

        int[] ans = new int[k];
        for (int i = 0; i < k; i++){
            ans[i] = pq.poll()[0];
        }
        return ans;
    }
}
```
基本的思路能想到，但是没想到priorityQueue
#### 需要注意的知识点：
#### 1. for (int num: nums)
#### 2. map.put(num, map.getOrDefault(num, 0) + 1);
#### 3. 关于priorityQueue, 一般（num, cnt） num表示元素值， cnt表示出现的次数
#### PriorityQueue<int[]> pq = new PriorityQueue<>((pair1, pair2)->pair2[1]-pair1[1]); 如果是pair1 - pair2说明是min heap; 如果是pair2 - pair1说明是max heap
#### 类似的还有： 
```
// PriorityQueue implementing Min heap based on values of Pair<K, V>
PriorityQueue<Pair<Integer, Integer> > pq =  new PriorityQueue<>((a, b) -> a.getValue() - b.getValue());
  
// PriorityQueue implementing Max heap based on values of Pair<K, V>
PriorityQueue<Pair<Integer, Integer> > pq = new PriorityQueue<>((a, b) -> b.getValue() - a.getValue());

//PriorityQueue implementing Max heap
PriorityQueue<Pair<Integer, Integer> > pq = new PriorityQueue<>((a, b) -> b.getKey() - a.getKey());

```
#### 4. 遍历map:
```
for(Map.Entry<Integer,Integer> entry:map.entrySet()){
    pq.add(new int[]{entry.getKey(),entry.getValue()});
}
```
