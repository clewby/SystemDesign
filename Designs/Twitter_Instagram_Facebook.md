# [**Twitter_Instagram_Facebook Design**](https://www.youtube.com/watch?v=bOhQLr7nbhQ)

# Functional Requirements:
* Make pasts with text, videos, images;
* Follow other users
* News feed tha quickly loads new posts from followers
* Extended requirements:
    * Liking a post 
    * Replying to a post 

# NON Functional Requirements:
* 200 millions of daily users
* Each user follows ~ 200 people
* 100 millions new tweets/posts per day
* 300 bytes per post (each character is 2 bytes, 270 bytes for tweet's text + 30 bytes for a metadata like timestamp, userId, image/video url etc.)
* 30 GB per day of storage for tweets/posts
* 54 TB per 5 years (30 GB * 365 days * 5) for tweets/posts
* If we gonna cache 20% of the tweets/posts (20% of tweets are responsible for 80% of the traffic): 180GB (30 GB * 30 days * 0.2) - cache per month
* 180 GB of cache can fit to one machine with 256GB memory, but better is to partition it as well
* Read/Write Ratio is about 100/1 (200 people that user follows / 1 write)

# API Design:
* create_post(user_id, post_content) creates post form the user
* get_news_feed(user_id) returns posts for the user
* follow(user_id1, user_id2, unfollow = false) follow/unfollow another user

# Database tables: 
* **Posts**
    * post_id: guid
    * user_id: guid
    * content: string
    * metadata: json
* **Users**
    * user_id: guid
    * email: string
    * pwd_hash: string
* **Followers** (can have indexes on both columns to query who are followers and what users user is following)
    * user_id1: guid
    * user_id2: guid


#  Architectural Overview: 
* More reads than Writes. We have to focus more on posts feed getting speed, than on latency of creating a new post
* **Naive solution** - not great
    * Use SQL related database and place all tables into it
    * Scale it up
    * Every time whe user tries to get news feed we have to find people that uesr follows and by list of ids find all people' posts order by timestamp and limit by 100/50/20 posts
    * All work done on read. But the problem is our system has 100/1 read/write ratio
    * We can do better and preconfigure news feed for each user on write
    * The downside of buildind news feed for each user on write is for STARS (who have millions of followers) it might cause millions of writes
    * Hybrid approach: for typical user we can deliver tweet to everyone's cache, for stars more work have to be done on read. Take some tweets from cache, some from read and compile everything together based on timestamps
* **Better solution**
    * Use Cassandra database that supports data partition
    * We dont have ralational model anymore
    * Partiotion and Replication
    * High chance of having hotspots (STARS posts), but we can cache certain posts in Redis
* **Images and Videos**
    * Store static videos and images in Amazon S3 + CD
* **Load Balancing**
    * To use microservices and scale services with more write pressure accordingly





![This is an image]()