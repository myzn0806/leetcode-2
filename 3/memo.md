https://leetcode.com/problems/longest-substring-without-repeating-characters/description/?envType=problem-list-v2&envId=xo2bgr0r

stringに含まれる重複しない文字列の最長を考える。
abcabcbb -> abc 3
bbbbb -> b 1
pwwkew -> wke 3

単純に考えると、先頭から一文字ずつ確認して条件に該当する文字列を作り、長さを確認する。
やることとしては、
1. まず一文字目から読み出す。
2. その文字を重複しない文字列に加える。
3. 次の文字を読み出す。
4. 読み出した文字との重複チェックをする。
5. 重複しない場合、重複しない文字列に追加、2に戻る
6. 重複する場合、そこで終了、次の文字に進めて1に戻る

上記の方針について、下記が考えられていない。
- 重複チェックをするのは良いが、その後の文字列長はどのように管理するか
- 入力される文字列の考慮（s consists of English letters, digits, symbols and spaces.）

ブランクの時にどのように扱えばいいのかがわからず、ブランクの理解、また自分で書いたコードの理解ができていない
現時点でのソース
```python
class Solution:
    def lengthOfLongestSubstring(self, s: str) -> int:
        left=0
        seen=[]
        max_len=0
        while left < len(s):
            right=0
            target_str=s[left:]
            while right < len(target_str):
                c=target_str[right]
                if c in seen:
                    max_len=max(max_len, len(seen))
                    break
                seen.append(c)
                right+=1
            left+=1
            seen.clear()
        return max_len
```

参考にしたPR
https://github.com/hayashi-ay/leetcode/pull/47/changes#diff-eaf04e4839867b1c256a01b37e3fd908cd889582123148e5910d72e4e4fcb421R87-R99
https://github.com/garunitule/coding_practice/pull/47/changes
https://github.com/h1rosaka/arai60/pull/49/changes
