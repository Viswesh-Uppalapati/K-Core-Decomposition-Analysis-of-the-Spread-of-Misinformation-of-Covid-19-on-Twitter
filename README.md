# The Spread of COVID-19 Misinformation: Tracking through K-Core Decomposition on Twitter

```
Viswesh Uppalapati, Skylar Wang
December 6, 2021
```
## Introduction

The ability of social media to reach masses at the click of a button has made the communication
of information across the globe much easier, but it comes with its own problems. The
convenience of having a post reach millions of people at an instant has posed a threat to the
integrity of the information being shared. Many people today use social media as a source of
world news and current events. They rely on the posts being shared by others to catch up on their
knowledge of the world. The spread of misinformation is becoming a pressing concern by the
day as it is seeping into the vast social media networks and making it to the screens of many,
especially those who rely solely on social media for their news and information. This is
important as these users are commonly fed on lies and are building their knowledge based on
misinformation, which could be immensely harmful to the individual or the people around them
in specific situations. When such misinformation spreads into large networks of social media, the
subset of it that is shared to thousands of users is less likely to be fact-checked as there are
already a lot of people sharing and interacting with it.

In recent years, Covid-19 has become a constant in our lives. It evolved into a global pandemic
where spread of misinformation about the virus could lead to actual human casualties. This is
one case of the spread of misinformation where it is immensely harmful to the people that fail to
fact check it. Over the time of the pandemic, many people have shared different types of
misinformation such as the virus being a hoax or conspiracy, different remedies on how to cure
it, and the downplay of the threat that it poses to society. Such spread of misinformation can lead
to many people being harmed if they go uninformed.

In this study, we will compare the spread of scientific and factual information to the spread of
misinformation about Covid-19 and see how this differs. We will create misinformation networks
and see how far reaching they are and how this is harmful to the users that feed off of this
information and fail to fact-check it. The purpose of this paper is to replicate the results
presented in _Anatomy of an Online Misinformation Network_ [7]_._ In doing so, we can better
understand the spread of misinformation online to better aid preventive research on the issue.

## Methods and data

**Dataset and Data Collection**

The dataset consists of exactly 2 million tweets. is a 2 million subset of tweets acquired from the
Twitter Stream related to COVID-19 chatter by Panacea Labs at Georgia State. Panacea Labs is a
platform that collects COVID-19 related twitter data in the form of tweet-ids, geo-location, and
date for all covid related data. Their datasets are updated every day, but the time interval we


considered for this paper were the tweets between March 22nd, 2020 and October 10th, 2021.
Although we wanted the start date to February 15th, 2020, the github repository provided by
Panacea Labs had no entries before March 22nd. From this interval in time, we uniformly
sampled data from each day until we reached a collection of 2 million tweet ids. The data
collected from the stream captures all languages, but the ones with the highest prevalence are:
English, Spanish, and French. The dataset contains all tweets, retweets, and other relevant
information that gives connecting information from one tweet to the next. The tweets distributed
here are only tweet identifiers (with date and time added) due to the terms and conditions of
Twitter to re-distribute Twitter data and its rate-limiting. They need to be hydrated to be used [1].

For acquiring the actual tweets using the ids, we used Twitter’s developer API called twarc.
Twarc is the Twitter API that lets developers send requests to hydrate a certain tweet given a
tweet id.We used twarc and the tweet IDs related to Covid-19 gathered by Panacea Labs at
Georgia State to rehydrate the tweets to analyze the content of them. The process of hydrating
tweets is to pass an already collected tweet ID as a query to the API, which returns a JSON
dictionary with all the information related to the tweet corresponding to that tweet ID. The tweet
IDs from each day during the time frame we considered are separated by Panacea Lab into
individual files which had to be individually downloaded using wget, a server-request module,
and sampled from.

**Exploratory Data Analysis**

Proportion of tweets that are missing | 0.179063 |
--- | --- |
Proportion of tweets that contain a URL | 0.360685 |
--- | --- |
Number of unique users | 1051563 |
--- | --- |
Proportion of data that are retweets | 0.717135 |

**Table 1. Summary statistics of the dataset**

Firstly, we performed exploratory analysis to better understand the dataset and to acquire all the
information necessary to build the misinformation networks. The results of the analysis are
shown above in Table 1. In the dataset of 2 million tweets, the proportion of tweets that are
missing came out to be around 0.179063. In other words, around 18% of the total data is missing,
leaving in 1,641,874 valid tweets left for analysis. This can be due to many external factors such
as the user being private, the user being blocked, or the tweet being deleted. Out of all the tweets
in the dataset, around 36% of them contain a url of some sort. This is integral to the
understanding of this paper as tweets are either classified as information or misinformation
based on the content and status of the url they are sharing. There are 1,051,563 unique users that
tweeted tweets in our current dataset. And lastly, around 72% of the tweets in the datasets are
retweets of other tweets. This is also a metric that is considered further in the results section.


**Figure 1: The distribution of the number of tweets per user of the dataset**

