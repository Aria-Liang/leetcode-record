# Day6 HashTable 01

##  Three basic way to realize hashtable 
### 1. Array
### 2. Set
### 3. Map
#### 如果需要集合是有序的，有set, 如果要求不仅有序而且有重复的数据，用multiset, set（数值不可重复）和multiset（数值可以重复）底层实现都是红黑树，key值有序，但是key不可修改，只能删除或者增加，O(logN)
#### map分为map, multimap, 和Unordered_map, 其中map（key不可重复）和multimap(key可重复）底层实现是红黑树，key有序，不可修改，查询和增删效率为O(logN)
#### Unordered_map底层实现是哈希表，key无序且key不可重复不可修改，查询和增删效率都是O(1)

##  Basic Knowledge about array
- Hashtable instantiation:
` Hashtable<String, Integer> hashtable = new Hashtable<>();`

- Hashtable Delete/Add/search/revise: 
```
// Adding elements to the hashtable
hashtable.put("A", 1);
// Getting values from the hashtable
int valueA = hashtable.get("A");
// Removing elements from the hashtable
hashtable.remove("B");

public void put(String key, Integer i) {
  if (hash.containsKey(key)) {
    List l = hash.get(key);
    if (l == null) {
      l = new ArrayList<Integer>();
      hash.put(key, l);
    }
    if (!l.contains(i)) {
      l.add(i);
    }
  }
}
```

- HashTable iteration: 
```
Set<Integer> setOfKeys = ht.keySet();
for (Integer key : setOfKeys) {
// Print and display the Rank and Name
     System.out.println("Rank : " + key + "\t\t Name : " + ht.get(key));
}
```

## Related Questions:
### [242. Valid Anagram](https://leetcode.com/problems/valid-anagram/description/)
第一反应是用hashtable做
```
import java.util.Hashtable;     //要先import一下
class Solution {
    public boolean isAnagram(String s, String t) {
        if (s.length() != t.length()){        // string the length表达方式是s.length()
            return false;
        }

        String[] sl = s.split("");        //不可以直接string element: s, 要先变成array
        Hashtable<String, Integer> hash = new Hashtable<>();
        for (String element: sl){
            if (hash.containsKey(element)){
                int l = hash.get(element);
                hash.put(element, l + 1);
            }
            else{
                hash.put(element, 1);
            }
        }

        String[] tl = t.split("");
        for (String ele: tl){
            if (hash.containsKey(ele)){
                int l = hash.get(ele);
                if (l == 1){
                    hash.remove(ele);
                }
                else{
                    hash.put(ele, l - 1);
                }
            }
            else{
                return false;
            }
        }

        return true;
    }
}
```
这里忽略了一个重要情报： 因为字符a到字符z的ASCII是26个连续的数值，所以只需要创建一个长度为26的array就可以了。在遍历 字符串s的时候，只需要将 s[i] - ‘a’ 作为index, 每出现一次，所在的元素做+1 操作即可，并不需要记住字符a的ASCII，只要求出一个相对数值就可以了。 
```
class Solution {
    public boolean isAnagram(String s, String t) {
        int[] record = new int[26];

        for (int i = 0; i < s.length(); i++) {
            record[s.charAt(i) - 'a']++;     // 并不需要记住字符a的ASCII，只要求出一个相对数值就可以了
        }

        for (int i = 0; i < t.length(); i++) {
            record[t.charAt(i) - 'a']--;
        }
        
        for (int count: record) {
            if (count != 0) {               // record数组如果有的元素不为零0，说明字符串s和t 一定是谁多了字符或者谁少了字符。
                return false;
            }
        }
        return true;                        // record数组所有元素都为零0，说明字符串s和t是字母异位词
    }
}
```

## Similar Questions:
### [383. Ransom Note](https://leetcode.com/problems/ransom-note/description/)
```
class Solution {
    public boolean canConstruct(String ransomNote, String magazine) {
        int[] letter = new int[26];

        for (int i = 0; i < magazine.length(); i++){
            int index = magazine.charAt(i) - 'a';
            letter[index] += 1;
        }

        for (int j = 0; j < ransomNote.length(); j++){
            int index = ransomNote.charAt(j) - 'a';
            letter[index] -= 1;
        }

        for (Integer num: letter){
            if (num < 0){
                return false;
            }
        }

        return true;
    }
}
```

### [49. Group Anagrams](https://leetcode.com/problems/group-anagrams/description/):

```
class Solution {
    public List<List<String>> groupAnagrams(String[] strs) {
        HashMap<String, List<String>> map = new HashMap<>();
        List<List<String>> result = new ArrayList<>();
        for (String str: strs){
            char[] new_str = str.toCharArray();    //在sort之前，要先变成char[] 注意是小写
            Arrays.sort(new_str);
            String s = new String(new_str);        //排序之后再变回string

            if (map.containsKey(s)){
                List<String> val = map.get(s);      //不可以写成 map.put(s, map.get(s).add(str)) 会报错
                val.add(str);
                map.put(s, val);
            }
            else{
                map.put(s, new ArrayList<>(Arrays.asList(str)));  //学会这个写法，Arrays.asList(str)
            }
        }

        for (List<String> list: map.values()){
            result.add(list);
        }

        return result;
    }
}

```

