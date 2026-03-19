---

---
#google #面試紀錄
# 20210205 Online Phone Interview 

- 自我介紹
- map與unordered_map的差別? 插入與取出的complexity?
map: binary tree, time: O(logN), space: O(N)
unordered_map: hash table, time O(1), space: O(N)
- cpp裡面型別轉換casting的方法有哪些?

### 白板題 樹狀有向圖蚱蜢跳

在一個有向圖裡, 有一隻蚱蜢會一直跳, 請給出這隻蚱蜢在經過一段時間之後, 在這些node裡的機率分布(只會跳edge, 不會往回跳), 圖如果是Tree的版本, 請給出sturct並寫出程式碼

Input:

A→B

A→C

Ans:

B=0.5

C=0.5

struct TreeNode {

}

void get_prob(TreeNode* root) {

}

### 白板題 DAG蚱蜢跳

承上題, 但變成是一般的DAG(directed acyclic graph), 請給出sturct並寫出程式碼

Input:

A→B

A→C

B→C

Ans:

C=1.0

struct Node {

}

void get_prob(vector<Node *> nodes) {

}





google 20210205

- embedded software
- 2 coding + 2 domain knowledge + 1 GME
- GME should focus on "user"
- Would take approximately 1 month
- Leetcode 20 min solve 80% of medium

[[Mikaela from Google.eml]]

# 20210503 Virtual Onsite 1&2

**What is the SLAB allocator? How is it working? What’s the basic size of a page in the buddy system?**
4K or 8K?
**How many kinds of Linux memory issues?  Can you list a few examples?**
Boundary violation
Memory leakage
**How do you find out the memory leaking? How do you debug it?**
**How many configs should be enabled in the kernel for KASan?**
Unknown?   4!!!
**What does this mean from the kernel log?**
“kernel: net_sched: page allocation failure: order:4, mode:0x104020”

```c++
static int a = 0;
static int b = 0;

Thread A:                    Thread B:
b = 1;                       while (b == 0)
sleep(1);
a =1 ;                       if (a == 0)
panic();

PC is at wl_cfg80211_mgmt_tx+0x46c/0x21f4
```




[[薪水秘辛]]