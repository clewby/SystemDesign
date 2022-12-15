# [Design Yelp](https://www.youtube.com/watch?v=dSuYtpk59OY)

# Functional Requirements:
* Add places and review them
* See/Query nearby places within rating (focus most on optimization)

# NON Functional Requirements:
* Read heavy application
* 500 million places, 100k queries/s
* 8 bytes for location id/geoHash
* 500 million * 8 bytes = 4 GB

# API Design:
* AddReview(location_id, user_id, stars, text)
* AddLocation(lat, lng, descr, category)
* Search(user_lat, user_lng, radius, queryTerms)

# Database tables: 
* User(user_id, email, pw_hash)
* Location(location_id, lat, lng, name, descr, category)
* Review(user_id, location_id, stars, text)

# Architectural Overview: 
* Use Geohashing to perform serarch queries
* Write Service - add reviews and locations
* We can use SQL database to keep reviews and locations
* Shard reviews by location_id (no need to aggregate reviews in parallel)
* Single leader replication for SQL db is a good idea
* Stream location changes by Kafka to Elastic Search cluster
* Search Service -> Location Cache -> Elastic


 

<!--
> quoted txt

```
some code
```
color - `#0969DA`

This site was built using [GitHub Pages](https://pages.github.com/).

* List1
* List2

- [x] #739
- [ ] https://github.com/octo-org/octo-repo/issues/740
- [ ] \(opt) Add delight to the experience when all tasks are complete :tada:
-->