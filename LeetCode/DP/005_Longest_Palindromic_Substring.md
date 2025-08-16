# 5. Longest Palindromic Substring
### Discription
#### Given a string s, return the longest palindromic substring in s.
- A string is palindromic if it reads the same forward and backward.

### Explaination
- Optimal substructure : longest substring $k$ come from longest substring $k-1$ 
- $dp[i][j] == True $ **If** $s[j...i]$ is **palindromic**
- Base case : Single character is palindromic substring
    - $dp[i][i]$ is True
- Case : 2 same characters and more than 2 characters
    - $s[j...i]$ is palindromic **If** $s[i]$ matched $s[j]$ **and** $s[j,j_{+1},...,i_{-1},i]$ is palindromic (more than 2 characters)
    - **If** j+1 == i **and**  $s[i]$ matched $s[j]$ then $s[j,i]$ is palindromic (2 characters)

### Code
```
class Solution {
public:
    string longestPalindrome(string s) {
        int n = s.length();
        if(n<=1)return s;

        //dp[i][j] is True if s[j...i] is palindromic
        vector<vector<bool>> dp(n,vector<bool>(n,false));
        int max_length = 1;// single char is palindromic
        int l=0,r=0;// record left and right boundry

        for(int i=0;i<n;++i){
            dp[i][i] = true; // every single char is palindromic
            for(int j=0;j<i;++j){
                if((i-j<=1||dp[i-1][j+1]) && s[i]==s[j]){
                    dp[i][j] = true;
                    if(i-j+1 > max_length){
                        l=j;
                        r=i;
                        max_length = i-j+1;
                    }
                }
            }
        }
        return s.substr(l,r-l+1);
    }
};
```
### Complexity
- Time : $O(n^2)$
    - n is the length of the string
- Space : $O(n^2)$
    - $vector<vector<bool>> dp(n,vector<bool>(n,false));$