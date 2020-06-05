# KareBot

Kare uses a webcrawler to import webpages from third party solutions which have noot been integrated using the APIs. 
Once added to the list of resources to be imported the target website will receive requests from out bot.


Webmasters can identify the KareBot by looking for the following signature: 

```KareBot/0.2 (https://developer.karehq.com/bot)```


# Robots.txt guidelines

The KareBot does not need to index you whole website. In fact it would be better to just esclude all pages which do not
contain knowledge that should be indexed.


Knowledge managers can configure the behviour of the KareBot from the control panel of the Kare cosole. 


Webmaster can apply robots rules by targeting the `KareBot` agent. For instance with the following exampe a webmaster could only allow articles and faqs while blockign everythign else. 

```
User-agent: KareBot
Disallow: /
Allow: /articles
Allow: /faq
```
