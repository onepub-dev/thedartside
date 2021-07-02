# Data Storage for Flutter

It's 2am and I can't sleep. The rain is pouring down and the wind is howling. Like a lone fisherman searching for a safe harbour here I stand in the middle of the night looking for a way to safely store my App's data.

Flutter has made building mobile apps fun, providing a fluid framework that lets a Developer express their creative urges.  But the creativity stops when it comes time to store your data, for this we need to apply some science.

{% hint style="info" %}
Fortunately for the world, I took up a career in IT and not creative writing.
{% endhint %}

Storing data for your mobile app can be tricky and this fact isn't much helped by the fact that there are so many ways to store your data.

So The Dart Side is going to do a brief tour of some of the alternatives and then we are going to explore one of the more traditional methods and what for most developers should probably be the cornerstone of how they store data.

Because it's 2am, let's have some fun and make some stuff up.

Instead of the more traditional term CRUD we are going to use TIDEL. With a bit of poetic license, it ties in rather nicely with our lone fisherman theme.

### TIDEL

* Transform
* Insert
* Delete
* Edit
* List

For tonight only, TIDEL is an extension of the CRUD concept that we are going to use to more fully describe the data operations we need to perform when building an app.

Let's start by looking at what are probably the three main storage techniques you might use for your Flutter app.

* On Device
* Cloud Service
* Client/Server

The differentiation between Cloud Service and Client/Server is a little arbitrary here. By Cloud Service I mean using a third party supplied and managed data store such as Firebase.

## On Device storage

On-device storage is by far the simplest way to store data for your mobile app. For small amounts of configuration style data you can use a Yaml file, but we are here to talk about data storage rather than configuration storage. 

For local storage, we have two main choices which are essentially a key store or an SQL store. Without trying to get into a debate about the technicalities, a key store is often referred to as a NoSQL store.

### Key Stores

Key stores tend to be quick and fairly simple to use but tend to suffer when it comes to building reports and complex data aggregation. However your typical Flutter App isn't about building reports so a Key Store might just be the thing. Tools like [Hive](https://pub.dev/packages/hive) provide a nice Flutter API which makes storing Dart objects to a Hive store nice and simple.

Define a Hive Person object

```dart
@HiveType(typeId: 0)
class Person extends HiveObject {

  @HiveField(0)
  String name;

  @HiveField(1)
  int age;
}
```

Store and retrieve data from a Hive store.

```dart
var box = await Hive.openBox('myBox');

var person = Person()
  ..name = 'Dave'
  ..age = 22;
box.add(person);

print(box.getAt(0)); // Dave - 22

person.age = 30;
person.save();

print(box.getAt(0)) // Dave 
```

Samples were taken from the Hive README.

The downside to local storage is just that, the storage is local. If your user loses their phone or has multiple devices then they won't be able to access their data.

There are lots of scenarios where we simply don't care about sharing data between devices and in these cases the likes of Hive are a suitable and simple alternative.

You could also consider simply storing JSON objects to the file system. The problem with this technique is that decoding large amounts of JSON is CPU intensive and searching and processing JSON is not very efficient. Except for small amounts of data I would stay away from using JSON as a storage solution.

The Hive site provides some benchmark performance comparisons against SQLite which show that hive is much more performant that sqlite. For small data sets I'm sure these statistics are valid, I'm not so certain the performance differential will hold up with larger data sets and more complex queries.

### SQL Stores

The more traditional approach is to use an SQL store such as SQLite.  SQLite has a number of advantages, it's a traditional SQL database that we all know, love and hate. SQLite is available through packages like [sqflite](https://pub.dev/packages/sqflite) which make it easy to integrate with Flutter.

Having a complete set of SQL commands at your disposal gives you a lot of power over your data. SQLite also supports indexes which can be critical for performance when you need to access data via complex queries or in large tables.

A key difference between the likes of Hive and SQLite are transactions.

For simple data sets you may not need transactions.

Transactions essentially guarantee that when you apply a set of operations, either all operations will be applied or no operations will be applied.

To make this a little more concrete, imagine that you are working as a developer at a bank.

You are building an app to transfer money between accounts. We need to move $10 from Brett's account to Tom's account.

At the start of the transactions, Brett has $30 and Tom has $15.

In total the two accounts contain $45.

