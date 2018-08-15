---
published: true
---
System Design interviews are less about getting lucky and more about actually doing the hard work of attaining knowledge. Performance in these interviews depends on the following 2 factors—
- Your knowledge — gained either through studying or practical experience.
- Your ability to articulate your thoughts.
- When companies ask design questions, they want to evaluate your design skills and experience in designing large scale distributed systems
- How well you do in such interviews often dictates hiring level/ salary.

_Note_: Most of the contents in this port are a compilation of the references.



### Steps of system design—
1. get a functional system first
    1. Requirement gathering, (Design twitter)
        * Who can post a tweet? (answer: any user)
        * Who can read the tweet? (answer: any user — as all tweets are public)
        * Will a tweet contain photos or videos (answer: for now, just photos)
        * Can a user follow another user? (answer: yes).
        * Can a user ‘like’ a tweet? (answer: yes).
        * What gets included in the user feed (answer: tweets from everyone whom you are following).
        * Is feed a list of tweets in chronological order? (answer: for now, yes).
        * Can a user search for tweets (answer: yes).
        * Are we designing the client/server interaction or backend architecture or both (answer: we want to understand the interaction between client/server but we will focus on how to scale the backend).
        * How many total users are there (answer: we expect to reach 200 Million users in the first year).
        * How many daily active users are there (100 million users sign-in everyday)
    2. System can be hypothetical, may consider basic functionalities. Say twitter dont need to support videos at this point.
2. API/Functionality definition
    1. postTweet(uid, time, location, tweet...)
    1. celebTweet(uid, time, location, tweet...)
    2. generateTimeline(uid, time)
    3. recordLove(uid, tweet_id)
3. Hardware/Resource requirement: Show ur interviewer you understand the scale
    1. Number of tweets/updates.
    2. Storage requirement
    3. Caching requirement
    4. Latency requirement, say we want the _timeline_ to generate fast
    5. BW requirement
    6. Network IO
    7. Firewall
4. Data Model
    1. User
    2. Tweet
    3. userFollows
    4. FavoriteTweets
5. High level design: Generally we think this is the only thing that matters, unfortunately at comes at 5th position here.
    1. Draw high level system diagram 
6. Detailed component design
7. Bottleneck identification and resolve
    1. Reduce Latency
    1. Queuing System, service discipline to select server
    1. Failure tolerance
    1. Bring in server on demand for scalability and increased load
￼

#### Scalability

- Read is cheap write is expensive. 10ms for a disk seek. 
- Batch writes, for faster writing.
- Writes --> disk seek. only 100seeks /second or 10ms per seek
- Reads--> 250ns to read 1 MB
- To learn more on these numbers [Click Here](http://highscalability.com/numbers-everyone-should-know)


#### Design Patterns with examples
- [Java design Patterns](http://www.fluffycat.com/Java-Design-Patterns/)
- A great resource on [adapter design pattern](https://medium.com/@ssaurel/implement-the-adapter-design-pattern-in-java-f9adb6a8828f)

￼A simple design pattern example-
{% highlight java linenos%}
MediaPlayer.java
public interface MediaPlayer {
 void play(String filename);
}

MediaPackage.java
public interface MediaPackage {
 void playFile(String filename);
}

Now, you can create the implementations classes :
MP3.java
public class MP3 implements MediaPlayer {
 @Override
 public void play(String filename) {
    System.out.println("Playing MP3 File " + filename);
 }
}

VLC.java
public class VLC implements MediaPackage {
 @Override
 public void playFile(String filename) {
    System.out.println("Playing VLC File " + filename);
 }
}
{% endhighlight %}


### Funny guy TechLead on Design Patterns
<a href="http://www.youtube.com/watch?feature=player_embedded&v=WV2Ed1QTst8
" target="_blank"><img src="http://img.youtube.com/vi/WV2Ed1QTst8/0.jpg" 
alt="Techlead Design Pattern" width="480" height="360" border="2" /></a>

### Simple twitter design
https://www.hiredintech.com/lecture_materials/twitter_problem_system_design.png![Simple twitter system design from HiredInTech.com](https://www.hiredintech.com/lecture_materials/twitter_problem_system_design.png)


### References:
1. [Anatomy of a system design interview](https://hackernoon.com/anatomy-of-a-system-design-interview-4cb57d75a53f).
1. [A compilation of lots of resources and systems](https://github.com/donnemartin/system-design-primer).
1. [Numbers on scalability](http://highscalability.com/numbers-everyone-should-know).
1. [Vending Machine w/ Design Pattern in Java](https://javarevisited.blogspot.com/2016/06/design-vending-machine-in-java.html).
1. [System Design Overall discussion](https://www.interviewbit.com/courses/system-design/). 
1. [Uber-like ride sharing design](https://sakib.ninja/experimenting-uber-like-application-architecture/). 
1. [Grokking System Design Interview](https://www.educative.io/collection/5668639101419520/5649050225344512).
