# AIDI-1003 Course Project: Part 1 – Collecting Reddit Data on Canadian Immigration

## 1. Research Motivation and Background
I am conducting a study related to Canadian immigration as part of this semester’s final project. The core idea is: on Reddit, do users’ discussions about immigration policy show any significant differences before and after a key policy change? I consider the end of May 2023 as a watershed point and divide the timeline into:
- **Pre-policy:** 2022-12-01 to 2023-05-31 (6 months)  
- **Post-policy:** 2023-06-01 to 2023-11-30 (6 months)

The reason I chose the end of May as the dividing line is that Immigration, Refugees and Citizenship Canada (IRCC) announced a major reform: a category-based Express Entry invitation targeting specific occupations or languages ([Canada launches new process to welcome skilled newcomers with work experience in priority jobs as permanent residents - Canada.ca]). This may affect how people discuss the immigration process, occupational demands, and their attitudes toward future applications. Therefore, I want to observe in Reddit’s immigration communities whether there is any noticeable change in discussion intensity or themes following this announcement.

## 2. Research Positioning and Purpose

### 2.1. Exploratory Research: Emphasis on AI/ML Practice
It should be noted that this project is positioned as an exploratory study, primarily aimed at practicing the AI/ML tools I learned in the course and testing their applicability and limitations in large-scale text analysis. In other words, I have not done a thorough literature review as in a traditional academic paper; my goal is to first try out techniques like unsupervised topic modeling (e.g., BERTopic) and sentiment analysis to see whether they can uncover valuable initial insights or trends from real Reddit data.

### 2.2. Priority on Demonstrating Tools and Methods
As a result, the core objective of this study is not to deliver a strictly academic paper. Instead, it is to build a data pipeline myself and experiment with various ML tools to see how effectively they handle massive text data and uncover potential insights. Even if the results differ from my expectations, it will still be a valuable learning experience that helps me better understand the capability boundaries and feasibility of AI/ML tools in real research scenarios.

### 2.3. Relationship with Subsequent, More In-depth Research
Despite its exploratory nature, this study still holds some academic value: it can lay the groundwork for more systematic, comprehensive research in the future. At the current stage, however, I’m more focused on demonstrating the feasibility of techniques and methods. If you are also engaged in academic research, you can regard this as a small “methodological experiment” to see how AI/ML tools combined with human interpretation might provide ideas for future, more rigorous studies.

## 2. Target Subreddits and Scope
This study focuses on the following two subreddits:
1. `r/ImmigrationCanada`  
2. `r/CanadaImmigrant`

### 2.1. Reason for Selection: Comprehensive Coverage and Popularity
- **High popularity:**
  - Based on my observations, `r/ImmigrationCanada` has more than XX hundred-thousand subscribers, and `r/CanadaImmigrant` also has over XX hundred-thousand (the exact numbers fluctuate, but both are relatively active). This means the daily posts and comments in these two communities are fairly substantial, ensuring that there are enough posts available for both the “pre-policy” and “post-policy” periods.

- **Diverse topics:**
  - These two communities often discuss visa applications, Express Entry processes, work/study permits, and opinions on various latest immigration policies. Compared with some more specialized or narrower subreddits (e.g., those focused only on Express Entry scores or restricted to international students), `r/ImmigrationCanada` and `r/CanadaImmigrant` cover a broader, more comprehensive range of discussion, which is suitable for observing the overall immigrant population’s reaction to policies.

- **High relevance to immigration policy:**
  - Whenever IRCC releases a new policy, these two communities often see a surge of questions, experience-sharing posts, or complaints. They can serve as vantage points or “public opinion hubs.”

### 2.2. Why Not Choose Other Specialized Subreddits?
On Reddit, there are indeed more niche communities (e.g., `r/ExpressEntry` focuses solely on EE scoring and draws) or region-specific subreddits (e.g., `r/Toronto`, `r/ontarioimmigration`). The reasons I did not select them are mainly:

- **Relatively narrow or region-specific scope:**
  - `r/ExpressEntry` mostly revolves around scoring and draw updates, and does not feature as broad a range of policy discussions as the two mentioned above.  
  - Regional subreddits may focus more on local living information rather than federal-level policy changes.

- **Aim to observe broader immigration topics:**
  - My research objective is to examine immigration topics on a macro scale, not restricted to a single program or region. The posts on `r/ImmigrationCanada` and `r/CanadaImmigrant` are more diverse in origin and content, suitable for an overall comparison of the “before vs. after” policy trend.

In short, I chose `r/ImmigrationCanada` and `r/CanadaImmigrant` for their broad coverage and active user base, allowing me to capture the main focal points of public attention on Canadian immigration policy to the greatest extent.

## 3. Obtaining Reddit Data: Previous Attempts and Limitations

