## Question
Given a set of **N** jobs where each **jobi** has a deadline and profit associated with it.

Each job takes **_1_** unit of time to complete and only one job can be scheduled at a time. We earn the profit associated with job if and only if the job is completed by its deadline.

Find the number of jobs done and the **maximum profit**.

### Answer
```cpp
/*
struct Job
{
	int id;	 // Job Id
	int dead; // Deadline of job
	int profit; // Profit if job is over before or on deadline
};
*/

class sortByProfit
{
public:
	bool operator()(Job &a, Job &b)
	{
		return a.profit > b.profit;
	}
};

class Solution
{
public:
	int getMaxDead(vector<Job> vec)
	{
		int max = INT_MIN;
		for (auto job : vec)
		{
			if (job.dead > max)
				max = job.dead;
		}
		return max;
	}
	// Function to find the maximum profit and the number of jobs done.
	vector<int> JobScheduling(Job arr[], int n)
	{
		// Insert arr into vec
		vector<Job> vec(arr, arr + n);

		sort(vec.begin(), vec.end(), sortByProfit());

		int maxDead = getMaxDead(vec);

		vector<int> jobSeq(maxDead, -1);
		int maxProfit = 0;
		int jobsDone = 0;

		for (auto job : vec)
		{
			for (int i = job.dead - 1; i >= 0; i--)
			{
				if (jobSeq[i] == -1)
				{
					jobSeq[i] = job.id;
					maxProfit += job.profit;
					++jobsDone;
					break;
				}
			}
		}
		return {jobsDone, maxProfit};
	}
};
```