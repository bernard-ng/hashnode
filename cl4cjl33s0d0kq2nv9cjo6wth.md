## How to become a better self-taught backend developer

This article was originally published in French [here](https://devscast.tech/posts/comment-devenir-meilleur-developpeur-backend-autodidacte-31).

A great exercise as a Backend developer is to develop and optimize an application that handles a huge amount of data (in terms of millions). Unfortunately, it is very often difficult to join and learn on this kind of project when you don't have enough experience. 

Question: How do you take on this kind of challenge to learn and think differently about your application development?

## Learn from the greats
All the big tech companies (GAFAM) have an engineer blog, in the articles they publish, they very often explain a problem they encounter, the difficulty and the approach they used to arrive at a solution. 
example:
- [How to count followers case of Medium](https://medium.engineering/counting-your-followers-facbfafe45d9)
- [How to represent search queries case of Pinterest](https://medium.com/pinterest-engineering/searchsage-learning-search-query-representations-at-pinterest-654f2bb887fc)
- [Hermes](https://engineering.fb.com/2019/07/12/android/hermes/)
- etc...

While I admit that it's hard to understand everything, it's extremely beneficial to put yourself in the shoes of a Twitter backend for example and think about how to solve a problem on a large scale.

Being in this position allows you to discover new approaches, new tools and new technologies that you can now use in your applications. Are you still using a SQL query with a LIKE statement to search? will this approach still be the right one when you have 10 million data? even with database indexes, at some point you will reach a limit. and you will have to look for a technology adapted only to search like [Typesens](https://typesense.org/), [elastic search](https://www.elastic.co/)... if you don't ask yourself these questions, you will never discover these tools by yourself, know the limits of the tools you are currently using, their strengths and weaknesses, be able to justify your technical choices. 

## Discover the stacks
Did you know that you can not only read articles but also see what stack a startup is using on platforms like https://stackshare.io/. It's also a way to do monitoring and discovery, the most interesting on stackshare being the justifications in technical choices by CTOs and the community.

## Open source exploration!
Many projects, libraries and frameworks we use today are open source, have you ever taken the time to understand how this or that functionality works in the code you download on almost every project? Keep in mind that it's not a black box and that you can explore and discover new development approaches, while exploring the Symfony source code I discovered concepts like NullObject, DataTransferObject, or the Joda condition... it's true that it's difficult to understand the entirety of a project but starting small and getting used to it can even raise you to the level of a contributor (in terms of code of course)

# Replicate the experiences
The best way to learn how to solve common problems you haven't encountered yet is to reproduce them in a small project and solve them step by step, depending on the technology you are using, some common problems already have universal solutions, for example if you are using an ORM like Doctrine you will probably be confronted to the N+1 problem. understanding in which context this problem occurs and how to solve it taking into account this same context will make you a better backend

You can give yourself an hour to generate data with a library like [Faker](https://fakerjs.dev/), populate your database and start exploring edge cases

## Conclusion
in conclusion becoming a better backend developer will require a conscious commitment with this precise goal in mind, this process can go through the following steps: 

- Learn from the greats
- Knowing the limits of the tools you use
- Being able to justify your technological choices
- Knowing what is done elsewhere
- Exploring opensource code in order to contribute to it
- Reproduce experiences
- explore borderline cases

and finally of course share what you know with the community like [Devscast](https://t.me/devscast)