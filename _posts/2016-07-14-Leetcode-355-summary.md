---
layout: post
title: Leetcode Problem 355 Summary
date: 2016-07-14
categories: blog
tags: [study]

---

# 题目

Design a simplified version of Twitter where users can post tweets, follow/unfollow another user and is able to see the 10 most recent tweets in the user's news feed. Your design should support the following methods:

1. postTweet(userId, tweetId): Compose a new tweet.
2. getNewsFeed(userId): Retrieve the 10 most recent tweet ids in the user's news feed. Each item in the news feed must be posted by users who the user followed or by the user herself. Tweets must be ordered from most recent to least recent.
3. follow(followerId, followeeId): Follower follows a followee.
4. unfollow(followerId, followeeId): Follower unfollows a followee.
Example:

Twitter twitter = new Twitter();

// User 1 posts a new tweet (id = 5).
twitter.postTweet(1, 5);

// User 1's news feed should return a list with 1 tweet id -> [5].  
twitter.getNewsFeed(1);

// User 1 follows user 2.  
twitter.follow(1, 2);

// User 2 posts a new tweet (id = 6).  
twitter.postTweet(2, 6);

// User 1's news feed should return a list with 2 tweet ids -> [6, 5].  
// Tweet id 6 should precede tweet id 5 because it is posted after tweet id 5.  
twitter.getNewsFeed(1);

// User 1 unfollows user 2.  
twitter.unfollow(1, 2);

// User 1's news feed should return a list with 1 tweet id -> [5],  
// since user 1 is no longer following user 2.  
twitter.getNewsFeed(1);

# 我的算法

1. 用户(user)和推文(tweet)是一对多的关系，为了提高查看最新推文的效率，我使用HashMap<Integer, PriorityQueue<Tweet>>进行存储。
2. 用户(user)关注(follow)是多对多的关系，为了提高取消关注的效率，我使用HashMap<Integer, HashSet<Integer>>进行存储。
3. 设计Tweet类并写Comparator类实现Tweet根据内部time变量进行优先队列的排序。
4. time变量用于时间戳。
5. 实现时遇到的问题有，希望使用iterator遍历优先队列PriorityQueue，但是这样遍历的结果并不是按序的，而我使用迭代器输出前10个推文导致算法失效。

# 代码

{% highlight java %}
public class Twitter {
    Map<Integer, Queue<Tweet>> tweets;
    Map<Integer, Set<Integer>> follows;
    int time = 0;
    Comparator<Tweet> comp = new Comparator<Tweet>() {
                public int compare(Tweet o1, Tweet o2) {
                    if (o1.time > o2.time) {
                        return -1;
                    } else if (o1.time == o2.time) {
                        return 0;
                    } else {
                        return 1;
                    }
                }
            };

    /** Initialize your data structure here. */
    public Twitter() {
        tweets = new HashMap<>();
        follows = new HashMap<>();
    }
    
    public class Tweet {
        public int userId;
        public int tweetId;
        public int time;
        
        public Tweet(int userId, int tweetId, int time) {
            this.userId = userId;
            this.tweetId = tweetId;
            this.time = time;
        }
    }
    
    /** Compose a new tweet. */
    public void postTweet(int userId, int tweetId) {
        Queue<Tweet> userTweets = null;
        Tweet tweet = new Tweet(userId, tweetId, this.time++);
        if (!tweets.containsKey(userId)) {
            userTweets = new PriorityQueue<>(comp);
            tweets.put(userId, userTweets);
        }
        userTweets = tweets.get(userId);
        userTweets.offer(tweet);
        tweets.put(userId, userTweets);
    }
    
    /** Retrieve the 10 most recent tweet ids in the user's news feed. Each item in the news feed must be posted by users who the user followed or by the user herself. Tweets must be ordered from most recent to least recent. */
    public List<Integer> getNewsFeed(int userId) {
        List<Integer> ret = new ArrayList<>();
        Queue<Tweet> temp = new PriorityQueue<>(comp);
        if (follows.containsKey(userId)) {
            for (Integer person : follows.get(userId)) {
                if (tweets.containsKey(person)) {
                    Queue<Tweet> topTweets = new PriorityQueue<>(tweets.get(person));
                    for (int i = 0; i < 10 && !topTweets.isEmpty(); i++) {
                        temp.offer(topTweets.poll());
                    }
                }
            }
        }
        
        if (tweets.containsKey(userId)) {
            Queue<Tweet> topTweets = new PriorityQueue<>(tweets.get(userId));
            for (int i = 0; i < 10 && !topTweets.isEmpty(); i++) {
                temp.offer(topTweets.poll());
            }
        }
        
        for (int i = 0; i < 10 && !temp.isEmpty(); i++) {
            ret.add(temp.poll().tweetId);
        }
        return ret;
    }
    