1. read the balance of  'Brett' account : $30
2. read the balance of 'Tom' account : $15
3. subtract $10 from  'Brett' account: $20
4. store the new value into 'Brett' account: $20
5. add the dollar value to 'Tom' account: $25
6. store the new value into 'Tom' account: $25

At the end of the transaction Brett should have $20 and Tom should have $25.

Now imagine if your app crashes after step 4.  Brett's account has $20 and Tom's account has $15. The total of the two accounts is: $35.

Without a transaction, you have just lost $10. In the banking world this is real money \(most money is stored electronically not as physical cash\).  Brett's account has had value removed, but that value hasn't been transferred into Tom's account.

This is where a Transaction is critical. If your app crashes after step 4, a Database that supports transactions \(and you use the facility\) will 'rollback' the database to the point before the transaction started. In this case it will be like steps 1-4 never happened.

Even if you are not moving money around you should be using transactions whenever you need to update a set of records that have a relationship and you are changing or creating the relationship.

The sqflite package provides some helpers to map Dart objects to your db as well as letting you write raw SQL statements. This is a skill you should have, but it can be rather painful at times.

SQLite is certainly harder to use than Hive, but is less likely to end in a dead end if your data storage and access needs get more complex.

If you have a large number of related entities, SQLite is a better choice than Hive.

Like Hive, SQLite also suffers from the fact that your data is only stored locally.

## Cloud Storage

Which really leads us on to Cloud Storage.

{% hint style="info" %}
Red clouds at night, sailor's delight. Red clouds in the morning, sailor's warning
{% endhint %}

The core advantage of Cloud Storage over local storage is that data can be shared between a user's devices or if they lose/destroy their device then can regain access to their data via re-registering their app on a new device.

In the Flutter world the most common Cloud Storage solution is Firebase.

I think it's reasonable to say that the Flutter community has a love/hate relationship with Firebase. The API hasn't been great but they tell me it is improving :\)

Cloud Stores like Firebase are typically Key Stores like Hive and have the same advantages/disadvantages as Hive does.

The key advantage with Firebase is that a user's data can be shared between devices which for many apps is a necessity. 

{% hint style="info" %}
Firebase looks after the synchronisation of data between devices.
{% endhint %}

Firestore is also able to help with performance by caching some data locally but it will still have lower performance compared to a local storage solution. Of course, if your device is offline you can end up operating on stale data.

The other thing to consider is pricing. Firebase is cheap to start out with and they have some nice free tiers but costs can skyrocket very quickly if you have a successful app. 

{% hint style="danger" %}
You need to look closely at your cost/revenue model before committing to using Firebase.
{% endhint %}

Given you have decided that you need off device storage then a key advantage of Firebase is that you don't have to worry about the infrastructure.  

Again like Hive, if you have a significant no. of Entities in you data model then Firebase probably isn't the right choice. Unlike Hive, Firebase does support transactions.

The other issue with Firebase is that you still may need to have a server somewhere that allows you to process business logic off the device. If this is case then you are probably better moving to a client/server model so you only have one API to deal with and you can combine business logic with data storage.

You are also going to have finer grain security controls with a client/server solution. 

## Client/Server

So now we get to the meat of what The Dart Side is about.  Using Dart on the server.

A traditional Client/Server model provides the most power and is the most complex to implement of all the options we have looked at.

The Client is your Flutter App and the Server is the backend service\(s\) that your Flutter App relies on to implement TIDEL.

A Client/Server model allows you to store your data in the cloud but also provides the ability to Transform your data off-device, providing a complete TIDEL implementation without the processing limitations of your typical mobile phone.

When people talk about Full Stack Development they are usually referring to using a Client/Server model.



![](.gitbook/assets/client-server%20%281%29.png)

### Scaling

Depending on the size of your user base you may need to consider how to scale your app as part of your design process.

{% hint style="info" %}
The database is often the primary bottleneck when it comes to application scalability.
{% endhint %}

One advantage with on-device storage is that you don't have to think about scaling. The same goes for using Firebase. Of course the problem with Firebase is that the costs will be unpredictable \( a surge in user numbers can result in a rather big surprise billing wise\).

Serverless goes some way to solving the scaling issue but again you can end up suffering from bill shock and it will almost always be much more expensive than rolling your own. I'm not going to go into Serverless here as the primary focus of this article is on storage. Perhaps a discussion for another day.

The two core methods of scaling are Horizontal and Vertical which I touch on below. If your decide to go down the client/server path then you do need to consider what type of scaling will be required as part of your design. Using a stateless server \(e.g. REST\) provides the most flexibility for scaleout and allows you to leave most of the scaling decisions until later in the development process.

