---
title: The ultimate solution to the nightmare of truncated tab titles
category: [programming]
---

### Problem

I think we've all been in this situation; whilst navigating the pages of a
website, following links by opening new tabs, we ultimately end up with an
unnavigable mess:

![Screenshot of tabs with the right of the title truncated][1]

Or even worse when not using [Tree Style Tab][2]:

![Screenshot of tabs with the right of the title truncated in Chrome][3]

Now luckily for me I use Tree Style Tab which means that I can change the
direction of text inside of tabs (in the preferences) so that I can now see
which pages I'm navigating:

![Screenshot of right to left text with Tree Style Tab][4]

Except this obviously doesn't work for other websites:

![Screenshot of right to left text with Tree Style Tab but the important
information is not visible as it is truncated towards the right][5]

We have a problem. Let's fix it!

It seemed to me like the solution was standardisation, either everyone writes
their titles so that they are readable when the text in the tab bar is left
aligned or everyone writes them so that they are readable right aligned.

After some thoughts I came to the conclusion that it was preferable that all
text inside of tab bars be written such that is was readable when the text was
left aligned (like for the PostgreSQL website) because it allows the text to be
written in a coherent hierarchy: from general to more specific (like is used
for [dates][6], [paths][7], and [paths][8]) and because the (only?) reason some
websites have the more specific information at the start of their title is
so that it is visible when the right side of the title is truncated.

We still have a problem.

It isn't practical to go against the web's (4.88 billion users) unstoppable
momentum. Browsers don't even support displaying text right aligned in the tab
bar (or so I thought) without an extension like Tree Style Tab.

The only solution that could work was to develop a browser extension that would
have a database of websites and display the title differently depending on the
website. If only websites could tell the browser how to display their title.
Extending the HTML5 spec doesn't sound like a great idea.

### Panacea

Except we don't need to. Whilst talking about this problem to my friend
[Pranav][pranav], he mentioned [right-to-left scripts][9] (like Arabic and
Hebrew) and how they manage to display their title in the right direction
within the tab bar. And indeed he was correct:

![The tab with Arabic text is right aligned and truncated towards the left][10]

And so this leads us to our solution:

```html
<title>&rlm;Nils's website - Hidden text in tab title</title>
```

If you're a developer I urge you to consider having a title that has a right to
left direction and if it's already the case, I urge you even more strongly to
make it readable by adding a [right-to-left mark][rlm] at the beginning of your
title. And hopefully if everyone does that, we will never have to click on
three different tabs before we find the one we want.

If you prefer you can use `&#8207;`, `&#x200F`, or a [raw UTF-8 character][11]
in HTML.

[1]: /assets/files/title-direction/tabs_ltr_same.png
[2]: https://github.com/piroor/treestyletab
[3]: /assets/files/title-direction/tabs_ltr_same_chrome.png
[4]: /assets/files/title-direction/tabs_rtl.png
[5]: /assets/files/title-direction/tabs_rtl_same.png
[6]: https://en.wikipedia.org/wiki/ISO_8601
[7]: https://en.wikipedia.org/wiki/URL
[8]: https://doc.rust-lang.org/reference/items/modules.html#module-source-filenames
[9]: https://en.wikipedia.org/wiki/Right-to-left_script
[10]: /assets/files/title-direction/arabic_news.png
[11]: https://unicode-explorer.com/c/200F

[pranav]: https://pranavg.me/
[rlm]: https://en.wikipedia.org/wiki/Right-to-left_mark
