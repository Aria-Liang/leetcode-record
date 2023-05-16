# Day8 String 01

##  Basic Knowledge about String
- String instantiation:
```
String str = "abc";
is equivalent to:
     char data[] = {'a', 'b', 'c'};
     String str = new String(data);
```

- String Method: 
```
charAt()	// Returns the character at the specified index (position)
contains()	// Checks whether a string contains a sequence of characters
equals()	// Compares two strings. Returns true if the strings are equal, and false if not
equalsIgnoreCase()	// Compares two strings, ignoring case considerations
isEmpty()	 // Checks whether a string is empty or not
length()	// Returns the length of a specified string
split()	// Splits a string into an array of substrings
toLowerCase()	// Converts a string to lower case letters
trim()	// Removes whitespace from both ends of a string
```

- Convert Char to String/ Convert String to Char: 
```
char c='S';  
String s=String.valueOf(c); 

char c='M';    
String s=Character.toString(c);  

for(int i=0; i<s.length();i++){  
        char c = s.charAt(i);  
        System.out.println("char at "+i+" index is: "+c);  
} 

String s1="hello";    
char[] ch=s1.toCharArray();    
for(int i=0;i<ch.length;i++){    
     System.out.println("char at "+i+" index is: "+ch[i]);   
}  
```
- Convert String to arrayList:
```
String s = "a,b,c,d,e,.........";
List<String> myList = new ArrayList<String>(Arrays.asList(s.split(",")));
```
- Convert char[] to string:
```
StringBuilder sb = new StringBuilder();
for (Character c : ch) {
     sb.append(c);
}
String output = sb.toString();

————————————————————————————————————————

或者直接new String(ch);
```

## Related Questions:
### [344. Reverse String](https://leetcode.com/problems/reverse-string/description/)
```
class Solution {
    public void reverseString(char[] s) {
        int left = 0;
        int right = s.length - 1;

        while (left < right){
            char tem = s[right];
            s[right] = s[left];
            s[left] = tem;
            left++;
            right--;
        }
    }
}
```

### [541. Reverse String II](https://leetcode.com/problems/reverse-string-ii/description/)
```
class Solution {
    public String reverseStr(String s, int k) {
        int slow = 0;
        int fast = k - 1;
        char[] ch=s.toCharArray();  

        while (fast < s.length()){
            help(ch, slow, fast);
            slow += 2*k;
            fast += 2*k;
        }

        if (slow < s.length()){
            help(ch, slow, s.length() - 1);
        }

        StringBuilder sb = new StringBuilder();
 
        for (Character c : ch) {
            sb.append(c);
        }
        
        String output = sb.toString();
        return output;
    }

    public void help(char[] s, int start, int end){
        while (start < end){
            char temp = s[end];
            s[end] = s[start];
            s[start] = temp;
            start++;
            end--;
        }
    }
}   
```
##### 思路基本正确，每次前进2k个元素，如果slow存在fast存在则直接进行反转，如果fast不存在，end index等于length - 1;
##### 可以优化： 1. 直接用string进行变化
#####           2. 用char[]但是不需要单独讨论  if (slow < s.length()){help(ch, slow, s.length() - 1)}；

优化版本1：
```
class Solution {
    public String reverseStr(String s, int k) {
        StringBuffer re = new StringBuffer();     //StringBuilder和StringBuffermutable, 但string是immutable,所以后面re.append()
        int length = s.length();
        int start = 0;

        while (start < length){
            int firstK = (start + k > length)? length: start + k;          //这种写法要熟悉
            int secondK = (start + 2*k > length)? length: start + 2*k;

            StringBuffer temp = new StringBuffer();
            temp.append(s.substring(start, firstK));
            re.append(temp.reverse());                                    //可以直接reverse

            if (firstK < secondK){
                re.append(s.substring(firstK, secondK));                  //substring前闭后开
            }
            
            start += 2*k;
        }

        return re.toString();                                            //记得之后要toString();
    }
}
```
优化版本2：
```
class Solution {
    public String reverseStr(String s, int k) {
        char[] s_list = s.toCharArray();             
        for (int i = 0; i < s_list.length; i += 2*k){                 //用for loop也可以，每次自动前进2*k
            int start = i;
            int end = Math.min(s_list.length - 1, start + k - 1);       
            while (start < end){
                char temp = s_list[end];
                s_list[end] = s_list[start];
                s_list[start] = temp;
                start++;
                end--; 
            }
        }
        return new String(s_list);
    }
}
```
### [151. Reverse Words in a String](https://leetcode.com/problems/reverse-words-in-a-string/description/)
```
class Solution {
    public String reverseWords(String s) {
        String[] ch = s.split(" ");

        int start = 0;
        int end = ch.length - 1;
        while (start < end){
            String temp = ch[end];
            ch[end] = ch[start];
            ch[start] = temp;
            start++;
            end--;
        }

        StringBuffer output = new StringBuffer();
        for (String str: ch){
            if (str.length() == 0){
                continue;
            }
            output.append(str.trim());
            output.append(" ");
        }

        String new_s = output.toString();

        return new_s.trim();
    }
}
```
#### 如果不可以使用trim和split这种java内置方法，且要求空间复杂度为O(1)：
#### 1. 去除首尾及中间多余空格
#### 2. 反转整个字符串
#### 3. 反转各个单词

