# One language to rule them all

## Change is coming. 

One language to rule them all, one language to code them, One language to bring them all, and in the darkness link them; In the Land of Backend where the shadows lie.

I've previously written how Dart is the [perfect language for writing CLI](dart-the-perfect-cli-language.md) applications using [DCli](https://pub.dev/packages/dcli).

I'm now going to go even further out on a limb and suggest that Dart is the one language that you should be using for just about everything.

{% hint style="info" %}
**Java introduced the concept of write once/run everywhere but never actually delivered on that promise.**
{% endhint %}

It is looking very much like Dart will actually deliver where Java failed.

The Dart/Flutter eco system now allows you to target every major platform:

* Web Applications \*1
* Android
* IOS
* Linx
* Windows
* OSX
* Internet Of things

Whilst a few of the pieces are still experimental, this will change over the next 12-18 months and I fully expect that we will have production ready versions of Dart/Flutter in every major environment.

So why should you care? The answer or rather the problem is language fragmentation within an organisation. It has to be one of the greatest contributors to waste in any IT environment.

Language fragmentation may be justified if:

* Specialised libraries are required that are only available with a specific set of language bindings.
* Specific requirements \(e.g. direct memory access\) dictate a specific type of language is used.
* OS' that dictate a specific language \(iOS and Android\)

Dart significantly reduces the incidence of being forced to choose another language.

Dart's initial focus \(well v2's focus\) has been on reducing language fragmentation for mobile device development. You can now build a single app for Android and iOS with a single language/framework.

That framework \(Flutter\) has now been expanded to support Web,  and Desktop apps for Linux, Windows and Mac OS.  As Dart's reach expands, the need to choose a different language for each problem starts to go away. 

Dart is now moving into the backend with server side tools such as [Conduit](https://pub.dev/packages/conduit) and [Shelf](https://pub.dev/packages/shelf), so now you can undertake full stack development with the Dart language.

{% hint style="warning" %}
Conduit makes it possible to do full stack mobile development with one language
{% endhint %}

Frontend development tends to get all of the attention but in reality most of the fragmentation is occurring on the backend.

It is not unusual for the dev, build, deployment and backend environment of an organisation to run 6+ languages and this really starts to hurt productivity.

{% hint style="info" %}
Most language fragmentation actually occurs on the backend.
{% endhint %}

Language fragmentation results in:

* Higher training costs as staff have to be trained in multiple languages
* Lower expertise levels as staff don’t have time to specialise in one language
* Reduce code sharing
* Increased risk as some languages may be known by only a single or few developers
* Greater complexity in DevOps environments due to multiple languages/development environments needing to be deployed
* Increased security risks due to more packages being installed and having to be updated/monitored for security vulnerabilities
* Reduce mobility of staff between areas of the business
* More bugs leaking through to production. Shared libraries experience greater levels of testing and greater expertise results in fewer bugs being introduced in the first place.
* Critical pieces of code being supported by a single developer

{% hint style="info" %}
A single language reduces an organisation's risk when a key staff member leaves.
{% endhint %}

Now imagine a world in which an organisation could use a single language from the frontend all the way into the shadows of the development and production systems.

Security patches just got a whole lot simpler, training staff is now easier, your team's expertise will increase significantly with the flow on effects of less bugs and less downtime.  

{% hint style="info" %}
It's just the mushroom induced hallucinogenic trip of a Brown Wizard
{% endhint %}

And just so you don't think this is a mushroom induced hallucinogenic trip of a Brown Wizard let me finish with  a brief real world case study.

## **Real World Case Study**

Noojee \(the company I work for\) needed to migrate their entire production environment to a new cloud provider. As part of the migration we  were looking to containerise all of our production components.

This essentially meant that we were going to have to redevelop our entire build, deployment and production tooling.

The existing tooling consisted of a mishmash of languages and libraries including:

Ruby, C, Python, Bash, Go, Java and I'm sure I've forgotten something.

Our core production applications are written in Java with some emerging Flutter Mobile apps.

In total it took about 12 months to migrate our devops environment and resulted in around 140,000 lines of Dart code.  I can't offer a line comparison of the old code \(because I'm too lazy to go and find it all\) but it wouldn't be meaningful as the new production tooling does far more than the old tooling did.

During the process we completely eliminated, C, Python, Bash, Go and Ruby from our environment.

Our main app is still written in Java with no intent of changing that as the ROI simply wouldn't stack up.

We now have a common Dart library that is shared across the production tooling. Previously we had disparate libraries across the languages.

### So what was the development teams experience.

In short, they loved moving to Dart.

We had few problems migrating the code as the reality is that none of the languages were bringing anything unique to the problem they were used to solve.

Dart is simple to use, so getting people up to speed was quick and easy.

The likes of the C and Go which you might expect were used for performance reasons or maybe they needed direct memory access, simply wasn't the case. These tools were easy to port or replace.

The Ruby code was the hardest to migrate, simply because Ruby encourages 'magic' programing practices which makes tracing through the code almost impossible and as the original programmer no longer works for the company it was like extracting gold from a Dwarf.

As this is essentially CLI tooling there was only a small no. of third party libraries utilised by the old code base. Most of the code had lots of call outs to other cli tools which was easy to replicate with DCli.

The team felt they were getting through task much faster, particularly as the project progressed and the sharing of libraries increased.

What is very clear, no one wants to go back to what we had. 

We now have an environment that any of our team can support and we have eliminated specialist pieces of code that only a single developer was able to support.

Because everyone is now working in the same language it's much easier to support each other and exchange ideas and code.

## Conclusion

As of today it is entirely practical to build a full stack application solely using Dart and build out your devops tooling using Dart and the likes of [DCli](https://pub.dev/packages/dcli).

There is no doubt that the Dart eco system still has some gaps but there is enormous momentum building to fill out the entire Dart ecosystem. Before you start a migration check that one of those gaps won't bite you on the arse. 

Achieving a single language across your organisation is a long term objective, particularly if you have large set of pre existing apps, but simplifying your devops environment is entirely practical.

Don't rewrite apps just because there is this new language. If an app needs maintenance then that is the time to considering moving.

You need to get management and team buy in or Dart will become just another language you now have to support :\)

You are still going to have to deal with the White Wizard who occupies the darkest recess of the server room, demanding that he should be allowed to write in Perl. But don't worry, Bilbo is coming and with him the One Language to Link them all.



\*1 Dart/Flutter is still not suitable for what I like to call brochureware sites. Sites where a customer might visit once or twice and load times are important. There are also issues around SEO, which can be overcome, but is somewhat challenging. Dart/Flutter are well suited to Web Applications where an user signs in and an initial download isn't a problem.

