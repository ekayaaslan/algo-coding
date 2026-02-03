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
  if (ids.size() > 1) {
    return {idv[0], idv[1]};
  }
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
  idx[nums[i]] = i;
}
```

```cpp
for (int i=0; i<n; i++) {
  int cnum = target-nums[i];
  if (idx.contains(cnum) && idx[cnum] != i) {
    return {i, idx[cnum]};
  }
}
```

Q1S4. [Two Sum.](https://leetcode.com/problems/two-sum) (LeetCode 1.)

```cpp
int idx = -1;
if (target%2 == 0) {
  for (int i=0; i<n; i++) {
    if (nums[i] == target/2) {
      if (idx != -1) {
        return {idx, i};
      }
      idx = i;
    }
  }
}
```

```cpp
unordered_map<int,int> idx;
for (int i=0; i<n; i++) {
  idx[nums[i]] = i;
}
```

```cpp
for (int i=0; i<n; i++) {
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
  if (idx.contains(cnum)) {
    return {i, idx[cnum]};
  }
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
sort(items.begin(), items.end(), [](item lhs, item rhs) {
  return lhs.num < rhs.num;
});
```

```cpp
for (int i=0; i<n-1; i++) {
  int cnum = target-items[i].num;
  auto [found, idx] = search(items, i+1, n-1, cnum);
  if (found) {
    return {items[i].idx, idx};
  }
}
```

```cpp
pair<bool,int> search(vector<item>& items, int lo, int hi, int target) {
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
sort(items.begin(), items.end(), [](item lhs, item rhs) {
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


