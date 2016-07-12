# Twitter System Design

### Use Cases
* Post tweets (std tweet length)
* Following other users.
* Display tweets in timeline.
* mark as favorite.


##Database Schema:

**User Table**

```
UserId
UserName
FirstName
LastName
ShortSummary
PasswordHash
PasswordSalt
UdatedTime
Createdtime
//Indexes: <UserId>
```

**Tweet Table**

```
TweetId
TweetContent
UserId (foriegn Key from User Table)
CreatedTime
//indexes: <TweetId, CreatedTime>
```

**Favorites Table**

```
UserId (foriegn Key from User Table)
TweetId
CreatedTime
//Indexes: <UserId, TweetId>
```

**Connections Table**

```
FollowerId
FolloweeId
CreatedTime
//Indexes: <FollowerId, FolloweeId>
```

## Questions
* What Data to be cached?
* How the system will handle surges in followers, for example if a guy has 10 million followers how his tweet will propogate in the network.
* 