### 3.1. Limitations of the Reddit API
Initially, I wanted to use Reddit’s official API (for instance, through PRAW or via `requests` with OAuth2), but I quickly encountered several strict constraints:

1. **Search results limited to about 1,000 posts**  
   - The official Reddit search API (e.g., `/search` or similar endpoints) by default returns only about 1,000 of the most relevant (or most recent) posts. Exceeding this range basically gets you nothing further. Although the official documentation does not explicitly state “only 1,000 results,” the community’s experience shows that Reddit’s official search strongly limits time-based or paginated queries, greatly reducing actual retrievable data.  
   - If you want to retrieve multiple months of history from a specific subreddit, it only takes a few requests to hit this “ceiling,” making it very hard to fetch older records.

2. **Lack of a convenient deep-history interface**  
   - The Reddit API is not designed for researchers to easily download “all posts between December 2022 and May 2023.” Typically, you can only make limited requests based on time parameters or sorting. If you try to use `before`/`after` parameters for multiple requests, you can run into rate limits or repeatedly retrieve the same data.

3. **Rate limits & complex pagination**  
   - The official documentation states that each application has a request rate limit. If you want to crawl massive data daily or weekly, you would have to implement many segmented queries and carefully manage pagination and IDs. A small mistake could result in a `429 (Too Many Requests)` error or frequent rate-limit issues, making the process time-consuming and cumbersome.

In summary, the Reddit API itself was never designed for “fetching all posts of a subreddit over multiple months or a year.” If you want detailed coverage of a given subreddit in any time range, it’s nearly impossible to get it in a single sweep. You usually end up with just a few hundred to a thousand recent posts.

---

### 3.2. Why Keyword Searches Won’t Work Either
Some might attempt using Reddit’s keyword search (or the `/search?q=...` endpoint), but for research involving a large volume of historical posts, this is still unreliable:

1. **The search interface essentially also has a ~1,000 post limit**  
   - Reddit’s search page or API similarly enforces “only around 1,000 results,” with those results typically sorted by relevance or hotness, making it impossible to simply paginate to get far more than 1,000 historical posts.

2. **Diverse ways of expression**  
   - If you only search for “immigration” or “visa,” you will miss many spelling variations (“immagration,” or short references like “IM Canada”) or synonymous terms (“work permit,” “PR process”), as well as all the informal styles in Reddit conversation.  
   - To cover all these variations or synonyms, you’d have to create a long list of keywords, which would bring in a lot of irrelevant content—and also deal with spelling corrections. Reddit’s informal chatting often involves casual spelling and language.

3. **Large amounts of irrelevant noise**  
   - Suppose you search “visa,” you might retrieve posts in other subreddits about “my VISA card got stolen,” which has nothing to do with immigration.  
   - Even if you then add “canada,” you might miss posts where someone wrote “cda” or “Ontario visa application,” etc.  
   - If you also need a strict time window (for example, “all posts within a given month”), a keyword search still doesn’t guarantee **all relevant** content is captured. Results are easily skewed by search ranking or limits.

Because of these factors, for large-scale, systematic research requiring “all posts in a certain month/time range from a subreddit,” merely relying on keyword search is far from sufficient. You cannot ensure that you capture every post with alternate expressions, nor can you systematically retrieve everything from that time window without missing data.

---

### 3.3. Need for a Larger-Scale, Comprehensive Method Unrestricted by the Search API
Therefore, if I want to obtain **all** submissions from `r/ImmigrationCanada` and `r/CanadaImmigrant` spanning 2022-12 to 2023-11:

- **Reddit API:** Might yield ~1,000 recent posts at best, or a bit more after a few requests, but can’t realistically cover several months.
- **Keyword searches:** Cannot guarantee capturing all relevant posts; plus it’s still hard to do deep historical searches across large timespans.

Hence, I finally turned to monthly `.zst` files to obtain Reddit’s **complete** data, effectively bypassing these constraints. In the following, I will explain how to obtain these `.zst` files from a post in `r/pushshift`, and then use torrent plus stream decompression to filter out only the subreddits I care about.

## 4. Using Monthly .zst Archives to Overcome Reddit API Limits

### 4.1. Finding the Dump Post in r/pushshift: Why Choose .zst Files?
On `r/pushshift`, I found a post titled something like “Dump files from 2005-06 to 2024-12,” posted by user `Watchful1`. It provides a torrent or magnet link containing **all** Reddit submissions (Submissions, not comments) from June 2005 to December 2024.

- **Size:** Each monthly .zst file is about 15–20GB compressed, and over 50GB uncompressed, so you need plenty of disk space.  
- **Coverage:** It includes every post from every subreddit for that month, free from the API’s 1,000-post limit.  
- **Download method:** You must download using a torrent client, which takes patience and bandwidth.

