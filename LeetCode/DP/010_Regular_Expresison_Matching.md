# 10. Regular Expression Matching

### Discription
#### Given an input string s and a pattern p, implement regular expression matching with support for `.` and `*` where:
- `.` Matches any single character.​​​​
- `*` Matches zero or more of the preceding element.
- The matching should cover the entire input string (not partial).

### Explaination
- Optimal substructure : If $s_i$ matched $p_{j}$ required $s_{i-1}$ matched $p_{j-1}$
- $dp[i][j] ==> s[0...i_{-1}]$ matched with $p[0...j_{-1}]$ ?
- Base case : $dp[0][0] = True$
    - **Empty String** matched **Empty Expression**
- Cases:
    - Character: If previous matched and current $s_{i}$ matched $p_{j}$ 
        - $dp[i-1][j-1]==True =>$ Previous $s_{i-1}$ matched $p_{j-1}$
        - Then $dp[i][j] = True$
    - `.`: Matched with any character
        - If $dp[i-1][j-1]==True ==> dp[i][j] = True$
    - `*`: Check match 0 or multiple times 
        - If $dp[i][j-2] == True$ (`*` matched 0 times)
        - If Previous matched $dp[i-1][j-1]==True$ **and** $(p_{j-1}$ is `.` **or** $p_{j-1}$ matched $s_i )$ (`*` matched multiple times)


### Code
```
class Solution {
public:
    bool isMatch(string s, string p) {
        if(p==".*")return true;
        int n = s.length(), m = p.length();
        vector<vector<bool>> dp(n+1, vector<bool>(m+1, false));
        dp[0][0] = true;

        for(int i=0;i<=n;++i){
            for(int j=1;j<=m;++j){
                if(p[j-1]=='*')
                    dp[i][j] = dp[i][j-2] || (i>0 && (p[j-2]=='.' || s[i-1]==p[j-2])) && dp[i-1][j];
                else
                    dp[i][j] = i>0 && dp[i-1][j-1] && (s[i-1]==p[j-1] || p[j-1]=='.');
            }
        }
        return dp[n][m];
    }
};
```
### Complexity
- Time : O(NM)
    - **N** represent the length of the string **s**
    - **M** represent the legnth of the pattern **p**
- Space : O(NM)
    - $vector<vector<bool>> dp(n+1, vector<bool>(m+1, false));$