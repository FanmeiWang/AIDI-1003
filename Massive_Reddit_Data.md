# How I Collected Massive Reddit Data: Pitfalls and Solutions

*This is my first official research post, following up on yesterday's update.*

---

## 1. Research Background: Focusing on a Canadian Immigration Policy

This project aims to examine discussions on Reddit before and after the release of a key Canadian immigration policy. I've defined two time periods:

- **Policy "Before"**: Around 6 months prior to the announcement  
- **Policy "After"**: Around 6 months after the policy was released or took effect

By comparing post volume, topics, and even sentiment across these periods, I hope to observe any noticeable changes in community engagement.

The specific policy I chose to focus on was announced in **May 2023**. It relates to **Express Entry** or another major immigration change introduced by IRCC. I hypothesize that this policy might have triggered shifts in Reddit discussions — but data will tell.

---

## 2. Where Did I Get the Data?

Initially, I turned to Reddit's official API (as I had used it before), but I ran into several issues:

### 2.1 Historical Limitations of Reddit API

- Reddit’s API only returns a few hundred to a few thousand posts  
- It doesn’t allow access to older content beyond recent months  
- It’s therefore unsuitable for retrospective or policy-based analysis

### 2.2 Keyword Search Challenges

Using keywords like "immigration" or "visa" seems logical, but:

- Posts may use alternative phrasing, abbreviations, or contain typos  
- Keywords could pull in irrelevant posts (e.g., visa for other countries)  
- It’s hard to ensure full coverage of relevant content over fixed periods

Hence, keyword-based crawling was neither reliable nor comprehensive enough for my needs.

---

## 3. Why Not Use Pushshift?

Many Reddit researchers used to rely on Pushshift — and it used to be fantastic. But now it presents multiple issues:

- **Outdated data**: Many months are missing or not fully updated  
- **Access limitations**: Some APIs require tokens or have been deprecated  
- **Inconsistency**: It couldn’t deliver complete post sets for the exact timeframes I needed

For a “before and after policy” study, it simply didn’t work.

---

## 4. The Breakthrough: Monthly `.zst` Files

While browsing r/pushshift, I discovered a post titled “Dump files from 2005-06 to 2024-12” by user Watchful1.

It linked to a torrent file that contains all Reddit submissions from June 2005 to December 2024 — monthly `.zst` compressed files.

### Why This Was a Game-Changer

- Bypasses API limits — gives complete historical data access  
- Selective download — I only grabbed the months I needed  
- Precise control — no more fuzzy keyword searches or missing posts

---

## 5. Target Subreddits

To study immigration-related reactions, I focused on two relevant communities:

- `r/ImmigrationCanada`  
- `r/CanadaImmigrant`

These subreddits host active discussions around Express Entry, study/work visas, and experiences that reflect policy impacts — suitable for comparative analysis.

---

## 6. How I Extracted the Data

### 6.1 Downloading `.zst` Files

1. Found the post in r/pushshift with monthly dumps  
2. Opened the Magnet link in `qBittorrent`  
3. Selected only the `.zst` files I needed (e.g., `RS_2023-04.zst`)  
4. Waited patiently — each file is 15–20GB compressed, 50GB+ uncompressed

### 6.2 Streamed Decompression and Filtering with Python

- Used `zstandard` with `stream_reader` to decompress files chunk by chunk  
- Parsed each line (Reddit submission) as JSON  
- Filtered by `subreddit` to include only relevant posts  
- Saved data in `.jsonl` files (pre-policy, post-policy)

---

## 7. Results and Next Steps

I’ve now completed:

- Downloading and filtering `.zst` Reddit data  
- Splitting data into `pre_policy.jsonl` and `post_policy.jsonl`

### Planned Next Steps

1. Apply BERTopic or other unsupervised modeling  
2. Conduct sentiment analysis (if time permits)  
3. Interpret results qualitatively to contextualize observed shifts  

---

## 8. Lessons Learned

- Large files are manageable with streamed processing  
- Torrent download requires patience but allows targeted extraction  
- Avoiding API/keyword constraints enables full Reddit coverage  
- Data collection is only the first step — analysis and interpretation are key

---

*Posted by [Fanmei Wang](https://github.com/FanmeiWang) | Project: [MiniProject_HousePrice](https://github.com/FanmeiWang/MiniProject_HousePrice)*
