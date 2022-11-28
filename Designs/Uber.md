# [Design Uber](https://www.youtube.com/watch?v=CI4j-uo58nQ&t=127s)

## Functional Requirements:
* See all the drivers/nearby cars and keep their locations up to date
* When there is a match both passanger and driver should be able to see each others' updated location
* Optimal route calculation with both time and distance numbers

## NON Functional Requirements:
* Push location changes to backend every 3 seconds
* 1 million active daily riders, 500k active daily drivers
* 4 bytes for GeoHash, 4 bytes for lat and lng, something fora name, popularity, rating = 20-24 bytes
* 1.5M users * 25 bytes = 37.5Mb = 40 MB

## API Design:
* Search(user_lat, user_lng, radius) - return all drivers for the user within specific radius
* Match(user_id, nearbyDriversIds)
* GetRoute(startLatLng, endLatLng) - returns estimated travel time and distance

## Database tables: 
* Users(user_id, email, pw_hash)
* Drivers(user_id, email, pw_hash, in_ride)
* Rides/Trips(user_id, driver_id, route, time)

## Architectural Overview: 
* Searching and Mathching:
    - We need to make sure that our geo indexes are up to date (drivers and riders in motion). Low latency queries, update locations every 3 seconds;
    - Actual durability of the data is not that important
    - It's very netural to use in-memory based solution for geo hash - Redis cache
    - To not overload server/s with geo hash we need to use partitionig based on bounding boxes; Some bounding boxes can be 'hot' (new york / manhattan) - we can partition these 'hot' boxes into small boxes;
    - Dynamic partition schema - consistent hashing, coordination servise, gossip protocol
    - We can have Redis nodes sharded by geo hashes/bounding boxes
    - When driver moves we can push his lat lng to the right geohash Redis node
    - When rider requests ~15 drivers - redis node start sending push notifications about driver positions - web sockets, server sent events, long poll
* Shortest Path:
    - Algorithms to find shortest path between nodes: Dijkstra's algorithm, A* search algorithm; All of them (based on BFS) will be slow in a real life as long as user wants to see results almost immediatly;
    - How can we spped up it? Caching!
    - Algorithm - Contraction Hierarchies - goal is to create smaller version of graph with only 'important' nodes. It has two phases: 
        - Preprocessing phase - shortcuts/edges creation between two vertices not adjacent in the original graph. We can do that because road networks change infrequently
        - Query phase - bidirectional search is performed
* Backgroung processing:
    - Collecting payments
    - Collecting data during the ride - stream processing - Kafka

## Architectural Diagram
* Geo Service - show/search for a drivers for a riders location
* Daspatch service - request a ride
![This is an image](assets/uber.png)