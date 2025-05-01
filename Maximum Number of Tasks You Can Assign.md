# Using binary search + greedy approach

## Time Complexity : O(NlogN + MlogM)

``` cpp []
class Solution {
public:
    bool canBeAssigned(int k, vector<int>& tasks, vector<int>& workers, int pills, int strength) {
        multiset<int>remWorkers (workers.end() - k, workers.end());
        int p = pills;

        for(int i = k - 1; i >= 0; i--) {
            auto it = remWorkers.upper_bound(tasks[i] - 1);
            if(it != remWorkers.end()) {
                remWorkers.erase(it);
            } else {
                if(p == 0) 
                return false;

                it = remWorkers.lower_bound(tasks[i] - strength);

                if(it == remWorkers.end()) 
                return false;

                remWorkers.erase(it);
                p--;
            }
        }

        return true;
    }

    int maxTaskAssign(vector<int>& tasks, vector<int>& workers, int pills, int strength) {
        sort(tasks.begin(), tasks.end());
        sort(workers.begin(), workers.end());

        int low = 0, high = min(tasks.size(), workers.size()), ans = 0;

        while(low <= high) {
            int mid = (low + high) / 2;

            if(canBeAssigned(mid, tasks, workers, pills, strength)) {
                ans = mid;
                low = mid + 1;
            } else {
                high = mid - 1;
            }
        }

        return ans;
    }
};
```

