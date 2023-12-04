# A/B testing

### Tools:
- Python
- ClickHouse
- Stats

It is necessary to discover the impact of an experiment of introducing a new feature in an application with a lot of news and a messenger, which took place from 2023-06-30 to 2023-07-06.

## Step 1. A/A testing.

We have data with a ready breakdown into experimental groups. Let's check the breakdown into experimental groups using data from 2023-06-23 to 2023-06-29. It is necessary to make a simulation as if we conducted 10,000 AA tests. At each iteration, it is necessary to form subsamples without repetition of 500 users from experimental groups 2 and 3 and compare these subsamples with a t-test.

**Results:** 

When running the AA-test 10,000 times, the null hypothesis is rejected approximately 5% of the time (4.74%) $\Rightarrow$ the splitting system works correctly.

## Step 2. A/B testing.

The time has come to analyze the results of the experiment that we conducted together with the data scientist team. The experiment took place from 2023-08-29 to 2023-09-04 inclusive. The experiment involved 2 and 1 groups.

Group 2 used one of the new post recommendation algorithms, group 1 was used as a control.

The main hypothesis is that the new algorithm in group 2 will lead to an increase in CTR.

Our task is to analyze the A/B test data.

**Results:**

The introduction of a new post recommendation algorithm influenced the distribution of CTR in the second group compared to the control group, which is confirmed by statistical tests. If you look at the distribution graph of the 2nd experimental group, it is easy to notice that its users can be divided into two parts - in one part the average CTR increased, in the other it fell (compared to the control group). Moreover, according to the bootstrap, the implementation of the algorithm led to a decrease in CTR in general. Perhaps this division of users into two camps is due to the fact that the new algorithm somehow incorrectly issued recommendations on one of the two operating systems (and the most popular one) and offered users posts that were not interesting to them. Or, for example, the algorithm did not take into account geolocation and rolled out posts to users from different regions of Russia about goods/services in Moscow (for example), which is why the algorithm worked well for Muscovites, but caused a negative reaction among residents of the regions. In any case, additional research is required to determine the reasons for the drop in CTR, but for now we can say for sure that it is not worth rolling out the algorithm on new users.

## Step 3. A/B testing (linearized likes were used as a key metric instead of CTR)

Here we analyze the results of t-tests using another metric - linearized likes instead of CTR.

**Task:**

1) Analyze the test between groups 0 and 3 using the linearized likes metric. Is the difference visible? Has 𝑝−𝑣𝑎𝑙𝑢𝑒 become less compared to the usual CTR?
2) Analyze the test between groups 1 and 2 using the linearized likes metric. Is the difference visible? Has 𝑝−𝑣𝑎𝑙𝑢𝑒 become less compared to the normal CTR?

We take data in the same range in which the AB-test was carried out: from 2023-08-29 to 2023-09-04 inclusive

**Results:** 

**Conclusion**: the **p-value has become smaller** - now the null hypothesis is rejected at any standard significance level $\Rightarrow$ t-test revealed differences between groups for linearized likes, which was not observed for CTR due to the features of CTR distributions in the second group.