注意不可以把int[]作为key, A HashMap compares keys using equals() and two arrays in Java are equal only if they are the same object. 


#### [438. Find All Anagrams in a String](https://leetcode.com/problems/find-all-anagrams-in-a-string/description/)

```
class Solution {
    public List<Integer> findAnagrams(String s, String p) {
        List<Integer> result = new ArrayList<>();
        if (p.length() > s.length()){
            return result;
        }

        int[] p_num = new int[26];
        for (int i = 0; i < p.length(); i++){
            int index = p.charAt(i) - 'a';
            p_num[index] += 1;
        }

        int counter = 0;
        for (Integer j: p_num){
            counter += j;
        }
        //System.out.println("0" + counter);

        int slow = 0;
        int fast = p.length() - 1;
        for (int k = slow; k <= fast; k++){
            int index = s.charAt(k) - 'a';
            p_num[index] -= 1;
            if (p_num[index] >= 0){
                counter -= 1;
            }
            else{
                counter += 1;
            }
        }

        if (counter == 0){
            result.add(slow);
        }

        fast += 1;


        while (fast < s.length()){
            int slow_index = s.charAt(slow) - 'a';
            if (p_num[slow_index] >= 0){
                counter += 1;
            }
            else{
                counter -= 1;
            }
            p_num[slow_index] += 1;

            int fast_index = s.charAt(fast) - 'a';
            p_num[fast_index] -= 1;
            if (p_num[fast_index] >= 0){
                counter -= 1;
            }
            else{
                counter += 1;
            }


            if (counter == 0){
                result.add(slow + 1);
            }

            slow += 1;
            fast += 1;
        }

        return result;
    }
}
```
也可以创建两个array

_________________________________________________________________________________________________________________________________________________________


#### [349. Intersection of Two Arrays](https://leetcode.com/problems/intersection-of-two-arrays/description/):  

这两题因为没有限制数值的大小，就无法使用数组来做哈希表了。Array和set相比速度快，占空间小
```
class Solution {
    public int[] intersection(int[] nums1, int[] nums2) {
        if (nums1.length == 0 || nums2.length == 0){
            return new int[0];
        }

        Set<Integer> set1 = new HashSet<>();       //hashset
        Set<Integer> intersect = new HashSet<>();

        for (int i = 0; i < nums1.length; i++){
            set1.add(nums1[i]);
        }

        for (Integer num: nums2){
            if (set1.contains(num)){
                intersect.add(num);
            }
        }

        int[] result = new int[intersect.size()];
        int i = 0;
        for (Integer element: intersect){
            result[i] = element;
            i++;
        }

        return result;
    }
}
```

#### [350. Intersection of Two Arrays II](https://leetcode.com/problems/intersection-of-two-arrays-ii/description/)
```
class Solution {
    public int[] intersect(int[] nums1, int[] nums2) {
        Arrays.sort(nums1);
        Arrays.sort(nums2);

        int pointer1 = 0;
        int pointer2 = 0;

        ArrayList<Integer> output = new ArrayList<>();

        while (pointer1 < nums1.length && pointer2 < nums2.length){
            if (nums1[pointer1] < nums2[pointer2]){
                pointer1++;
            }

            else if (nums2[pointer2] < nums1[pointer1]){
                pointer2++;
            }

            else if (nums1[pointer1] == nums2[pointer2]){
                output.add(nums1[pointer1]);
                pointer1++;
                pointer2++;
            }
        }

        int[] result = new int[output.size()];

        for (int i = 0; i < output.size(); i++){
            result[i] = output.get(i);
        }

        return result;
    }
}
```

#### [202. 快乐数](https://leetcode.com/problems/happy-number/description/)
这题有个思路是如果遇到一样的数字，就可以结束循环
```
class Solution {
    public boolean isHappy(int n) {
        HashSet<Integer> sum = new HashSet<>();
    
        while (n != 1 && !sum.contains(n)){  // hashset 是 contains,不是containsKey
            sum.add(n);
            n = getSum(n);
        }

        return n == 1;

    }

    private int getSum(int m){
        int total = 0;
        while (m > 0){
            int res = m % 10;     //通过不断的%10可以取最后一位数，通过/10可以去掉最后一位数
            total += res*res;
            m = m/10;
        }

        return total;
    }
}
```

#### [1. 两数之和](https://leetcode.com/problems/two-sum/description/)
```
class Solution {
    public int[] twoSum(int[] nums, int target) {
        //可以用hashmap，时间复杂度O(n),空间复杂度O(n) 
        Map<Integer, Integer> hash_num = new HashMap<Integer, Integer>();

        for (int i = 0; i < nums.length; i++){
            int complement = target - nums[i];
            if (hash_num.containsKey(complement)){
                return new int[]{hash_num.get(complement), i};
            }

            else{
                hash_num.put(nums[i], i);
            }
        }

        return new int[2];


    }
}
```
