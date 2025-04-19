# ASSIGNMENT: Sampling and Reproducibility in Python

Read the blog post [Contact tracing can give a biased sample of COVID-19 cases](https://andrewwhitby.com/2020/11/24/contact-tracing-biased/) by Andrew Whitby to understand the context and motivation behind the simulation model we will be examining.

Examine the code in `whitby_covid_tracing.py`. Identify all stages at which sampling is occurring in the model. Describe in words the sampling procedure, referencing the functions used, sample size, sampling frame, any underlying distributions involved, and how these relate to the procedure outlined in the blog post.

Run the Python script file called whitby_covid_tracing.py as is and compare the results to the graphs in the original blog post. Does this code appear to reproduce the graphs from the original blog post?

Modify the number of repetitions in the simulation to 100 (from the original 1000). Run the script multiple times and observe the outputted graphs. Comment on the reproducibility of the results.

Alter the code so that it is reproducible. Describe the changes you made to the code and how they affected the reproducibility of the script file. The output does not need to match Whitby’s original blogpost/graphs, it just needs to produce the same output when run multiple times

# Author: Shilan Dong

```
There are 3 stages of sampling, which align with the processes described in Andrew Whitby's blog post.
1. Infection Sampling
Sampling Procedure: A simple random sample from the population (1,000 individuals) to determine who gets infected.
Function: infected_indices = np.random.choice(ppl.index, size=int(len(ppl) * ATTACK_RATE), replace=False)
Sample Size: 10% of all individuals (ATTACK_RATE = 0.10), resulting in 100 infections out of 1,000 simulated people.
Sampling Frame: All individuals (200 attending a wedding and 800 attending brunches).
Distribution: Uniform random sampling without replacement. Infections are assigned uniformly across all individuals, regardless of their event type.
Relation to Blog Post: This models the assumption that infections occur equally across events (weddings and brunches)
2. Primary Contact Tracing Sampling
Sampling Procedure: Each infected person has a fixed 20% probability of being successfully traced.
Function: ppl.loc[ppl['infected'], 'traced'] = np.random.rand(sum(ppl['infected'])) < TRACE_SUCCESS
Sample Size: 20% of infected individuals (TRACE_SUCCESS = 0.20). For example, if 100 are infected, ~20 are traced.
Sampling Frame: All infected individuals (subset of the 1,000 people).
Distribution: Bernoulli trials with success probability TRACE_SUCCESS. Each infected individual has an independent 20% chance of being traced.
Relation to Blog Post: This reflects the real-world limitation where only a fraction of infections are successfully traced. The blog post emphasizes that this step introduces bias because subsequent tracing depends on this initial sample.
3: Secondary Contact Tracing
Sampling Procedure: If two or more infected individuals are traced from a particular event type, all infected individuals from that event type are considered traced.
Function: ppl.loc[ppl['event'].isin(events_traced) & ppl['infected'], 'traced'] = True
Sample Size: Varies dynamically based on primary tracing outcomes.
Sampling Frame: Infected individuals who attended events where at least two infections were already traced.
Distribution: Indirectly based on earlier Bernoulli outcomes and event grouping.
Relevance to Blog Post: This mimics cluster-based tracing, where certain event types  are more likely to be fully traced if even a couple of cases are found. As the blog highlights, this introduces systematic bias—events like weddings appear more dangerous than they actually are because they are overrepresented in traced data.

The code does not reproduce the graghs from the blog (traced wedding does not show shift towards 0.5).

There is minor shift in both samples. Reproducibility is not achieveable.

To ensure the code is reproducible, a fixed random seed is set.
```


## Criteria

|Criteria|Complete|Incomplete|
|--------|----|----|
|Altercation of the code|The code changes made, made it reproducible.|The code is still not reproducible.|
|Description of changes|The author explained the reasonings for the changes made well.|The author did not explain the reasonings for the changes made well.|

## Submission Information

🚨 **Please review our [Assignment Submission Guide](https://github.com/UofT-DSI/onboarding/blob/main/onboarding_documents/submissions.md)** 🚨 for detailed instructions on how to format, branch, and submit your work. Following these guidelines is crucial for your submissions to be evaluated correctly.

### Submission Parameters:
* Submission Due Date: `23:59 - 16/02/2025`
* The branch name for your repo should be: `assignment-1`
* What to submit for this assignment:
    * This markdown file (a1_sampling_and_reproducibility.md) should be populated.
    * The `whitby_covid_tracing.py` should be changed.
* What the pull request link should look like for this assignment: `https://github.com/<your_github_username>/sampling/pull/<pr_id>`
    * Open a private window in your browser. Copy and paste the link to your pull request into the address bar. Make sure you can see your pull request properly. This helps the technical facilitator and learning support staff review your submission easily.

Checklist:
- [ ] Create a branch called `assignment-1`.
- [ ] Ensure that the repository is public.
- [ ] Review [the PR description guidelines](https://github.com/UofT-DSI/onboarding/blob/main/onboarding_documents/submissions.md#guidelines-for-pull-request-descriptions) and adhere to them.
- [ ] Verify that the link is accessible in a private browser window.

If you encounter any difficulties or have questions, please don't hesitate to reach out to our team via the help channel in Slack. Our Technical Facilitators and Learning Support staff are here to help you navigate any challenges.