```
class Solution {
    //用 char[] 来实现 String 的 removeExtraSpaces，reverse 操作
    public String reverseWords(String s) {
        char[] chars = s.toCharArray();
        //1.去除首尾以及中间多余空格
        chars = removeExtraSpaces(chars);
        //2.整个字符串反转
        reverse(chars, 0, chars.length - 1);
        //3.单词反转
        reverseEachWord(chars);
        return new String(chars);
    }

    //1.用 快慢指针 去除首尾以及中间多余空格，可参考数组元素移除的题解
    public char[] removeExtraSpaces(char[] chars) {
        int slow = 0;
        for (int fast = 0; fast < chars.length; fast++) {
            //先用 fast 移除所有空格
            if (chars[fast] != ' ') {
                //在用 slow 加空格。 除第一个单词外，单词末尾要加空格
                if (slow != 0)
                    chars[slow++] = ' ';
                //fast 遇到空格或遍历到字符串末尾，就证明遍历完一个单词了
                while (fast < chars.length && chars[fast] != ' ')
                    chars[slow++] = chars[fast++];
            }
        }
        //相当于 c++ 里的 resize()
        char[] newChars = new char[slow];
        System.arraycopy(chars, 0, newChars, 0, slow); 
        return newChars;
    }

    //双指针实现指定范围内字符串反转，可参考字符串反转题解
    public void reverse(char[] chars, int left, int right) {
        if (right >= chars.length) {
            System.out.println("set a wrong right");
            return;
        }
        while (left < right) {
            chars[left] ^= chars[right];
            chars[right] ^= chars[left];
            chars[left] ^= chars[right];
            left++;
            right--;
        }
    }

    //3.单词反转
    public void reverseEachWord(char[] chars) {
        int start = 0;
        //end <= s.length() 这里的 = ，是为了让 end 永远指向单词末尾后一个位置，这样 reverse 的实参更好设置
        for (int end = 0; end <= chars.length; end++) {
            // end 每次到单词末尾后的空格或串尾,开始反转单词
            if (end == chars.length || chars[end] == ' ') {
                reverse(chars, start, end - 1);
                start = end + 1;
            }
        }
    }
}
```
关键学习一下中间去除空格的操作

### [剑指 Offer 05. 替换空格](https://leetcode.cn/problems/ti-huan-kong-ge-lcof/)
```
class Solution {
    public String replaceSpace(String s) {
        StringBuilder sb = new StringBuilder();
        for (int i = 0; i < s.length(); i++){
            if (s.charAt(i) == ' '){        //注意这里是'  '而不是" "
                sb.append("%20");
            }
            else{
                sb.append(s.charAt(i));
            }
        }
        return sb.toString();
    }
}
```

也可以用双指针法：
```
public String replaceSpace(String s) {
    if(s == null || s.length() == 0){
        return s;
    }
    //扩充空间，空格数量2倍
    StringBuilder str = new StringBuilder();
    for (int i = 0; i < s.length(); i++) {
        if(s.charAt(i) == ' '){
            str.append("  ");
        }
    }
    //若是没有空格直接返回
    if(str.length() == 0){
        return s;
    }
    //有空格情况 定义两个指针
    int left = s.length() - 1;//左指针：指向原始字符串最后一个位置
    s += str.toString();
    int right = s.length()-1;//右指针：指向扩展字符串的最后一个位置
    char[] chars = s.toCharArray();
    while(left>=0){
        if(chars[left] == ' '){
            chars[right--] = '0';
            chars[right--] = '2';
            chars[right] = '%';
        }else{
            chars[right] = chars[left];
        }
        left--;
        right--;
    }
    return new String(chars);
}
```

#### [剑指 Offer 58 - II. 左旋转字符串](https://leetcode.cn/problems/zuo-xuan-zhuan-zi-fu-chuan-lcof/)
```
class Solution {
    public String reverseLeftWords(String s, int n) {
        StringBuilder sb = new StringBuilder();

        for (int i = n; i < s.length(); i++){
            sb.append(s.charAt(i));
        }

        for (int j = 0; j < n; j++){
            sb.append(s.charAt(j));
        }

        return sb.toString();
    }
}
```

也可以通过翻转来做
```
class Solution {
    public String reverseLeftWords(String s, int n) {
        int len=s.length();
        StringBuilder sb=new StringBuilder(s);
        reverseString(sb,0,n-1);
        reverseString(sb,n,len-1);
        return sb.reverse().toString();
    }
     public void reverseString(StringBuilder sb, int start, int end) {
        while (start < end) {
            char temp = sb.charAt(start);
            sb.setCharAt(start, sb.charAt(end));
            sb.setCharAt(end, temp);
            start++;
            end--;
            }
        }
}
```
