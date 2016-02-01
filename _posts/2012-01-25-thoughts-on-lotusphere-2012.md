---
id: 306
title: Thoughts On Lotusphere 2012
date: 2012-01-25T19:51:56+00:00
author: ryanjbaxter
layout: post
guid: http://ryanjbaxter.com/?p=306
permalink: /2012/01/25/thoughts-on-lotusphere-2012/
categories:
  - Connections
  - Lotus
  - Lotusphere
  - Lotusphere 2012
  - Notes
  - OpenSocial
  - Shindig
  - Web Development
---
Is it even possible to summerize Lotusphere 2012?  Probably not, no one person can, but I will summerize it from my perspective  <img src="http://ryanjbaxter.com/wp-includes/images/smilies/simple-smile.png" alt=":)" class="wp-smiley" style="height: 1em; max-height: 1em;" /> Overall it was a great week, I enjoyed it.  As always, it was busy.  This year was the busiest yet (I have only gone 3 times so it is not much of a sample size compared to some.)   Overall I thought it was a good conference.  However, I did not feel the exceitement I had felt in the past, but that may have just been me, as an IBMer I have inside knowledge about things so I see things a bit differently.

&nbsp;

## **Monday**

#### OGS

I thought the OGS was excellent this year, cetainly better than last year.  Michael J Fox was great, he really gave some good perspectives about using technology to help with his foundation.  I liked the format of the OGS as well, perspective followed by demos.  The OGS flew by compared to last year.  I thought there could have been more attention payed to Notes and Domino.  I am not sure whether the content there was rushed because it was at the end of the OGS and they were running out of time, but it just seemed like the demos of that content could have been more in depth.

&nbsp;

#### The Introduction of OpenSocial

At the OGS, IBM announced its support for OpenSocial in Notes and Domino Social Edition, as well as Connections Next.  For more info on Notes and Domino Social Edition, I suggest you read <a href="http://www.edbrill.com/ebrill/edbrill.nsf/dx/lotusphere-2012-lotus-notesdomino-social-edition" target="_blank">Ed Brill&#8217;s blog post on it</a>, he can do a better job explaining exactly what it is.  However the key feature in Notes and Domino Social Eiditon is the support for OpenSocial in Notes and iNotes.  This support allows us to provide embedded experiences, as well as surface an activity stream in Notes and iNotes.  Its a reliefe for me to finally have announced this support because me and my team have essentially been working in secret for the past year and half, and I have not been able to talk about it at all.  However if you are clever you probably noticed IBMs heavy involvement in the specification and implementation projects over the past 2 years and you probably could have guessed what IBM was planning.  Now we can finally talk openly about how we are going to be using OpenSocial and what it means for you, as an IBM customer/buisness partner.

&nbsp;

One of the key uses of OpenSocial in Notes and Domino Social Edition and in Connections Next is embedded experiences.  For those of you that don&#8217;t know, embedded experiences allows you to embed the &#8220;experience&#8221; you would get in one application, in a completley different application.  For example, it allows you to embed the experience from your favorite project management tool into an email, or an entry in an activity stream.  This allows users to stay in context, and not have to jump back and forth between mutliple applications throughout the day.  If you live in your inbox, you can stay there and get more work done without going off to multiple applicaitons throughout the day.  For all of you that say Notes is not social, or email is not social, I disagree.  Embedded experiences is email 2.0 and it allows email to become one of the most powerful and useful forms of communication in the organization!

&nbsp;

The second most important benefit to adopting OpenSocial is its support for <a href="http://activitystrea.ms/" target="_blank">activity streams</a>.  It&#8217;s important to understand that activity streams is its own specification.  However activity streams needs OpenSocial.  The activity streams spec just defines a data model, in other words it just defines what the activity stream should look like, there are no APIs.  OpenSocial puts APIs ontop of this data model, allowing us to access the data.  Connections Next exposes its activity stream through the APIs OpenSocial defines.  In other words, the activity stream in Connections Next uses standards to expose its activity stream!  IBM did not invent its own APIs we adhere to the specification.

&nbsp;

However embedded expriences and activity streams is only the tip of the ice berg when it comes to OpenSocial.  You can build full fledged applications using OpenSocial, and all you need to know is HTML, Javascript, and CSS.  Since Notes, iNotes, and Connections will all be supporting OpenSocial the same application will run accross all three!  Imagine that, build an app once and it works across the portfolio, how cool is that?  It will also be possible to run these applications ouside of IBM products, for example they will eventually work in Jive.  (Once they also support the same version of OpenSocial.)  The amount of reuse you will get from OpenSocial should be the biggest advantage to using OpenSocial gadgets.  The other hidden advantage is the built in social APIs.  Want to get a user&#8217;s network contacts?  There is an API for that.  What to post something to the activity stream?  There is an API for that.  You no longer need to worry about how all this information gets there or where it is coming from, you just call the API and the informaiton is returned to you.  The application (container in OpenSocial terms) takes care of it for you!

