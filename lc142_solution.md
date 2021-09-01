# 方法一： 快慢指针

## ![image](https://user-images.githubusercontent.com/37040063/131587889-43fad839-2b95-48c2-8ef4-180ac888db5f.png)

x means the first meeting point

assume L is the length between the start point and the entry point of the loop.

and C is the length between the entry point and x

and D is the remaining part of the circle.

设置两个快慢指针，fast走过的路程是L+2C+D, slow走过的路程是2(L+C). 如果有环的话，两个指针会在X点碰撞。 这时我们得到L+2C+D = 2(L+C) => L = D; 
那么当两个指针第一次碰头后，设置一个res指向head， 继续同时挪动slow和res。 slow走过D， pointer走过L， 这是到达环的入口处（注意L=D）

时间复杂度： O(N),  N为链表中节点的个数;
空间复杂度：O(1), 因为我们只使用了slow，fast，res 三个指针
```
public class Solution {
    public ListNode detectCycle(ListNode head) {
        ListNode slow = head;
        ListNode fast = head;
        ListNode x = null;
        while (fast != null && fast.next!= null) {
            fast = fast.next.next;
            slow = slow.next;
            //meet point
            if (slow == fast) {
                x = fast;
                break;
            }  
        }   
        // there is no loop
        if (x == null ) return null;
        slow = head;
        while (slow != x) {
            slow = slow.next;
            x = x.next;
        }
        return slow;
    }
}
```
## 方法二 哈希表
另一个很简单的思路是使用哈希表记录走过的点。一旦这个点再次出现在哈希表中，那么这个点就是环的起点。
时间复杂度 O(n)
空间复杂度O（N），因为使用哈希表记录数据

```
public class Solution {
    public ListNode detectCycle(ListNode head) {
        Set<ListNode> visited = new HashSet<ListNode>();
        ListNode ptr = head;
        while (ptr != null) {
            if(visited.contains(ptr)) return ptr;
            else {
                visited.add(ptr);
            }
            ptr = ptr.next;
        }
        return null;
    }
}
```
