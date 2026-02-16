Q1S1. [Two Sum.](https://leetcode.com/problems/two-sum) (LeetCode 1.)

```cpp
for (int i=0; i<n; i++) {
  for (int j=i+1; j<n; j++) {
    if (nums[i]+nums[j] == target) {
      return {i, j};
    }
  }
}
```

Q1S2. [Two Sum.](https://leetcode.com/problems/two-sum) (LeetCode 1.)

```cpp
unordered_map<int,vector<int>> idx;
for (int i=0; i<n; i++) {
  idx[nums[i]].push_back(i);
}
```

```cpp
if (target%2 == 0) {
  auto& idv = idx[target/2];
  if (idv.size() > 1) {
    return {idv[0], idv[1]};
  }
  // Erase target/2 to avoid returning its index in
  // the linear seach below.
  idx.erase(target/2);
}
```

```cpp
for (int i=0; i<n; i++) {
  int cnum = target-nums[i];
  if (idx.contains(cnum)) {
    return {i, idx[cnum][0]};
  }
}
```

Q1S3. [Two Sum.](https://leetcode.com/problems/two-sum) (LeetCode 1.)

```cpp
unordered_map<int,int> idx;
for (int i=0; i<n; i++) {
  // Override is intentional. This way idx keeps the
  // latest index of the number.
  idx[nums[i]] = i;
}
```

```cpp
for (int i=0; i<n; i++) {
  int cnum = target-nums[i];
  if (idx.contains(cnum)) {
    // Ensure the complement is not the element itself.
    if (idx[cnum] != i) {
      return {i, idx[cnum]};
    }
  }
}
```

Q1S4. [Two Sum.](https://leetcode.com/problems/two-sum) (LeetCode 1.)

```cpp
int index = -1;
if (target%2 == 0) {
  for (int i=0; i<n; i++) {
    if (nums[i] == target/2) {
      if (index != -1) {
        return {index, i};
      }
      index = i;
    }
  }
}
```

```cpp
unordered_map<int,int> idx;
for (int i=0; i<n; i++) {
  // Override is not strictly necessary. Idx may
  // actually hold any index of the number.
  idx[nums[i]] = i;
}
```

```cpp
for (int i=0; i<n; i++) {
  // Do not check when nums[i] is the exact half to
  // ensure it does not return the same element in the
  // pair and note this case is already handled above.
  // Also, it does not need to check when nums[i] is
  // smaller than the half as the matching pair will
  // be checked on its complement.
  if (nums[i] > target/2) {
    int cnum = target-nums[i];
    if (idx.contains(cnum)) {
      return {i, idx[cnum]};
    }
  }
}
```

Q1S5. [Two Sum.](https://leetcode.com/problems/two-sum) (LeetCode 1.)

```cpp
unordered_map<int,int> idx;
for (int i=0; i<n; i++) {
  int cnum = target-nums[i];
  // The map at this moment contains only numbers that
  // are iterated over so far. The search hence does
  // not miss any pair that may sum to target.
  if (idx.contains(cnum)) {
    return {i, idx[cnum]};
  }
  // Insert the current number *after* the check. This
  // guarantees it does not pair a number with itself.
  idx[nums[i]] = i;
}
```

Q1S6. [Two Sum.](https://leetcode.com/problems/two-sum) (LeetCode 1.)

```cpp
struct item {
  int num;
  int idx;
};
vector<item> items(n);
for (int i=0; i<n; i++) { 
  items[i] = {nums[i], i};
}
sort(items.begin(), items.end(),
  [](item lhs, item rhs) {
    return lhs.num < rhs.num;
});
```

```cpp
for (int i=0; i<n-1; i++) {
  int cnum = target-items[i].num;
  auto [found, idx] =
    search(items, i+1, n-1, cnum);
  if (found) {
    return {items[i].idx, idx};
  }
}
```

```cpp
pair<bool,int> search(
    vector<item>& items,
    int lo, int hi, int target) {
  while (lo < hi) {
    int mid = (lo+hi)/2;
    int num = items[mid].num;
    if (target <= num) {
      hi = mid;
    } else {
      lo = mid+1;
    }
  }
  item& item = items[lo];
  if (item.num == target) {
    return {true, item.idx};
  }
  return {false, 0};
}
```

Q1S7. [Two Sum.](https://leetcode.com/problems/two-sum) (LeetCode 1.)

```cpp
struct item {
  int num;
  int idx;
};
vector<item> items(n);
for (int i=0; i<n; i++) { 
  items[i] = {nums[i], i};
}
sort(items.begin(), items.end(),
  [](item lhs, item rhs) {
    return lhs.num < rhs.num;
});
```

```cpp
int i = 0;
int j = n-1;
while (i < j) {
  int sum = items[i].num + items[j].num;
  if (sum == target) {
    return {items[i].idx, items[j].idx};
  }
  if (sum < target) { 
    i ++;
  } else {
    j --;
  }
}
```

Q2S1. [Add Two Numbers](https://leetcode.com/problems/add-two-numbers) (LeetCode 2.)

```cpp
// The first node does not contain any digits of the
// sum, instead it is there for convenience.
ListNode* out = new ListNode();
ListNode* res = out;
bool carry = false;
```

```cpp
while (l1 || l2) {
  int sum = carry;
  if (l1) { sum += l1->val; }
  if (l2) { sum += l2->val; }
  out->next = new ListNode(sum%10);
  // Prepare for the next iteration.
  carry = (sum > 9);
  if (l1) { l1 = l1->next;}
  if (l2) { l2 = l2->next;}
  out = out->next;
}
```

```cpp
if (carry) {
  out->next = new ListNode(1);
}
```

```cpp
// Digits of the sum start from the second node.
return res->next;
```

Q3S1. [Longest Substring Without Repeating Characters](https://leetcode.com/problems/longest-substring-without-repeating-characters) (LeetCode 3.)

```cpp
static constexpr int K = 128;
bool has_duplicates(string& s, int lo, int hi) {
  vector<bool> seen(K);
  for (int i=lo; i<=hi; i++) {
    if (seen[s[i]]) {
      return true;
    }
    seen[s[i]] = true;
  }
  return false;
}
```
```cpp
int longest = 0;
for (int i=0; i<n; i++) {
  for (int j=i; j<n; j++) {
    if (!has_duplicates(s, i, j)) {
      longest = max(longest, j-i+1);
    }
  }
}
```

Q3S2. [Longest Substring Without Repeating Characters](https://leetcode.com/problems/longest-substring-without-repeating-characters) (LeetCode 3.)

```cpp
static constexpr int K = 128;
bool has_duplicates(string& s, int lo, int hi) {
  vector<bool> seen(K);
  for (int i=lo; i<=hi; i++) {
    if (seen[s[i]]) {
      return true;
    }
    seen[s[i]] = true;
  }
  return false;
}
```
```cpp
int longest = 0;
for (int i=0; i<n; i++) {
  for (int j=i+longest; j<n; j++) {
    if (!has_duplicates(s, i, j)) {
      longest = max(longest, j-i+1);
    }
  }
}
```

Q3S3. [Longest Substring Without Repeating Characters](https://leetcode.com/problems/longest-substring-without-repeating-characters) (LeetCode 3.)

```cpp
static constexpr int K = 128;
bool has_duplicates(string& s, int lo, int hi) {
  vector<bool> seen(K);
  for (int i=lo; i<=hi; i++) {
    if (seen[s[i]]) {
      return true;
    }
    seen[s[i]] = true;
  }
  return false;
}
```
```cpp
int longest = 0;
for (int i=0; i<n; i++) {
  for (int j=i+longest; j<min(n,i+K); j++) {
    if (!has_duplicates(s, i, j)) {
      longest = max(longest, j-i+1);
    }
  }
}
```
