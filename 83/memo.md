## Step.1
### 考えたこと
連結リストから重複したノードを取り除く。
循環ではなく値の比較をする必要がある。ありがたいことに、ソート済みなので今見ているノードとその次に注目して、同じならskip
、そうでないならremovedにつなげる。setもいらない。

結局エラーになり、解答を見て納得。
ノードを指すポインタの使い方をよく理解せずに使っていた。
removed = nodeにすると、nodeの動きを追いかけてしまっている。


ダメだったコード
```python
class Solution:
    def deleteDuplicates(self, head: Optional[ListNode]) -> Optional[ListNode]:
        node = head
        removed = node
        while node and node.next:
            if node.val == node.next.val:
                node = node.next
                continue
            removed.next = node
            node = node.next
        return removed.next
```

修正後
```python
class Solution:
    def deleteDuplicates(self, head: Optional[ListNode]) -> Optional[ListNode]:
        node = head
        while node and node.next:
            if node.val == node.next.val:
                node.next = node.next.next
            else:
                node = node.next
        return head
```


## Step.2
読んだPR
https://github.com/quinn-sasha/leetcode/pull/3/files
  headとその次を指す二つのポインタを用意し、比較して重複していた場合、node.next = node.next.nextのように動かしている。
  https://discord.com/channels/1084280443945353267/1183683738635346001/1183690900791111730
    このcppの解答も、隣り合うものを比較している。
https://discord.com/channels/1084280443945353267/1228896007279083653/1231823840355815475
  コードが、読む人にとって謎解きになっていないかには注意。submitして結果だけ判定してくれるシステムとは違って、実際に人がコードを読むのだから。
https://discord.com/channels/1084280443945353267/1228896007279083653/1231839540700909598
  入力が配列の場合はどう書けそうか？
https://github.com/hayashi-ay/leetcode/pull/20/files

https://github.com/ichika0615/arai60/pull/3/files#r1818021744
  本質的には、group, filter, group

https://github.com/TORUS0818/leetcode/pull/5/files
  再帰で書いている。
  ノード数が300だからいいけど、どのあたりが限界なのか？
  https://discord.com/channels/1084280443945353267/1200089668901937312/1222230694843912322
  import sys
  sys.getrecursionlimit()
  が使える。
  デフォルトで1000が深さの限界のようなので、使うときは制限に気を配る。

https://github.com/hayashi-ay/leetcode/pull/20/files
  様々な解法がある。

https://github.com/fuga-98/arai60/pull/5/files
  stackを使っている。

ユニークノードを集める実装
```python
class Solution:
    def deleteDuplicates(self, head: Optional[ListNode]) -> Optional[ListNode]:
        if not head:
            return head
        node = head
        unique = ListNode()
        dummy = unique
        while node:
            if node.next and node.val == node.next.val:
                node.next = node.next.next
                continue
            dummy.next = ListNode(node.val)
            dummy = dummy.next
            node  =node.next
        return unique.next
```


C++でも解いてみた。
``cpp
class Solution {
public:
    ListNode* deleteDuplicates(ListNode* head) {
        ListNode* node = head;
        while (node != nullptr) {
            while (node->next != nullptr && node->val == node->next->val) {
                node->next = node->next->next;
            }
            node = node->next;
        }
        return head;
    }
};
```

## Step.3
```python
    def deleteDuplicates(self, head: Optional[ListNode]) -> Optional[ListNode]:
        node = head
        while node:
            while node.next and node.val == node.next.val:
                node.next = node.next.next
            node = node.next
        return head
```

