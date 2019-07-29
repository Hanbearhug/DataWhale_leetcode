1.给定一个链表，删除链表的倒数第 n 个节点，并且返回链表的头结点。
思路很简单，双指针，当一个指针走过n-1步后，第二个指针和第一个指针同时往它们的下一个节点移动。  
需要注意的是几种特殊情况：
1.若head为空，在链表题里首先要考虑的就是head为空的情况。
2.涉及到长度的概念，就要考虑总长度和所给定的长度（这里是n）之间的关系，在这里如果n大于链表长度，
那么倒数第n个节点不存在，我们应该直接返回头节点。
3.如果n为0，那么不删除任何节点直接返回头节点。
这三个特殊条件在下面的题目里也有体现。
```
class Solution {
    public ListNode removeNthFromEnd(ListNode head, int n) {
        if (head==null) {
        	return null;
        }
        else {
        	ListNode fast = head;
        	int count = 1;
        	while (fast!=null&&count<n) {
        		fast = fast.next;
        		count++;
        	}
        	if (count!=n) {
        		return head;
        	}
        	ListNode slow = head;
        	ListNode pre = new ListNode(-1);
        	ListNode reHead = pre;
        	pre.next = slow;
        	while (fast.next!=null) {
        		pre = pre.next;
        		slow = slow.next;
        		fast = fast.next;
        	}
        	pre.next = slow.next;
        	return reHead.next;
        }
    }
}
```
2.给定一个链表，旋转链表，将链表每个节点向右移动 k 个位置，其中 k 是非负数。
同样的，我们首先想有哪些特殊情况，那么也能列出与第一题类似的特殊情况。  
1.head为空。  
2.k大于链表长度。  
3.k为0。  
但是这里我们处理三种特殊情况的方式有所不同。对于head为空的情况我们直接返回null，  
当k大于链表长度时，由于此时为旋转链表，根据示例我们知道右移这一操作是可以循环的，  
因此我们将k对链表长度取余而后再归属到k小于链表长度的情形，对于k为0的情况，比较简单，  
直接返回头节点，不需要操作。  
剩下的与第一题类似，使用双指针找到倒数第k个节点，而后将它和前一个节点断开，再将尾节点  
连到头节点上。
```
class Solution {
    public ListNode rotateRight(ListNode head, int k) {
        if (head==null||k==0) {
        	return head;
        }
        ListNode fast = head;
        int count = 1;
        while (count<k) {
        	if (fast.next==null) {
        		fast = head;
        		k %= count;
        		if (k==0)
        			return head;
        		count=1;
        	}
        	else {
        		fast = fast.next;
        		count++;
        	}
        }
        if (fast.next==null) {
        	return head;
        }
        ListNode slow = head;
        ListNode pre = new ListNode(-1);
        pre.next = head;
        while(fast.next!=null) {
        	fast = fast.next;
        	slow = slow.next;
        	pre = pre.next;
        }
        fast.next = head;
        pre.next = null;
        return slow;
    }
}
```
3.反转链表  
链表有一个常用操作是在头节点之前设置dummy节点，反转链表时我们每一个循环都进行同样的4步，即：  
1.储存当前节点node的下一个节点next。  
2将node的next指针指向pre节点。  
3.将node赋值给pre节点。  
4.将next赋值给node节点。  
剩下的就是返回新的头节点并将原有的头节点指向null了。  
但首先考虑特殊情况，当链表的长度小于等于1，即head为空或者head的next指针指向空时是不需要反转的，  
此时可以直接返回。
```
class Solution {
    public ListNode reverseList(ListNode head) {
        if (head==null||head.next==null){
            return head;
        }
        else{
            ListNode pre = head;
            ListNode node = pre.next;
            ListNode next;
            while (node!=null){
                next = node.next;
                node.next = pre;
                pre = node;
                node = next;
            }
            head.next = null;
            return pre;
        }
    }
}
```
4.反转链表2  
反转从位置 m 到 n 的链表。请使用一趟扫描完成反转。  
本题与上题类似，由于限定条件链表长度大于等于1，因此不需要进行反转的就是m==n的情况。
除此之外分两种情况讨论：
其一是m=1，其二是m>1，当然也可以不讨论，而与第一题一样设置dummy节点，  
我们首先想清楚有哪些节点需要记录，显然需要记录的是反转开头节点的前一个节点和反转节点之后的  
一个节点，然后需要一个count变量记录当前所在的区间，保证只反转给定区域内的节点，剩下的与上题  
类似，但要格外注意的是此时原先的头节点可能仍是头节点，也可能不是，如果设置了dummy变量则不需要考虑这种情况。  
```
class Solution {
    public ListNode reverseBetween(ListNode head, int m, int n) {
        if (head.next==null||m==n){
            return head;
        }
        if (m==1){
            ListNode pre = head;
            ListNode node = pre.next;
            ListNode next=null;
            int count=1;
            while (count<n){
                next = node.next;
                node.next = pre;
                pre = node;
                node = next;
                count++;
            }
            head.next = next;
            return pre;
        }
        else{
            ListNode pre = new ListNode(-1);
            ListNode node = head;
            pre.next = head;
            int count=1;
            while (count<m){
                pre = pre.next;
                node = node.next;
                count++;
            }
            ListNode preReverse = pre;
            node = node.next;
            pre = pre.next;
            count++;
            ListNode next=null;
            while (count<=n){
                next = node.next;
                node.next = pre;
                pre = node;
                node = next;
                count++;
            }
            preReverse.next.next = next;
            preReverse.next = pre;
            return head;
        }
    }
}
```
