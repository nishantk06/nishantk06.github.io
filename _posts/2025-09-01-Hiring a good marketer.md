---
layout: post
categories: blog
title: Hiring a gooood marketer
published: false
---

It's no secret anymore, atleast in B2B SaaS marketing, that both Google and OpenAI love Reddit. As a marketer, that means SEO doesn't end at blog posts for me. I have to be where the users are. So I learned how to get really, really good at Reddit marketing. 

The story starts with leaving comments. Often, especially in my domain, there were a lot of people asking for what we built on Reddit. Then there were people who found the original post trying to see if someone answered the question. A lot of times, kind anonymous users leave useful responses. A lot of other times, brands can come in to help!
 
So I tried a few experiments, leaving a few comments at a time on relevant posts. And then, tring tring, we got a B2B customer who only found us via Reddit. It seemed as though this experiment could scale. So I knew I had to come up with a systematic way of tracking Reddit. 

There are multiple problems to fix here:

1. Finding and commenting on all Reddit posts relevant to our domain in the hope that some will get us traffic is tedious and inefficient. 
2. There's no easy way to track if we have commented on posts that are showing up on search. The marketing ecosystem around Reddit is quite nascent.
3. New conversations are happening all the time that are relevant to our business but we don't have a simple way of monitoring them. 

#1: How to find high-impact posts

This whole effort was built on the idea that the user journey now looked like Google > Reddit > Solution. So I only needed to care about Reddit posts that showed up on search.

That then was the way to narrow it down. With Ahrefs (or Semrush or any of the others), we had visibility on which links from reddit.com were performing well on search. Filter by keywords relevant to you and voila: a list of reddit posts that are 1. asking about problems your business solves and 2. ranking for queries with other people asking the same question on search/LLMs and hoping reddit has the answer. 

{% comment %}
Here's what a sample output would look like for a CRM company:

Notice the tens of results and notice that with global marketing teams, these companies have still not caught on to the fact that their customers are here and they can leave comments. 
{% endcomment %}

#2: How to automate the commenting

I'm not going to spend my precious time writing manual comments to each reddit post, and there are a lot of them. The interns get bored of it too, and with LLMs being as good as they are, this is a task ripe for automation. 

Now, the Ahrefs data can be easily exported as a csv, so it can be made into a Google Sheet. Once all the Reddit URLs that rank on search are on a sheet, I need to generate a column with the ideal comment. I can get an LLM running in the Sheet but it needs more than the post URL to come up with a meaningful comment.

Thankfully, GPT wrote me an App Script that takes the Reddit URL as input and uses the public Reddit API to fetch the title and description of the post. One shot running the script, and the content is on the sheet. Now we have enough data. I can pass it to the Gemini API (because it's free) and get the ideal comment with a healthy mix of being helpful and promoting my business. 

There we go. With that script, one click of a button and Reddit comments are generated on the sheet beside the URL they should be posted on. All that's left is actually posting it. 

I think the best way to do this is to have people in the team do it through their Reddit accounts, as long as they're willing. We can disclose that we're part of the company we're talking about, but it still comes across as more genuine than a company handle posting. That just seems like an ad. So the sheet has to have a field with the member of the team who will make the post. The content is already there. They just have to paste it and click "comment".  

We might be able to automate this part as well, but I'm scared of getting banned as a bot. Feels safer to keep this manual for now. 

Part #3: How to stay current by getting alerted on newly-indexed posts
Part #4: Reddit comment ranking: Coming up on top
(Still figuring it out. Will update once done.)