**Why not just rely on search or other interfaces?**
- Searches or API calls can’t capture all posts from a specific month.  
- Pushshift API now has unstable updates/permissions.  
- Only these monthly .zst files actually provide **complete** data.

## 5. Step-by-Step: Download and Filtering

To obtain all the posts from `r/ImmigrationCanada` and `r/CanadaImmigrant` for my target period (December 2022 to November 2023), I performed these detailed steps:

### 5.1. Select Only the Needed Months (Torrent Download)
1. **Installation and preparation**  
   - Install a torrent client, e.g., qBittorrent or Transmission.  
   - Open the torrent file (or copy the magnet link) provided in the `r/pushshift` post.

2. **Selective download**  
   - The client will list `.zst` files from 2005-06 (RS_2005-06.zst) through 2024-12 (RS_2024-12.zst), etc.  
   - Uncheck them all initially, then select only the months you need (e.g., `RS_2022-12.zst` through `RS_2023-11.zst`).  
   - “RS” means “Reddit Submissions”; “RC” would be “Reddit Comments” (which I’m not downloading for now).

3. **Waiting for completion**  
   - The download speed depends on the number of seeders and your network conditions. It could take hours or days.  
   - Each .zst file is over 10GB, so ensure you have enough disk space.

**Small tips:**
- If your network is unstable, you can pause or stop seeding after finishing each month, then move on to the next.  
- You can set the download priority in your torrent client to get the most critical month first for testing.

### 5.2. Python + zstandard: Stream Decompression and Subreddit Filtering
Once the .zst files are downloaded, the biggest concern is that you typically cannot extract all 50GB at once just to filter a portion, because that’s too large. Hence, I used a stream-based decompression approach with a Python script.

1. **Stream decompression**  
   - Install the `zstandard` library (`pip install zstandard`).  
   - In a Python script, use `zstandard.ZstdDecompressor().stream_reader(file)` in small chunks (e.g., 1MB each) so the entire 50GB is never loaded into memory at once.

2. **Subreddit check**  
   - Each line is one Reddit submission in NDJSON format. Parse it as JSON and check whether the `subreddit` field is in `{immigrationcanada, canadaimmigrant}`.  
   - If it matches, write it to a `filtered_YYYY-MM.jsonl` (or `pre_policy.jsonl`) file; if not, skip it.

3. **Monthly outputs or direct “pre”/“post” grouping**  
   - I first processed each month’s .zst separately and saved a file like `filtered_YYYY-MM.jsonl`.  
   - Then, I combined all months from the pre-policy period into “pre_policy.jsonl” and did the same for “post_policy.jsonl,” so I could load them in one go later for comparisons.

(**Script example** [upcoming])

---

### 5.3. Download & Decompression Time and Cautions
- **Torrent seeding stability:** With good seeders and a solid network, tens of GB can download in a matter of hours; otherwise, it might take days.  
- **Script runtime:** Each .zst can contain tens of millions of submissions, so stream decompression may require hours for each file.  
- **Disk space:** Each compressed file is ~15–20GB, so plan ahead for enough storage.

**Benefits:** In the end, you keep only the two subreddits’ posts for the specified months in `.jsonl` format—much smaller—avoiding storing a massive amount of irrelevant data.

## 6. Subsequent Analysis: Foundations for Unsupervised Learning and Sentiment Research
Having completed the above steps, I now have the Reddit posts from both the “pre-policy” and “post-policy” periods. Next, I can directly load them into an unsupervised topic modeling tool (e.g., BERTopic) for comparative analysis, and check whether users’ topics or sentiments differ significantly between the two timeframes. Achieving such complete and precise time coverage would have been nearly impossible using the Reddit API or keyword searches alone.

In summary, monthly `.zst` files let us circumvent the Reddit API’s 1,000-post limit and avoid the pitfalls of missed or noisy data in keyword searches. Stream decompression plus Python scripts solve the “giant file” challenge, leaving me only the most relevant subreddit posts. I hope this experience can help anyone else seeking large-scale Reddit data. In my next update, I’ll share how I apply topic modeling, sentiment analysis, etc., and we can see together whether there are truly any notable changes in discussion before versus after the policy announcement.

## 7. Data Download Complete: Next Steps
- **Unsupervised Topic Modeling** (BERTopic or other methods)  
  Compare common themes in different time periods.
- **Human Interpretation**  
  AI can help parse large text structures, but I still need deeper reading and theoretical analysis to explain focal points from social or policy perspectives.

If you’re also interested in large-scale Reddit data collection, feel free to connect with me in the comments. In my next post, I’ll describe how to load these datasets into a model, tweak parameters, and share more thoughts on the May 2023 policy changes.
