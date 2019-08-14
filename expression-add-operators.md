## 282. Expression Add Operators
### Problem Description 
Given a string that contains only digits 0-9 and a target value, return all possibilities to add binary operators (not unary) +, -, or * between the digits so they evaluate to the target value.

Example:
**Input**: num = `"123"`, target = `6`
**Output**: `["1+2+3", "1*2*3"]`

### First Impression
Looks like a DFS problem as at each position there are 4 choices: 
1) No operation 
2) Addition 
3) Subtraction 
4) Multiplication

### Tips/Edge Cases 
* Need to keep track of the previous operand because of multiplication. 
* Be careful of the operator precedence. (e.g. `3+4*2` should be evaluated as `11`, not `14`). 
* Need to handle cases such as `"503", where `"03"` is not a valid operand itself. 

### Code 
```python3
def addOperators(self, num, target):
        """
        :type num: str
        :type target: int
        :rtype: List[str]
        """

        def dfs(index, val, prev_val, expr): 
            if index == len(num): 
                if val == target: 
                    res.append("".join(expr)) 
            else: 
                curr_val = 0 
                for j in range(index, len(num)): 
                    curr_val = curr_val * 10 + int(num[j])
                    if index == 0: 
                        dfs(j+1, curr_val, curr_val, [str(curr_val)]) 
                    else: 
                      # multiplication 
                        product = prev_val*curr_val
                        dfs(j+1, val-prev_val+product, product, expr+["*"+str(curr_val)])
                        # addition 
                        dfs(j+1, val+curr_val, curr_val, expr+["+"+str(curr_val)]) 
                        # subtraction (notice the minus sign to handle cases such as 1 - 10 * 5) 
                        dfs(j+1, val-curr_val, -curr_val, expr+["-"+str(curr_val)])
                     # this is the handle cases such as "205", where 05 is not a valid operand 
                    if num[index] == '0': 
                        break 

        res = [] 
        dfs(0, 0, 0, []) 
        return res
```

### Time/ Complexities 
Time: O(4^N) as at each position of the input string, there are 4 possibilities. 
Space: O(N) as the depth of the recursion stack would be the length of the input string. 
