# Design Facebook : System Design Interview


695
a_ranjan_s's avatar
a_ranjan_s
877
Last Edit: August 30, 2023 12:47 AM

83.4K VIEWS

### Design a simple model of Facebook where people can add other people as friends. In addition, where people can post messages and that messages are visible on their friend's page. The design should be such that it can handle 10M of people. There may be, on an average 100 friends each person has. Every day each person posts around 10 messages on an average.

Use Case
A user can create their own profile.
A user can add other users to his friend list.
Users can post messages to their timeline.
The system should display posts of friends to the display board/timeline.
People can like a post.
People can share their friends post to their own display board/timeline.
Constraints
Consider a whole network of people as represented by a graph. Each person is a node and each friend
relationship is an edge of the graph.
Total number of distinct users / nodes: 10 million
Total number of distinct friend’s relationship / edges in the graph: 100 * 10 million
Number of messages posted by a single user per day: 10
Total number of messages posted by the whole network per day: 10 * 10 million
Basic Design
Our system architecture is divided into two parts:

First, the web server that will handle all the incoming requests.
The second database, which will store the entire person's profile, their friend relations and posts.
First, three requirements creating a profile, adding friends, posting messages are written some information
to the database. While the last operation is reading data from the database.

The system will look like this:

Each user will have a profile.
There will be a list of friends in each user profile.
Each user will have their own homepage where his posts will be visible.
A user can like any post of their friend and that likes will reflect on the actual message shared by his
friend.
If a user shares some post, then this post will be added to the user home page and all the other friends of
the user will see this post as a new post.

Bottleneck
A number of requests posted per day is 100 million. Approximate some 1000 request are posted per
second. There will be an uneven distribution of load so the system that we will design should be able to
handle a few thousand requests per seconds.

Scalability
Since there is, a heavy load we need horizontal scaling many web servers will be handling the requests.
In doing this we need to have a load balancer, which will distribute the request among the servers.
This approach gives us a flexibility that when the load increases, we can add more web servers to handle
the increased load.
These web servers are responsible for handling new post added by the user. They are responsible for
generating various user homepage and timeline pages. In our case, the client is the web browser,
which is rendering the page for the user.

We need to store data about user profile, Users friend list, User-generated posts, User like statues to the
posts.
Let us find out how much storage we need to store all this data. The total number of users 10 million. Let
us suppose each user is using Facebook for 5 to 6 years, so the total number of posts that a user had
produced in this whole time is approximately 20,000 million or 20 billion. Let us suppose each message
consists of 100 words or 500 characters. Let us assume each character take 2 bytes.

```md 
Total memory required = 20 * 500 * 2 billion bytes.
= 20,000 billion bytes
= 20, 000 GB
= 20 TB
1 gigabyte (GB) = 1 billion bytes
1000 gigabytes (GB) = 1 Terabytes 
```
Most of the memory is taken from the posts and the user profile and friend list will take nominal as
compared with the posts. We can use a relational database like SQL to store this data. Facebook and
twitter are using a relational database to store their data.

Responsiveness is key for social networking site. Databases have their own cache to increase their
performance. Still database access is slow as databases are stored on hard drives and they are slower
than RAM. Database performance can be increased by replication of the database. Requests can be
distributed between the various copies of the databases.

Also, there will be more reads then writes in the database so there can be multiple slave DB which are
used for reading and there can be few master DB for writing. Still database access is slow to we will use
some caching mechanism like Memcached in between application server and database. Highly popular
users and their home page will always remain in the cache.

There may be the case when the replication no longer solves the performance problem. In addition, we
need to do some Geo-location based optimization in our solution.
Again, look for a complete diagram in the scalability theory section.
If it were asked in the interview how you would store the data in the database. The schema of the
database can look like:

### Table Users:
1. User Id
2. First Name
3. Last Name
4. Email
5. Password
6. Gender
7. DOB
8. Relationship

### Table Posts:
1. Post Id
2.  Author Id
3.  Date of Creation
4.  Content

### Table Friends:
1. Relation Id
2. First Friend Id
3. Second Friend Id

### Table Likes:
1. Id
2. Post Id
3. User Id


That's it ;
Thanks for reading out !

