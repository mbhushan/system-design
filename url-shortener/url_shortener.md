## Design URL Shortener Service

```

Problem Statement: To design a url shortener service
which takes long urls and returns a short url which is
easy to share on facebook, twitter, sms etc.

```

### Questions:
* urls have expiration date?
* Support custome urls?


Algorithm to convert long urls to short urls like **famous.com/756ab**
> **Long URL:** https://www.famous.com/cutecat
> 
> **Short url:** https://famo.us/71uabz

#### Rough calculations on id space:
If we go for 6 digit IDs consisting of [0-9][a-z][A-Z] then we can generate (10 + 26 + 26)^6 = 62^6 = ~56 Billion IDs (that’s a lot!).

#### Request Response Life Cycle:
* URL Shortener API get a request at load balancer.
* Load balancer directs the request to one of the attached web server.
* Web server parses the request and extracts the long url.
* We make a request to redis incr method and get the integer value VAL.
* Convert VAL to base 62 called KEY
* Now store the KEY, Long_URL in a central key,value store.
* Return the short url as response.

#### Rough Latency numbers: 
> There are 2 round trips to central key->value store, one for get autoincrement id and another for storing the key and long url value. This total amounts to ~2 ms.
> 
We can optimize this further if we get the auto-increment integer value from the webserver itself. This will save 1 network call ie 1 ms in overall latency.

#### Total storage needed:
* Lets take a sample site like twitter for estimations purposes.
* Twitter DAU (Daily Active Users): ~100 mil
* Lets take 30% write and 70% read traffic for daily users.
* 100 mil translates to roughly 1100 RPS.
* That makes 330 write requests and 770 read requests.
* Average url length: 200 chars
* Unique id length: 6 chars.
* Lets say approximately we need to store 210 chars, with each char being 2 bytes
	- 
=> 210*2 = 420 bytes
Total write data = 30 mil * 420 bytes
=> 12600 * 10^6 bytes => 12600 MB => 12 GB/day
--
 * Monthly storage = 12*30 GB = 360 GB
 * Yearly storage => 12*360 GB => 4320 GB => 4.3 TB








