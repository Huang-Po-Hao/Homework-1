# 51115122

## 第1題

## 解題說明

題目要求實現一個遞迴函數來計算Ackermann函數  A(m, n) ，並進一步提供一個非遞迴算法來計算該函數。

### 解題策略

1. **遞迴函數**:
   
   如果  m = 0  時，則   A(m, n )=n + 1 。
   
   如果  n = 0  時，則  A(m, n )=A(m - 1, 1)。

   否則  A(m, n )=A(m - 1, A(m, n - 1))。

   直接使用遞迴實現。

2. **非遞迴算法**:
   
    為避免深層遞迴導致的堆疊溢出，設計一個使用堆疊或循環的疊代算法。
   
    將計算分解為逐步更新 \( m \) 和 \( n \) 的操作。

## 程式實作

以下為主要程式碼：

### 遞迴

```cpp
#include <iostream>
using namespace std;
int ackermann(int m, int n) {
    if (m==0) return n+1;
    else if (n==0) return ackermann(m-1,1);
    else return ackermann(m-1, ackermann(m,n-1));
}
int main() {
    int m, n;
    cin >> m >> n;
    int result = ackermann(m, n);
    cout << result << '\n';
    return 0;
}
```

### 非遞迴

```cpp
#include <iostream>
using namespace std;

int ackermann(int m, int n) {
    struct Call { int m, n; };
    Call stack[100000]; 
    int top = -1;
    stack[++top] = {m, n};
    
    while (top >= 0) {
        Call current = stack[top--];
        m = current.m;
        n = current.n;
        
        if (m == 0) {
            if (top == -1) {
                return n + 1;
            }
            stack[top].n = n + 1;
        } else if (n == 0) {
            stack[++top] = {m - 1, 1};
        } else {
            stack[++top] = {m - 1, -1}; 
            stack[++top] = {m, n - 1};
        }
    }
    return n; 
}
int main() {
    int m, n;
    cin >> m >> n;
    int result = ackermann(m, n);
    cout << result << '\n';
    return 0;
}
```

## 效能分析

1.**時間複雜度:**
 - $m=0:O(1)$
 - $m=1:O(n)$
 - $m=2:O(n)$
 - $m=3:O(2^n)$
 - $m>=4:塔級增長$
   
2.**空間複雜度:**
 - $m=0:O(1)$
 - $m=1:O(n)$
 - $m=2:O(n)$
 - $m=3:O(2^n)$
 - $m>=4:塔級增長$

## 測試與驗證

### 測試案例

| 測試案例 | 輸入參數 $(m,n)$ | 預期輸出 | 實際輸出 |
|----------|--------------|----------|----------|
| 測試一   | $(0,0)$      | 1        | 1        |
| 測試二   | $(0,1)$      | 2        | 2        |
| 測試三   | $(1,1)$      | 3        | 3        |
| 測試四   | $(2,1)$      | 5        | 5        |
| 測試五   | $(2,2)$      | 7        | 7        |
| 測試六   | $(3,1)$      | 13       | 13       |
| 測試七   | $(3,2)$      | 29       | 29       |
| 測試八   | $(3,3)$      | 61       | 61       |

### 結論
 遞迴函數能正確計算Ackermann函數值，符合定義。 

 非遞迴算法通過堆疊模擬遞迴，成功計算相同結果。

## 心得討論

### 選擇遞迴的原因
遞迴直接反映Ackermann函數的數學定義，容易理解和實現。

### 轉向非遞迴的原因

1. **效能與穩定性:**
   
     遞迴可能因大 ( m ) 或 ( n ) 導致堆疊溢出，非遞迴版本通過堆疊管理避免此問題。

     疊代實現更適合實際應用，特別是當 ( m ) 和 ( n ) 較大時。

3. **資源控制:**
   非遞迴算法允許手動管理記憶體和計算步驟，適應資源受限環境。

透過遞迴和非遞迴兩種實現，展示了Ackermann函數的計算方法。遞迴適合理論學習，而非遞迴則實用於工程應用。

# 第2題

## 解題說明

題目要求實現一個遞迴函式，計算集合S的所有可能子集。幂集包含集合S的空集以及所有可能的非空子集。

### 解題策略

1.使用遞迴函式將問題分解：對於集合S，移除最後一個元素，計算剩餘子集，並為每個子集添加或不添加該元素以生成完整幂集。

2.基本情況：當集合為空時，返回包含空集的幂集。

3.主程式呼叫遞迴函式，並輸出結果。

## 程式實作

以下為主要程式碼：

```cpp
#include <iostream>
#include <vector>
using namespace std;

void powerset(vector<char>& S, vector<vector<char>>& result, vector<char> current, int index) {
    if (index == S.size()) {
        result.push_back(current);
        return;
    }
    powerset(S, result, current, index + 1);
    current.push_back(S[index]);
    powerset(S, result, current, index + 1);
}

vector<vector<char>> powerset(vector<char> S) {
    vector<vector<char>> result;
    vector<char> current;
    powerset(S, result, current, 0);
    return result;
}

int main() {
    vector<char> S = {'a', 'b', 'c'};
    vector<vector<char>> result = powerset(S);
    for (const auto& subset : result) {
        cout << "{ ";
        for (char c : subset) {
            cout << c << " ";
        }
        cout << "}" << endl;
    }
    return 0;
}
```

## 效能分析

1.時間複雜度:
 $O(2^n)$。

2.空間複雜度:
 $O(2^n)$。

## 測試與驗證

### 測試案例

| 測試案例 | 輸入參數 $S$ | 預期輸出 | 實際輸出 |
|----------|--------------|----------|----------|
| 測試一   | $S=${}        | {}        | {}       |
| 測試二   | $S=${'a'}     | {},{a}    | {},{a}   | 
| 測試三   | $S=${'a','b'} | {},{a},{b},{a,b}     | {},{a},{b},{a,b}|
| 測試四   | $S=${'a','b','c'} | {},{a},{b},{c},{a,b},{a,c},{b,c},{a,b,c}|{},{c},{b},{b,c},{a},{a,c},{a,b},{a,b,c}         |

### 結論
 程式能正確計算集合S的所有子集。

 測試案例涵蓋了空集、單元素集和多元素集，驗證程式的正確性。

## 心得討論

1. **程式邏輯簡單直觀:**
   
    遞迴方法通過為每個元素決定是否包含，清楚表達幂集的生成過程。例如，對於 $S = ${a, b, c}，遞迴逐一處理每個元素，生成所有組合。

2. **遞迴的語意清晰:**

   每次遞迴呼叫處理一個元素，生成包含該元素和不包含該元素的子集，逐層擴展最終結果，符合問題的遞推性質。

雖然遞迴實現直觀且符合問題的本質，但當n 較大時，空間複雜度 $O(2^n)$可能導致記憶體使用問題。
