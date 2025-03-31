# 🧠 How I Collected Massive Reddit Data: Pitfalls and Solutions

> *This is my first official research post, following up on yesterday's update.*

---

## 1. 🎯 Research Background: Focusing on a Canadian Immigration Policy

This project aims to examine discussions on Reddit before and after the release of a key Canadian immigration policy. I've defined two time periods:

- **Policy "Before"**: Around 6 months prior to the announcement  
- **Policy "After"**: Around 6 months after the policy was released or took effect

By comparing post volume, topics, and even sentiment across these periods, I hope to observe any noticeable changes in community engagement.

The specific policy I chose to focus on was announced in **May 2023**. It relates to **Express Entry** or another major immigration change introduced by IRCC. I hypothesize that this policy might have triggered shifts in Reddit discussions — but data will tell.

---

## 2. 📦 Where Did I Get the Data?

Initially, I turned to Reddit's official API (as I had used it before), but I ran into several issues:

### 🔹 (1) Historical Limitations of Reddit API

- Reddit’s API only returns a few hundred to a few thousand posts.
- It doesn’t allow access to older content beyond recent months.
- It’s therefore unsuitable for retrospective or policy-based analysis.

### 🔹 (2) Keyword Search Challenges

Using keywords like "immigration" or "visa" seems logical, but:

- Posts may use alternative phrasing, abbreviations, or contain typos  
- Keywords could pull in irrelevant posts (e.g., visa for other countries)  
- It’s hard to ensure full coverage of relevant content over fixed periods  

Hence, keyword-based crawling was neither reliable nor comprehensive enough for my needs.

---

## 3. 🚫 Why Not Use Pushshift?

Many Reddit researchers used to rely on Pushshift — and it used to be fantastic. But now it presents multiple issues:

- **Outdated data**: Many months are missing or not fully updated  
- **Access limitations**: Some APIs require tokens or have been deprecated  
- **Inconsistency**: It couldn’t deliver complete post sets for the exact timeframes I needed

For a “before and after policy” study, it simply didn’t work.

---

## 4. 💡 The Breakthrough: Monthly `.zst` Files

While browsing [r/pushshift](https://www.reddit.com/r/pushshift), I discovered a post titled **“Dump files from 2005-06 to 2024-12”** by user `Watchful1`.

It linked to a torrent file that contains **all Reddit submissions** from June 2005 to December 2024 — monthly `.zst` compressed files.

### ✅ Why This Was a Game-Changer

- **Bypasses API limits** — gives complete historical data access  
- **Selective download** — I only grabbed the months I needed  
- **Precise control** — no more fuzzy keyword searches or missing posts  

---

## 5. 🎯 Target Subreddits

I focused on two highly relevant communities:

- `r/ImmigrationCanada`  
- `r/CanadaImmigrant`  

These subreddits host active discussions around Express Entry, study/work visas, and experiences that reflect policy impacts — perfect for comparative analysis.

---

## 6. 🛠 How I Extracted the Data

### 📥 6.1. Downloading `.zst` Files

1. Found the post in `r/pushshift` with monthly dumps  
2. Opened the Magnet link in **qBittorrent**  
3. Selected only the `.zst` files I needed (e.g., `RS_2023-04.zst`)  
4. Waited patiently — each file is 15–20GB compressed, over 50GB uncompressed

> Tip: Don't download all files at once — just the months you need!

---

### 📂 6.2. Streamed Decompression + Filtering with Python

To avoid using up disk or memory:

- Used `zstandard` with `stream_reader` to decompress files chunk by chunk (~1MB at a time)  
- Parsed each line (Reddit submission) as JSON  
- Kept only posts where the `subreddit` field matched `r/ImmigrationCanada` or `r/CanadaImmigrant`  
- Saved those into `.jsonl` files, e.g. `filtered_2023-04.jsonl`

> Result: a clean dataset, ready for topic modeling and sentiment analysis!

---

### ⏳ 6.3. Time, Space & Automation

- **Download speed** varies based on torrent seeders  
- **Decompression** takes several hours per file  
- **Disk space**: Each output file is large but manageable  
- **Errors**: Handle bad JSON lines gracefully  
- **Automation**: I wrote a Python script to batch-process multiple `.zst` files

---

## 7. ✅ Results So Far & What’s Next

I’ve now completed:

- ✅ Downloading and filtering `.zst` Reddit data  
- ✅ Splitting data into `pre_policy.jsonl` and `post_policy.jsonl`

### 🔮 Next Steps

1. **Topic Modeling** with BERTopic to detect shifts in discussion themes  
2. **Sentiment Analysis** (if time permits) to track emotional tone  
3. **Interpretation** — machines cluster, but humans explain!

In a future blog post, I’ll show how I loaded these files into BERTopic and what insights emerged.

---

## 8. 🧠 Lessons Learned: Reddit at Scale

- Don't fear large files — **streamed processing works wonders**  
- **Torrent** is your friend, just be patient  
- Skip API and keyword limitations — go straight to raw data  
- The real challenge is in analysis and interpretation — but data collection is a big first win!

---

## 👋 Want to Discuss?

If you're interested in Reddit data or Canadian immigration research, feel free to connect!

**Stay tuned for the next post, where I dive into topic modeling results.**  
And thank you for reading!

---

*Posted by [Fanmei Wang](https://github.com/FanmeiWang) | Project: [MiniProject_HousePrice](https://github.com/FanmeiWang/MiniProject_HousePrice)*
