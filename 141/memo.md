## Step.1
- 与えられた連結リストのなかにサイクルがあるかどうかを判定する。
- 連結リストの末尾が他のノードを指している場合、サイクルがある。
連結リストをwhileで調べていくことを考えた。サイクルがない場合、末尾はどこも指していないはず。
  - サイクルがある場合、末尾はどこか連結リストを指しているはず。
    そもそもこの場合、末尾かどうかの判定はできないのでは？
  - サイクルがない場合、末尾は連結リストのどのノードも指していないはず。
ここで時間切れ、方針もたたなかった。

解答をみた。
訪れたノードを記録しておいて、指しているノードがその中に含まれていれば循環していると判断できる。
入力について、while current.nextで判定をしたが、そもそも入力がhead=[]の場合を考慮できていなかった。
訪問済みノードを記録して照合する方法のほかに、fast, slowという変数を用意して、追いかけっこさせる。二つがノード上で出会えばサイクルしているといえる。
フロイドの循環検出法というらしい。
https://github.com/katsukii/leetcode/pull/12/files


```python
class Solution:
    def hasCycle(self, head: Optional[ListNode]) -> bool:
        visited_node = set()
        current = head
        while current:
            if current in visited_node:
                return True
            visited_node.add(current)
            current = current.next
        return False
```

調べたこと
- pythonのset
https://docs.python.org/3/tutorial/datastructures.html#sets
https://docs.python.org/ja/3.13/c-api/set.html python C/APIマニュアル
https://github.com/python/cpython/blob/main/Objects/setobject.c cpythonのset実装
https://github.com/fuga-98/arai60/pull/2#discussion_r1958369231 setの配列リサイズ時の挙動について。
リサイズの際は配列をk倍するので、償却計算量が平均で定数になる。k倍ではなくk回だと、償却計算量が増える。計算量が一瞬増えるが、全体の操作回数でならすとk倍の方が計算量が少なくなる。
https://github.com/python/cpython/blob/3d40317ed24d03e9511ee96316bd204a8f041746/Objects/setobject.c#L125 cpythonでリサイズしているところ。


## Step.2

参考にしたPR
https://github.com/katsukii/leetcode/pull/12/files
https://github.com/Fuminiton/LeetCode/pull/1/files
headではなくnodeを使ってリストを走査する。
https://github.com/Fuminiton/LeetCode/pull/1#discussion_r1947976750
currentという単語の使いどころに注意。コード内で前後の時間が必要な文脈なら良いかもしれないが、「注目しているノード」程度の意味ならnodeでいい。



```python
class Solution:
    def hasCycle(self, head: Optional[ListNode]) -> bool:
        visited = set()
        node = head
        while node:
            if node in visited:
                return True
            visited.add(node)
            node = node.next
        return False
```


## Step.3

```python
class Solution:
    def hasCycle(self, head: Optional[ListNode]) -> bool:
        fast = head
        slow = head
        while fast and fast.next:
            fast = fast.next.next
            slow = slow.next
            if fast == slow:
                return True
        return False
```

- 調べたこと
循環検出は、chatGPTに聞いてみたところ、下記のようなところで使えそう。
ネットワークルーティング
  パケットの無限ループを防ぐルーティングアルゴリズムで。
ガベージコレクション
  オブジェクトの循環参照を検出する。
  https://docs.python.org/ja/3.13/library/gc.html　
オートマトンとコンパイラ設計
