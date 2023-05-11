# Day3 LinkedList01

##  Storage in Memory
Linkedlist elements are stored in non-consecutive memory locations

##  Basic Knowledge about array
- Define an Linkedlist in java: 
```
public class ListNode {
    // 结点的值
    int val;

    // 下一个结点
    ListNode next;

    // 节点的构造函数(无参)
    public ListNode() {
    }

    // 节点的构造函数(有一个参数)
    public ListNode(int val) {
        this.val = val;
    }

    // 节点的构造函数(有两个参数)
    public ListNode(int val, ListNode next) {
        this.val = val;
        this.next = next;
    }
}
```
- LinkedList instantiation:
`LinkedList<String> ll = new LinkedList<String>();`

- LinkedList Delete/Add/search/revise: 
Add/Delete: O(1) 
Search: O(n)
```
l1.add("A");
l1.addLast("B"); //at the beginning of the list
l1.addLast("C"); //at the end of the list

l1.contains(Object o);  // return true if the list contains the specified element;
l1.get(int index);      // return the element at the specific position in the list;
l1.getFirst();
l1.getLast();

l1.indexOf(Object o); // return the index of the first occurance of the specific element, return -1 if not exist

l1.poll() // retrieves and remove the head of the list
l1.remove() //retrieve and remove the head of the list
       
list.size()
```

- LinkedList iteration: 
```
for (int i = 0; i < linkedList.size(); i++) {
     System.out.print(linkedList.get(i) + " ");
}

while也可
```
- Change LinkedList to Array:
```
LinkedList<Integer> list= new LinkedList<Integer>();
Object[] a = list.toArray();
for(Object element : a)
        System.out.print(element+" ");
```

## Related Questions:
### [203. Remove Linked List Elements](https://leetcode.com/problems/remove-linked-list-elements/description/)
```
class Solution {
    public ListNode removeElements(ListNode head, int val) {
        ListNode dummy = new ListNode(0);
        dummy.next = head;
        ListNode prev = dummy;
        ListNode curr = head;

        while (curr != null){
            ListNode next = curr.next;
            if (curr.val == val){
                prev.next = next;
                curr = next;
            }
            else{
                prev = curr;
                curr = next;
            }
        }

        return dummy.next;
    }
}
```

#### [707. Design Linked List](https://leetcode.com/problems/design-linked-list/):  
singly linkedlist：
```
/** 
class ListNode{
    int val;
    ListNode next;
    ListNode(int val){
        this.val = val;
    }
}
*/

class MyLinkedList {
    int size;
    ListNode head;
    public MyLinkedList() {
        size = 0;
        head = new ListNode(0);
    }
    
    public int get(int index) {
        if (index < 0 || index >= size){
            return -1;
        }

        ListNode curr = head;
        for (int i = 0; i <= index; i++){
            curr = curr.next;
        }

        return curr.val;
    }
    
    public void addAtHead(int val) {
        addAtIndex(0, val);
    }
    
    public void addAtTail(int val) {
        addAtIndex(size, val);
    }
    
    public void addAtIndex(int index, int val) {
        if (index > size){
            return;
        }

        if (index < 0){
            index = 0;
        }

        size++;

        ListNode prev = head;
        for (int i = 0; i < index; i++){
            prev = prev.next;
        }

        ListNode toAdd = new ListNode(val);
        toAdd.next = prev.next;
        prev.next = toAdd;


    }
    
    public void deleteAtIndex(int index) {
        if (index < 0 ||  index >= size){
            return;
        }

        size--;
        if (index == 0){
            head = head.next;
            return;
        }

        ListNode prev = head;
        for (int i = 0; i < index; i++){
            prev = prev.next;
        }

        prev.next = prev.next.next;
    }
}
```
心得： 自己写的时候没有用到size和dummy node导致需要考虑很多种情况

double linkedlist
```
//双链表
class ListNode{
    int val;
    ListNode next,prev;
    ListNode() {};
    ListNode(int val){
        this.val = val;
    }
}


class MyLinkedList {  

    //记录链表中元素的数量
    int size;
    //记录链表的虚拟头结点和尾结点
    ListNode head,tail;
    
    public MyLinkedList() {
        //初始化操作
        this.size = 0;
        this.head = new ListNode(0);
        this.tail = new ListNode(0);
        //这一步非常关键，否则在加入头结点的操作中会出现null.next的错误！！！
        head.next=tail;
        tail.prev=head;
    }
    
    public int get(int index) {
        //判断index是否有效
        if(index<0 || index>=size){
            return -1;
        }
        ListNode cur = this.head;
        //判断是哪一边遍历时间更短
        if(index >= size / 2){
            //tail开始
            cur = tail;
            for(int i=0; i< size-index; i++){
                cur = cur.prev;
            }
        }else{
            for(int i=0; i<= index; i++){
                cur = cur.next; 
            }
        }
        return cur.val;
    }
    
    public void addAtHead(int val) {
        //等价于在第0个元素前添加
        addAtIndex(0,val);
    }
    
    public void addAtTail(int val) {
        //等价于在最后一个元素(null)前添加
        addAtIndex(size,val);
    }
    
    public void addAtIndex(int index, int val) {
        //index大于链表长度
        if(index>size){
            return;
        }
        //index小于0
        if(index<0){
            index = 0;
        }
        size++;
        //找到前驱
        ListNode pre = this.head;
        for(int i=0; i<index; i++){
            pre = pre.next;
        }
        //新建结点
        ListNode newNode = new ListNode(val);
        newNode.next = pre.next;
        pre.next.prev = newNode;
        newNode.prev = pre;
        pre.next = newNode;
        
    }
    
    public void deleteAtIndex(int index) {
        //判断索引是否有效
        if(index<0 || index>=size){
            return;
        }
        //删除操作
        size--;
        ListNode pre = this.head;
        for(int i=0; i<index; i++){
            pre = pre.next;
        }
        pre.next.next.prev = pre;
        pre.next = pre.next.next;
    }
}
```
基本差不多，主要是在增加node或者删除node的时候需要多连接一步

#### [206. Reverse Linked List](https://leetcode.com/problems/reverse-linked-list/)
Two Pointer
```
class Solution {
    public ListNode reverseList(ListNode head) {
        ListNode prev = null;
        ListNode curr = head;
        while (curr != null){
            ListNode next_node = curr.next;
            curr.next = prev;
            prev = curr;
            curr = next_node;
        }

        return prev;

    }
}
```
还可以用stack和recursion做
