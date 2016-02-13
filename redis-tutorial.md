---
layout: post
title: Redis Tutorial
permalink: /redis-tutorial/
---

##Application Development Tutorial

###Redis Tutorial
Redis is often described as a key-value data structure store, but what is a data structure store? We can think of it like this: Redis is a database that offers data structures which can be used to store your data. We will be focusing on one data structure in this tutorial, the sorted set.

In this tutorial you will learn the basics of Redis and how to integrate it with your application.

We will be building a simple voting application that makes use of Redis.

>**Prerequisite:**

>Basic background in web application development is required in this tutorial.
>Basic knowledge in databases.
>Basic knowledge in data structures.
>It is Recommended that you have completed bluemix basics (link herE!!!)

<br>

####Get a copy of the project from GitHub
1. Create a directory redistemp and go to that directory

	```text		
	> mkdir redistemp
	> cd redistemp
	```

2. Clone the repository from https://github.com/int-argc/RedisVoting.git

	```text		
		> git clone https://github.com/int-argc/RedisVoting.git
	```

3. Go to the project root.
 ```text		
		> cd redis-voting
	```
4. Notice that the directory structure looks  like this
// make ascii tree of directory

<br>

####Run the Application in Bluemix

1. The application starts with an empty list of candidates.

2. Add two candidates, Person A and Person B.

3. You can see that the candidates are added in the list.

4. Vote on Person A once. This should make Person A in the top of the list with Person B below him.

5. Now vote for Person B twice, since Person B already contains more votes than Person A, Person B should appear at the top of the list.

6. Play with the application and try to add more candidates and give them random number of votes.

7. Click reset candidate list and you will have an empty list.


Imagine how you would do that in a regular MySQL database. For example, if you would sort the candidates in the list, you will probably have something like this:

    SELECT * from Candidates ORDER BY votes DESC;

Since MySQL stores data in no particular order, MySQL will sort the candidates every time you want that data.

That can hurt performance, and that is one problems that can be solved with Redis.

The application in this tutorial uses sorted set, a data structure offered by Redis. More of it will be discussed further.


####Examine the Code
Let us first examine the code and learn how we can connect to a Redis Server.

 1. Open the file `RedisConnector.java`
	 
	
 Observe RedisConnector.java. Similar to most database systems, connecting to the Redis server also requires a driver. We will be using **Jedis** in this tutorial but there are other drivers that can be used with Java. <!-- link to others? -->

	The information needed to connect to the Redis server are the following: **IP address, port and password**.


 2. Examine the method `configParameters()`

	This method extracts the information from the cloud environment variable VCAP_SERVICES. This has already been discussed in <link to bluemix basics>.

 3. Examine the constructor `RedisConnector()`

	    pool = new JedisPool(new JedisPoolConfig(), this.host, this.port, 2000, this.password);

	> JedisPool(pool config, host, port, timeout, password);

	The constructor makes a JedisPool, this will be responsible for providing a Jedis instance.


 4. Examine SetOperations.java


``` java
public class SetOperations {

	// *** constructor removed ***

	public void add(int score, String member) {
		Jedis jedis = null;
		try {
			jedis = pool.getResource();
			jedis.zadd(keyname, score, member);
		} finally {
			jedis.close();
		}
	}

	public int getScore(String member) {
		Jedis jedis = null;
		double score = -1;
		try {
			jedis = pool.getResource();
			score = jedis.zscore(keyname, member);
		} finally {
			jedis.close();
		}

		return (int)score;
	}

	public void incrementScore(String member) {
		Jedis jedis = null;
		try {
			jedis = pool.getResource();
			jedis.zincrby(keyname, 1, member);
		} finally {
			jedis.close();
		}
	}

	public void deleteSet() {
		Jedis jedis = null;
		try {
			jedis = pool.getResource();
			jedis.del(keyname);
		} finally {
			jedis.close();
		}
	}

	public Set<String> sortDesc() {
		Jedis jedis = null;
		Set<String> s = null;
		try {
			jedis = pool.getResource();
			s = jedis.zrevrange(keyname, 0, -1);
		} finally {
			jedis.close();
		}
	}
}
```
Since we will only be using a single data structure in this tutorial, all operations on sorted sets are placed in a single class SetOperations.java

A set is a data structure that contains unique elements. Redis offers a variation of a set called a sorted set. This is similar to a set except that each element has a specific score.

Methods in SetOperations.java just calls the methods from the Jedis API, so we will just be discussing the methods from Jedis instead. Below is an overview of each method:
<br />

    zadd(<set name>, <score>, <member>)

*adds member to the set having an initial score specified by the parameter score.*

> Note: we will be using zadd to add a new candidate.

<br />

    zscore(<set name>, <member>)

*returns the score of the member*

> we will be using zscore to retrive a particular candidate's number of votes.

    zincrby(<set name>, <increment>, <member>)

*increments the score of member by the parameter increment.*

    del(<set name>)

*deletes the entire set.*

    zrevrange(<set name>, <start index>, <end index>)

*reverses the order of the sorted set and returns a subset of which from `<start index>` to `<end index>`. It would appear in a way that the set is sorted in descending order of member's score.*


<br />
With that, we already have an idea of how the application works. Candidates are members of the set. And the score of the member is the number of votes of the candidate.

Adding a candidate would be like:

    zadd("candidates", 0, "Person A")

Voting on a candidate would be like:

    zincrby("candidates", 1, "Person A")

Getting a candidate's number of votes:

    zscore("candidates", "Person A")

Getting the list of candidates ranked from the most to the least votes.

    zrevrange("candidates", 0, -1)

With the data structures offered by Redis, application data storage is simplified.

##End ---



####End of Tutorial