    /** Follower follows a followee. If the operation is invalid, it should be a no-op. */
    public void follow(int followerId, int followeeId) {
        if (followerId == followeeId) return;
        
        Set<Integer> followees = null;
        if (!follows.containsKey(followerId)) {
            followees = new HashSet<>();
            follows.put(followerId, followees);
        }
        followees = follows.get(followerId);
        followees.add(followeeId);
        follows.put(followerId, followees);
    }
    
    /** Follower unfollows a followee. If the operation is invalid, it should be a no-op. */
    public void unfollow(int followerId, int followeeId) {
        if (followerId == followeeId) return;
        
        Set<Integer> followees = null;
        if (!follows.containsKey(followerId)) {
            return;
        }
        followees = follows.get(followerId);
        followees.remove(followeeId);
        follows.put(followerId, followees);
    }
}

/**
 * Your Twitter object will be instantiated and called as such:
 * Twitter obj = new Twitter();
 * obj.postTweet(userId,tweetId);
 * List<Integer> param_2 = obj.getNewsFeed(userId);
 * obj.follow(followerId,followeeId);
 * obj.unfollow(followerId,followeeId);
 */
{% endhighlight %}

# 改进借鉴

我的关注点在于刷题，实际的OO设计并不好，借鉴discuss中vote数最高的代码如下：

{% highlight java %}
public class Twitter {
    private static int timeStamp=0;
    
    // easy to find if user exist
    private Map<Integer, User> userMap;
    
    // Tweet link to next Tweet so that we can save a lot of time
    // when we execute getNewsFeed(userId)
    private class Tweet{
        public int id;
        public int time;
        public Tweet next;
        
        public Tweet(int id){
            this.id = id;
            time = timeStamp++;
            next=null;
        }
    }
    
    
    // OO design so User can follow, unfollow and post itself
    public class User{
        public int id;
        public Set<Integer> followed;
        public Tweet tweet_head;
        
        public User(int id){
            this.id=id;
            followed = new HashSet<>();
            follow(id); // first follow itself
            tweet_head = null;
        }
        
        public void follow(int id){
            followed.add(id);
        }
        
        public void unfollow(int id){
            followed.remove(id);
        }
        
        
        // everytime user post a new tweet, add it to the head of tweet list.
        public void post(int id){
            Tweet t = new Tweet(id);
            t.next=tweet_head;
            tweet_head=t;
        }
    }
    
    
    

    /** Initialize your data structure here. */
    public Twitter() {
        userMap = new HashMap<Integer, User>();
    }
    
    /** Compose a new tweet. */
    public void postTweet(int userId, int tweetId) {
        if(!userMap.containsKey(userId)){
            User u = new User(userId);
            userMap.put(userId, u);
        }
        userMap.get(userId).post(tweetId);
            
    }
    

    
    // Best part of this.
    // first get all tweets lists from one user including itself and all people it followed.
    // Second add all heads into a max heap. Every time we poll a tweet with 
    // largest time stamp from the heap, then we add its next tweet into the heap.
    // So after adding all heads we only need to add 9 tweets at most into this 
    // heap before we get the 10 most recent tweet.
    public List<Integer> getNewsFeed(int userId) {
        List<Integer> res = new LinkedList<>();

        if(!userMap.containsKey(userId))   return res;
        
        Set<Integer> users = userMap.get(userId).followed;
        PriorityQueue<Tweet> q = new PriorityQueue<Tweet>(users.size(), (a,b)->(b.time-a.time));
        for(int user: users){
            Tweet t = userMap.get(user).tweet_head;
            // very imporant! If we add null to the head we are screwed.
            if(t!=null){
                q.add(t);
            }
        }
        int n=0;
        while(!q.isEmpty() && n<10){
          Tweet t = q.poll();
          res.add(t.id);
          n++;
          if(t.next!=null)
            q.add(t.next);
        }
        
        return res;
        
    }
    
