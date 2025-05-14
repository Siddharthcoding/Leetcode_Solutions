# Using matrix exponentiation

## Time Complexity : O(26^3 * log T)

``` cpp []
using ll = long long;
class Solution {
public:
    int MOD = 1e9 + 7;
    int N = 26;

    vector<vector<ll>> multiply(vector<vector<ll>> &A, vector<vector<ll>> &B) {
        vector<vector<ll>> res(N, vector<ll>(N, 0));

        for (int i = 0; i < N; i++)
            for (int k = 0; k < N; k++)
                for (int j = 0; j < N; j++)
                    res[i][j] = (res[i][j] + A[i][k] * B[k][j]) % MOD;

        return res;
    }

    vector<vector<ll>> matrixPower(vector<vector<ll>> base, int exp) {
        vector<vector<ll>> res(N, vector<ll>(N, 0));

        for (int i = 0; i < N; i++) 
        res[i][i] = 1; 

        while (exp > 0) {
            if (exp % 2 == 1) 
            res = multiply(res, base);

            base = multiply(base, base);
            exp /= 2;
        }

        return res;
    }

    int lengthAfterTransformations(string s, int t, vector<int>& nums) {
        vector<vector<ll>> trans(N, vector<ll>(N, 0));
        for (int i = 0; i < N; i++)
            for (int shift = 1; shift <= nums[i]; shift++)
                trans[(i + shift) % N][i]++;

        auto transT = matrixPower(trans, t);

        vector<ll> freq(N, 0);
        for (char ch : s) 
        freq[ch - 'a']++;

        vector<ll> finalFreq(N, 0);
        for (int i = 0; i < N; i++)
            for (int j = 0; j < N; j++)
                finalFreq[i] = (finalFreq[i] + transT[i][j] * freq[j]) % MOD;

        ll result = 0;
        for (ll count : finalFreq)
            result = (result + count) % MOD;

        return (int)result;
    }
};

```