&nbsp;

I am really excited to start to get this stuff in the hands of the ICS community, we want your feedback.  We want to make OpenSocial as successful as XPages has been and it has a lot of potential.  Will if be perfect?  No of course not, but it has a good start and you will have a team of IBMers behind it trying to make it as successful as it can be.

&nbsp;

#### Lotusphere University

Once again this year GBS and IBM invited students from universities throughout Florida to spend the day at Lotusphere.  There was about 750 students who atteneded this year.  I thought this was a great opportunity for the students, but I still think the format of the event for the students needs to be tuned a little.  I was asked to have lunch with the students and give a short presentation on OpenSocial to them.  Lunch was great.  Most of them were interested in how I got my job at IBM or how they could get internships.  I only graduated 3 years ago, so I can remember being in their shoes.  My advice to them is always work on your resume and practice your interviewing.  Making a good impression during an interview is key.  I heard from many of them that they thought the OGS was a little boaring.  I think all the business talk means little to them.  They are a long way from being interested in social business, nor do they understand how any of this software would help a business, they are not part of a business, they are college students.  They want to know what they can do to get a job.  That is the bottom line for them, anything from skills they need to learn, to techniques for interviewing, that is what they want to know.  I think letting them come into the labs was a good idea.  They got to talk to and get advice from engineers who know what it takes to succeed.  I think access to profesionals in their field of study is the most excisting thing for them.  Overall though, I hope the program continues and grows, its good to have young blood at the conference.

&nbsp;

## Tuesday

#### Show115 &#8211; Socialize Your Applications With OpenSocial

I could probably write a whole blog post on this session.  I will admit, I was a little nervous about this session.  I was concerned that we were going to be not only talking about but demonstrating technology that was not yet accessible to the audience and therefore they would lose interest.  I guess I was wrong.  I had a lot of possitive feedback from people who attended, I was actually flattered.  Someone even said to me that it was &#8220;the best show n tell they had ever attended.&#8221;  I co-presented with Stanton Sievers, a teammate of mine, and for the most part everything went smoothly.  Practice seems to be key with show n tells, making sure you know every little thing that may go wrong helps.  Even knowing the little details, like what to have open in which browsers when you start the session can go a long way.  The slides from the show n tell are online on the <a href="http://www.socialbizonline.com/confapps/pdf.nsf/0/F742E87014520DD785257988005ADED1/$FILE/SHOW115.pdf" target="_blank">social business online site</a> and on <a href="http://www.slideshare.net/ryanjbaxter/lotusphere-2012-show115-socialize-your-apps-using-opensocial" target="_blank">SlideShare</a>.  I have also <a href="http://dl.dropbox.com/u/58852600/ls12/show115_socialize_your_apps.zip" target="_blank">uploaded a zip file</a> with all the code snippets you will need to follow along.

&nbsp;

&nbsp;

#### Ad115 &#8211; Extending IBM Lotus Notes and IBM Lotus iNotes With OpenSocial

This session also went very well.  We went into more details on OpenSocial and what IBM has contributed to the spec, and we showed plenty of examples on how you can leverage OpenSocial in Notes and iNotes Social Ediiton.  The one critique I receieved, that is completley correct, is we didn&#8217;t show any code  <img src="http://ryanjbaxter.com/wp-includes/images/smilies/frownie.png" alt=":(" class="wp-smiley" style="height: 1em; max-height: 1em;" /> A session in the application development track should show code and we didn&#8217;t.  I appologize for that, it was on oversite on our part.  If you want to see code though please take a look at the the show115 slides/zip.  There is plenty of code in there <img src="http://ryanjbaxter.com/wp-includes/images/smilies/simple-smile.png" alt=":)" class="wp-smiley" style="height: 1em; max-height: 1em;" />

&nbsp;

&nbsp;

## Wednesday

#### Social App Throwdown

If you did not go to this event, than I am sorry for you  <img src="http://ryanjbaxter.com/wp-includes/images/smilies/simple-smile.png" alt=":)" class="wp-smiley" style="height: 1em; max-height: 1em;" /> I thought this was excellent.  My team worked many many many hours on getthing things ready for this session.  There was a lot of work done to get a hosted beta enviornment ready for business partners to start building their apps.  I cannot thank the business partners who participated enough.  MutualMind, GBS, Intravision, SugarCRM, Silanis, and Trilog all gave excellent demos and really built applications that blew us (IBMers and the audience) away.  I think this session also got the point across to the audience of the power of OpenSocial and what it brings to the table.  I had customers, business partners, other IBMers, and analysts all come up and say how excellent the demos were.  The session was recorded, so I hope it will be released so everyone who missed it can watch it.

&nbsp;

&nbsp;