# How to: Recreate HTML

Despite YouTube undoubtedly being one of the most popular websites ever, there is surprisingly a lack of any HTML archive of many parts of the site, mostly parts that require you to log in first.

Whatever your purpose for using old YouTube HTML may be, you are probably interested in getting the HTML for some common feature that for some reason doesn't have one floating out there. Maybe your old YouTube revival or frontend needs the ability to add a video to a playlist from the watch page, for example, and you just cannot find an archive of the page. Or maybe you need the comments tab from Creator Studio...

Recreating HTML is a daunting task and it expects some expertise with the technologies involved.

However! Speaking as the author of this article herself, Taniko Yamamoto, I started off with only a vague knowledge of web development in 2021. I hardly knew the nuances of HTML, and I was just learning CSS and JavaScript. While there is going to be some learning to do, it doesn't necessitate an in depth understanding of the topics.


### Recommended article: [How to: YouTube JS Guide](/howto_yt_js_guide.md)

## The relation between HTML and CSS

Fortunately for us, HTML doesn't really mean anything! Especially since YouTube primarily writes according to the HTML4 specification, which basically means they only used `div`, `span`, and `ul`/`li` for all markup.

This also means that browser styles don't really have to be kept in mind too much. If you need a refresher on the traits of these primitive elements:

- **`div`** is primarily used for elements that break to a new line on the page. This is because, by default, it comes with `display:block` in CSS. Use `div` for most things that need to appear on a new line, under other elements.
- **`span`** is the opposite of this. They are displayed as `inline-block` by default, meaning that they will occupy the same line until they meet an edge, where they will be forced to the next line. Use spans for content that needs to appear on the same line as something.
- **`ul`/`ol`** and **`li`** are primarily used for lists. If the same type of element (menu item, video renderer, etc.) is reused several times in a row, it will most likely have been a `div` or `span` wrapped in an `li` within a `ul`.

## Googling - The hardest part.

Surprisingly, it can be very difficult to find reference screenshots, especially if they pertain to submenus and the like.

Here are a few tips for finding reference images:

- **Don't limit yourself to image search.** YouTube search supports the same date filters as Google search itself. YouTube videos on the topic (i.e. tutorials or old update overviews or people complaining about the change back then) are more likely to contain a specific part that wouldn't be caught in a screenshot you can find on Google Images.
- **Keep your queries simple.** Trying to search for very specific part is likely to yield no results. If you can't find anything, keep simplifying the query and digging deeper into the results.
- **It may take a while until you find anything.** A lot of screenshots will probably regard a popular or even unrelated feature. Google+ comments integration is a big example. It may take a lot of trial and error to find anything. Don't be afraid to dig through related images, as I've found what I'm looking for many times with them.

## CSS - The easy part.

Yeah, you heard me right. One of the most important parts of reconstructing HTML is finding the CSS.

Fortunately, YouTube has made it very easy for us. Unlike their JavaScript code, their production CSS is hardly minified, making it very easy to read once it's unminified.

Along with this, most of YouTube's CSS is archived on the Wayback Machine, making it incredibly easy to find. Moreover, they have unversioned copies of production resources circa 2013 on the static content server, `s.ytimg.com/yt/` (not yts), so some rarer goodies like [`www-copyright.css`](//s.ytimg.com/yt/cssbin/www-copyright.css) can be found there.

For searching the Wayback Machine, I recommend using the CDX API. It can simply be accessed with this query:

https://web.archive.org/cdx/search/cdx?url=s.ytimg.com/yts/cssbin&matchType=prefix&filter=statuscode:200&from=2019&to=2020

You can drop the `from` and `to` parts of the query in order to perform a broad search, or keep them if you need something from a specific time period.

Once you have the CSS, it's going to be a few hours of trial and error to see what class names applied to which elements. I recommend using a browser's inspector for this.

## Conclusion

You're not going to be good at it immediately. It will take quite a bit of trial and error before you can get something good.

Few people are as terminally online and autistic as I am, so they won't have the time to obsess over the ancient source code of a single website.

Since I reckon that the majority of people who will be applying this are either *OYC revival creators* or *userscript creators*, both of whom will have finer control over the behaviour of the recreation, you don't really need to go to extremes. I like doing so, but for many of you, you can just use reference screenshots and remake everything from scratch rather than trying to recreate something that works with YouTube's own CSS and JS.