    /** Follower follows a followee. If the operation is invalid, it should be a no-op. */
    public void follow(int followerId, int followeeId) {
        if(!userMap.containsKey(followerId)){
            User u = new User(followerId);
            userMap.put(followerId, u);
        }
        if(!userMap.containsKey(followeeId)){
            User u = new User(followeeId);
            userMap.put(followeeId, u);
        }
        userMap.get(followerId).follow(followeeId);
    }
    
    /** Follower unfollows a followee. If the operation is invalid, it should be a no-op. */
    public void unfollow(int followerId, int followeeId) {
        if(!userMap.containsKey(followerId) || followerId==followeeId)
            return;
        userMap.get(followerId).unfollow(followeeId);
    }
}

/**
 * Your Twitter object will be instantiated and called as such:
 * Twitter obj = new Twitter();
 * obj.postTweet(userId,tweetId);
 * List<Integer> param_2 = obj.getNewsFeed(userId);
 * obj.follow(followerId,followeeId);
 * obj.unfollow(followerId,followeeId);
 */
{% endhighlight %}

# 二刷

{% highlight java %}
public class Twitter {
    class User {
        int userId;
        Set<Integer> follow;
        List<Tweet> tweets;
        public User(int userId) {
            this.userId = userId;
            follow = new HashSet<>();
            tweets = new ArrayList<>();
        }
    }
    
    class Tweet implements Comparable<Tweet> {
        int tweetId;
        int time;
        public Tweet(int tweetId, int time) {
            this.tweetId = tweetId;
            this.time = time;
        }
        @Override
        public int compareTo(Tweet t1) {
            return this.time - t1.time;
        }
    }
    static final int NEWS_NUMBER = 10;
    int timestamp;
    Map<Integer, User> users;
    /** Initialize your data structure here. */
    public Twitter() {
        timestamp = 0;
        users = new HashMap<>();
    }
    
    /** Compose a new tweet. */
    public void postTweet(int userId, int tweetId) {
        if (!users.containsKey(userId)) {
            users.put(userId, new User(userId));
        }
        User user = users.get(userId);
        user.tweets.add(new Tweet(tweetId, timestamp++));
    }
    
    /** Retrieve the 10 most recent tweet ids in the user's news feed. Each item in the news feed must be posted by users who the user followed or by the user herself. Tweets must be ordered from most recent to least recent. */
    public List<Integer> getNewsFeed(int userId) {
        if (users.containsKey(userId)) {
            User user = users.get(userId);
            Queue<Tweet> queue = new PriorityQueue<>();
            for (Integer i : user.follow) {
                User followee = users.get(i);
                List<Tweet> list = followee.tweets;
                for (Tweet t : list) {
                    // System.out.println(t.tweetId + " " + t.time);
                    if (queue.size() < NEWS_NUMBER) {
                        queue.offer(t);
                    } else if (t.time > queue.peek().time) {
                        queue.offer(t);
                        queue.poll();
                    }
                }
            }
            for (Tweet t : user.tweets) {
                if (queue.size() < NEWS_NUMBER) {
                    queue.offer(t);
                } else if (t.time > queue.peek().time) {
                    queue.offer(t);
                    queue.poll();
                }
            }
            List<Integer> result = new ArrayList<>();
            while (!queue.isEmpty()) result.add(0, queue.poll().tweetId);
            return result;
        } else {
            return new ArrayList<>();
        }
    }
    
    /** Follower follows a followee. If the operation is invalid, it should be a no-op. */
    public void follow(int followerId, int followeeId) {
        if (followerId == followeeId) return;
        if (!users.containsKey(followerId)) {
            users.put(followerId, new User(followerId));
        }
        if (!users.containsKey(followeeId)) {
            users.put(followeeId, new User(followeeId));
        }
        User follower = users.get(followerId);
        follower.follow.add(followeeId);
    }
    
    /** Follower unfollows a followee. If the operation is invalid, it should be a no-op. */
    public void unfollow(int followerId, int followeeId) {
        if (followerId == followeeId) return;
        if (!users.containsKey(followerId)) {
            users.put(followerId, new User(followerId));
        }
        if (!users.containsKey(followeeId)) {
            users.put(followeeId, new User(followeeId));
        }
        User follower = users.get(followerId);
        follower.follow.remove(followeeId);
    }
}

/**
 * Your Twitter object will be instantiated and called as such:
 * Twitter obj = new Twitter();
 * obj.postTweet(userId,tweetId);
 * List<Integer> param_2 = obj.getNewsFeed(userId);
 * obj.follow(followerId,followeeId);
 * obj.unfollow(followerId,followeeId);
 */
 {% endhighlight %}