The figure above shows the distribution of the number of tweets per user in the dataset. The
graph is one of the many generated samples of the distribution. We chose to sample a smaller
portion of the dataset as plotting the number of tweets of each unique user in the dataset results
in a histogram with a single bar as most of the data is concentrated in a single area. Therefore,
the data was sampled and plotted to see whether there was another underlying distribution. Only
one figure of the sample distribution is shown as all the data samples resulted in a nearly
identical graph. This is unsurprising as most users use Twitter to tweet once in a while and only
very few accounts ever exceed more than 15 to 20 tweets.

**Finding Misinformation**

The next step in our analysis comes in the form of classifying misinformation sources among the
tweets. To do so, we first compiled a list of misinformation sources from Iffy+, a public platform
that keeps records on the url domains that are considered to be spreading misinformation
according to other fact-checking websites and credible sources [2]. We also compiled a list of
Fact-checking courses that included websites like Snopes, FactCheck, PolitiFact, MBFC, and
FlackCheck.

Next, we went through our dataset finding all the tweets that contained urls. We grabbed the urls
from each tweet and categorized each of them as fact-check or misinformation based on the list
of sources we compiled above. We resolved the urls using requests as twitter urls are specially
encoded. We extracted the domain of the urls and checked them against all the known sources to
compute our metric. It seemed that around 2.5 to 3% of the tweets in any given trial lead to a
misinformation source. Lastly, we looked at the 10000 most popular tweets across the entire
dataset and found that there was neither any misinformation nor any fact-checking among the


most popular tweets. This is reasonable as the tweets that are super popular and are at the top of
Twitter are less likely to be misinformation due to easy detection and less likely to be
fact-checked as well as many people are sharing and retweeting them.

**Network core analysis**

A k-core is a maximal subgraph that contains nodes of degree k or more and the main core is the
core with the largest degree [4]. For social networks, k-cores are used to identify influential users
[5], and to characterize the efficiency of information spreading [6].

For our purposes, We constructed a graph of twitter users by connecting a node from user A to B
if user A retweeted a tweet by user B from our entire dataset. We used networkx, a python graph
module, to build the graph. We computed a sequence of k-core, where k∈[2, 10]. These k-cores
were used for further analysis that are discussed in our results section.

## Results and Discussion

Upon performing k-core on the entire tweets dataset. We found that many of our largest cores
occurred when k = 10. This means that the densest network of retweets in our dataset consisted
of nodes of users that have retweeted at least 10 or more tweets.

**Figure 2: Average number of tweets per user by k-value across each k-core**

The graph above is generated using the different k-cores of the main graph that were computed.
It shows that the average number of tweets per user increases with the value of k. This makes
sense as the k-core gets denser, the number of links from one user to the next increases. This is to
show that in the denser part of the k-cores, users are tweeting more frequently as they may be
bots or maybe users that try to put out as much content as possible. Upon analyzing this content,
however, it is evident that at higher k-values, there’s very little fact checking being done, which
is consistent with the findings of Shao and others [7]. The denser cores of the graph always result


in the fewest users with large numbers of edges between the nodes of the graph. This indicates
that misinformation networks present in the denser cores of the graph generally are bots or
misinformation sources as there is a negligible amount of fact checking being performed at such
large values of k. While we were unable to generate a graph to show the depletion of
fact-checking due to computing power limitations, we find that the results in the paper are
consistent with those in _Anatomy of an Online MisinformationNetwork._

## Acknowledgments

Thanks to Justin Eldridge and Aaron Fraenkel of theHalıcıoğlu Data Science Institutefor
supporting the replication and research of this paper. They provided us with the tools and
resources necessary to complete the project. We also want to thank Twitter for providing data
through their API and Panacea Lab at Georgia State for the gathered Tweet IDs for all tweets
about the COVD-19.

## References

1. Banda, Juan M et al. “A large-scale COVID-19 Twitter chatter dataset for open scientific
    research -- an international collaboration.” ArXiv arXiv:2004.03688v1. 7 Apr. 2020
    Preprint.
2. Golding, Barrett. “Iffy+ MIS/Disinfo Sites.” _Iffy.news_ ,20 June 2021,
    https://iffy.news/iffy-plus/.
3. politifact.com, factcheck.org, snopes.com, mediabiasfactcheck.com, flackcheck.org
4. An O(m) Algorithm for Cores Decomposition of Networks Vladimir Batagelj and Matjaz
    Zaversnik, 2003.https://arxiv.org/abs/cs.DS/
5. Kitsak M, Gallos LK, Havlin S, Liljeros F, Muchnik L, Stanley HE, et al. Identification
    of influential spreaders in complex networks. Nature Physics. 2010;6:888–.
6. Conover MD, Gonçalves B, Flammini A, Menczer F. Partisan asymmetries in online
    political activity. EPJ Data Science. 2012;1(1):
7. Shao C, Hui P-M, Wang L, Jiang X, Flammini A, Menczer F, et al. (2018) Anatomy of an
    online misinformation network. PLoS ONE 13(4): e0196087.
    https://doi.org/10.1371/journal.pone.