{% hint style="info" %}
Vertical scaling is easier and cheaper than Horizontal scaling but will only take you so far.
{% endhint %}

Whilst the following talks about scaling at the Web Server and the Application Server you are most likely going to have to scale out the DB first. Horizontal and Vertical scaling works at the DB level as well as the application level.

### Web Server

The Web Server is typically an Apache or Nginx Web Server which sits in front of your Application Server.

You should use the Web Server to serve up all static content.

As your user base increases you can use the Web Server to [load balance](https://docs.nginx.com/nginx/admin-guide/load-balancer/http-load-balancer/) requests across multiple instances of your Application Server \(Horizontal scaling\).

If you develop your backend with a traditional stateless server \(REST\) then you can start with Vertical scaling and then move to Horizontal scaling. If your server isn't stateless then Horizontal scaling gets harder to implement \(read about [sticky sessions](https://docs.nginx.com/nginx/admin-guide/load-balancer/http-load-balancer/#enabling-session-persistence)\).

### Application Server

The Application Server might be written in Java, PHP, Ruby or Node JS, but this blog is about writing one in Dart.

You can do Vertical scaling at the Application Server. This essentially means adding more CPU/Memory or faster disks.  Vertical scaling is the cheapest method of scaling and if you are building an app that is targeting internal users of an Enterprise this will normally be sufficient. 

You can also do vertical scaling by separating each piece of the server onto different VMs.

e.g. Web Server, Application Server and DB all run on a separate VM.

For most applications the Database will be the primary bottleneck so look at how you can scale your db.

### Database 

Using a traditional Database for storage has a number of significant advantages. Whilst your focus might be on building a Flutter app, the reality is that you are going to need to manage your user base, report on activity and provide general support to your users. 

{% hint style="info" %}
You are going to need to build reporting tools and SQL Databases have the best 3rd party support for reporting.
{% endhint %}

To do these things you are going to need access to the user's data. A traditional SQL database is still by far the best tool for managing user data and will provide the easiest and cheapest path to building management and reporting tools for your application.

Some of the questions that come up with the above model are:

1. **Why can't my Flutter App talk directly to the Database.** Well technically it can, but you shouldn't.  The first problem is security. If your Flutter App is going to connect to the database then it will need an account on the database with a username and password.  Most database permission models are fairly simplistic. You can say that a user has access to a given table \(tblAccount\) but you can't say that a user only has access to their own rows in the table. This means that any of your Flutter users can potentially see the full set of accounts stored in tblAccount. For the Flutter App to access the db, it must either store the u/p as an asset/string in the app or you must ask the user to enter the u/p. If it's stored in your app then it's a fairly simple process to extract those details from the app binary. Either way this is considered a completely insecure method of providing access to your db.  The next problem is connection limits. DB's aren't really designed to accept 10s of thousands of short lived connections as each connection uses a large amount of memory and establishing a new connection is time consuming. On the other hand, Application Servers are designed to handle lots of short lived connections.  
2. **Why do I need a Web Server?** Many Application Servers can serve up most of the traffic that a Web Server can, it's just that Web Servers tend to be more highly optimised to serve static content such as images so it's usually better to have the Web Server serve your static content.  Web Servers also tend to do a better job with providing a HTTPS connection. Your web server is likely to support the latest web standards \(e.g. HTTP3\) well before your Application Server does. As your customer base grows you may also need to scale out your infrastructure. Web Servers can support load balancing \(Horizontal scaling\) which allows you to run multiple Application Servers behind a single Web Server and the Web Server will automatically spread requests from your Flutter App amongst your Application Servers.  Noojee's [Nginx-LE](https://pub.dev/packages/nginx_le) is an example of a Web Server, based on Nginx, that provides encryption and management tools written in Dart. Nginx-LE makes for a quick and easy setup for Dart developers with  built-in support for the Dart Web Application Server [Conduit](https://pub.dev/packages/conduit).
3. **What does an Application Server do anyway?** For the answer read on.

## Dart Application Server

So finally we get back to talking about Dart.

An application server is where you write your Application business logic in your language of choice.

Your Flutter App will send a request to the Web Server over HTTPS. The Web Server will then connect to your Application Server via HTTP. There is no need to use a secure connection between the Web Server and the Application Server provided the Application Server is behind your firewall and data is only transmitted within your secure network.

{% hint style="info" %}
The Web Server will likely maintain a pool of already established connections to your Application Server to reduce the overheads of establishing a new connection for each request.
{% endhint %}

The Application Server accepts HTTP requests from your Flutter App \(via the Web Server\).

The Application Server may apply transformations on the data sent and then store/retrieve data from the database as per your Flutter App's request.

There are a number of ways you can format your request when you send data to your Application Server .

[REST](https://www.ibm.com/cloud/learn/rest-apis) is probably the most common method but there are others such as GraphQL. 

REST was designed as a protocol that is intimately bound to HTTP and uses standard HTTP verbs such as POST , DELETE, PUT, GET,   to implement TIDEL actions. Most often the data is exchanged using JSON but you should look at the likes of [protobuf](https://developers.google.com/protocol-buffers/docs/reference/dart-generated) if you are concerned about Jank caused by encoding/decoding JSON objects.

{% hint style="info" %}
Protobuf provides significant performance advantages over JSON. A 60ms decode of data encoded in JSON takes about 12ms when protobuf is used. You just went from Jank to no Jank.
{% endhint %}

The Application Server in turn maintains a pool of database connections ready to retrieve, delete or store data into.

Placing your business logic within your Application Server is usually faster and more secure than performing those tasks on the user's device, but there are exceptions to this rule.

### Security

Remember that you often need to implement security twice. Once in the client and once in the server.

If you have user access controls such as 'what users can add a new account' then you need to implement the permissions on both the client and the server.

On the client you might disable the 'Add' button on the account screen. This however is not sufficient to keep your application secure. A user may be able to hack your app and directly use your Server API. In this case a disabled 'Add' button won't stop them sending a request to add a new account.

This is why you must implement security on the client and the server. If you are going to be lazy then skip the work on the client, but never skip the work on the server.  Your server must be bulletproof.

### Scaling

As noted, your database is the component most likely to suffer from performance issues. There are certainly exceptions to this such as if you are processing large images, running AI systems etc. But for the most common workloads the Db is going to be the core problem.

#### Indexes

The first place to start is ensuring that you have the right indexes on your tables.

Every table should have an integer primary key with an associated index.

You should always try to join tables by the primary key.

Too few indexes is a problem as is too many.

Indexes slow down inserts, updates and deletes. So the more indexes you have the slower these operations will be.

Indexes speed up select statements. You need to understand what indexes your queries need. Look at the join statements, the where clauses and the order by clauses to work out what indexes you need.

If you have a where clause with three 'and' clauses then you need a compound index with the same three columns used in the where clause. The where clause MUST use the columns \(the and clauses\) in the same order as the index declares the columns.

Most DBs can use a two column index even if the where clause has three 'and' clauses. So you don't necessarily need an index for every where clauses.

If your tables are small \(less than 100 rows\) then you may not need an index at all.

Must DB's offer query analysis tools to help you identify what indexes are needed. Learn how to use these?

Most DB's will provide reporting on slow queries. Check your slow query logs on a regular basis and experiment with different indexes.

Remember that index will perform differently based on the size of the DB, so doing tests on a tiny DB when your production DB is huge will not be valid.

## Horizontal and Vertical Scaling

As with your application server Vertical Scaling is always cheaper and easier but Horizontal Scaling will take you further.

Vertical Scaling of a DB involves:

* adding more CPU
* adding more memory \(always a good starting point\).
* adding more/faster disks.

Memory is of the cheapest starting point. DB's do heavy caching so more memory will always help. Check that your DB is configured to use the additional memory!

Moving from spinning rust to SSD will make a huge difference but moving to RAID can add even more performance.  I'm not going to go into the nitty gritty of RAID here as there are lots of articles on the topic. RAID 10 is usually a good starting point for a DB.

Horizontal scaling of a DB is quite challenging, there are all kinds of issues such as allocation of primary keys that you will need to deal with. If you think you might need to horizontally scale your DB you should start reading before you start developing.

For enterprise apps you will likely get away with Vertical scaling.  You should however remember that Horizontal Scaling a DB also improved your redundancy \(the same is true for the Application Server\) so you should look at exploring what your users expect in terms of up time before you rule Horizontal scaling out.



## TO BE CONTINUED....

And here we will pause for a moment. In the next blog we are going to look at the details of implementing an Application Server using the Dart Application server [Conduit](https://pub.dev/packages/conduit).

















