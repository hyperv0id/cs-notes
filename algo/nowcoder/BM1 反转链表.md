https://www.nowcoder.com/practice/75e878df47f24fdc9dc3e400ec6058ca

```c++
/*
struct ListNode {
	int val;
	struct ListNode *next;
	ListNode(int x) :
			val(x), next(NULL) {
	}
};*/
class Solution {
public:
    ListNode* ReverseList(ListNode* pHead) {
	    // 特判
		if(!pHead || !pHead->next){return pHead;}
		
		ListNode* now = pHead;
		ListNode* pre = nullptr;
		
		while(now){
			// 转换
			ListNode* cur = now;	// pre    cur(now)
			now = now->next;  // pre    cur   now
			cur->next = pre;  // pre<---cur   now
			pre = cur;  // <---cur(pre)   now
		}
		return pre;
    }
};
```