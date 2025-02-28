## Step.1
サイクルの開始ノードのindexを調べる。
setにvisitedノードを入れといて、サイクルを検出した時のノードを返せばよい。

```python
class Solution:
    def detectCycle(self, head: Optional[ListNode]) -> Optional[ListNode]:
        node = head
        visited = set()
        while node:
            if node in visited:
                return node
            visited.add(node)
            node = node.next
        return None
```

## Step.2
フロイドの循環検出アルゴリズムを使ってみた。
サイクル検出ができるのは分かるが、最初に出会うノードが分かる理由が分からなかった。
この説明で納得した。
https://discord.com/channels/1084280443945353267/1246383603122966570/1252209488815984710

```python
class Solution:
    def detectCycle(self, head: Optional[ListNode]) -> Optional[ListNode]:
        fast = slow = head
        while fast and fast.next:
            fast = fast.next.next
            slow = slow.next
            if fast == slow:
                break
        else:
            return None

        slow = head
        while slow != fast:
            slow = slow.next
            fast = fast.next
        return slow
```

参考にしたPR
https://github.com/yus-yus/leetcode/pull/2/files
https://github.com/katsukii/leetcode/pull/13/files　サイクル検出とサイクルの開始地点を求める関数を用意している。


## Step.3
 ```python
 class Solution:
    def detectCycle(self, head: Optional[ListNode]) -> Optional[ListNode]:
        visited = set()
        node = head
        while node:
            if node in visited:
                return node
            visited.add(node)
            node = node.next
        return None
```