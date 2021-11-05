# ompl_to_json_search
OPML blog post searches via JSON and MongoDB

## What this is and why

Dave Winer recently noted posted a [Tools for Thought challenge](http://scripting.com/2021/11/02/184243.html?title=aToolsforthoughtChallenge) which got me thinking. His first point in the challenge was this:

*"After I've taken notes on what I'm thinking about, how to find:
The things I wrote about. (Where did I say that?)"*

This has to do partially with the various tools for thought that people use. Data from them should be interchangable, of course.
What if you use different tools for different thoughts though? You still might have related ideas or thought patterns between the data in the those different tools. But you'd never know it.

I've spent a small amount of time (too little, actually) working with JSON data in [MongoDB](https://www.mongodb.com/) for a side project, where I can query for information stored in my database. Maybe there's room for a database to store all of our "thoughts", regardless of which tool those those are in. If so, my little MongoDB experience might come in handy. And I'll learn a little more if I tinker, anyway.

## So I tinkered...

[Dave's blog](http://scripting.com/) is OPML, which unfortunately, MongoDB doesn't quite support. I suppose you could make it work but for a quick and dirty POC, I decided to use JSON, which is exactly what MongoDB supports. And Dave has nightly backups of his site, not just in OPML, but [also in JSON](https://github.com/scripting/Scripting-News/tree/master/blog/items).

So, I grabbed a few of his blog posts in JSON from various dates. And two of the posts I specifically grabbed have the word "product" in them. This way, I could try a MongoDB query to show me the posts with the word "product" in the text field.

![scripting posts in json](https://user-images.githubusercontent.com/16373212/140446771-f554dbb5-32ac-4bdf-b77e-1293e120c890.jpg)

## ...and it worked

It took me multiple attempts to get things just right but after importing the four posts in JSON format (*see item 5 below*), I was able to use MongoDB Compass, which is a GUI connection to the database. Other ways to connect to and query a database are the MongoDB CLI, which is also useful for testing, and of course, just about any standard programming language [through official libraries](https://docs.mongodb.com/drivers/).

![json import results](https://user-images.githubusercontent.com/16373212/140446831-70f9f3b0-9bfb-44eb-902f-8d58f8cca48a.jpg)

In MongoDB-land using Compass, this query returned only the posts with the word "product" in them: 

    {text: { $regex : /product/ } }

![query results for product](https://user-images.githubusercontent.com/16373212/140446865-250c07e9-7fc2-4b45-b9b6-8f63bc490bfc.jpg)

Obviously, I did this manually. but anything that can be done manually in MongoDB can be done programmatically. I simply used the MongoDB GUI to see if this was viable. I took the same approach with my other project using JSON in MongoDB and then coded up the manual steps successfully. So as a POC, I'd say this is a success!

## Next steps

1. Code up the database bits to programmatically query across all JSON objects (i.e.: blog posts).
2. Code up the pushing of blog posts to MongoDB; perhaps this can be done through a GitHub action.
3. Put together a small web front end with a search box and output query results from matching blog posts
4. A simple Node.js server for the front end to call to, which will make the appropriate query calls to the database and also return the results to the front end.
5. Because of how MongoDB works to organize data, I had to put all of the posts in a single JSON file representing an array of objects. That's the top figure in this readme. Dave's current backup to JSON structure is to have one JSON object file per post. I'll have to address this.
6. Probably more that I haven't thought of yet.
