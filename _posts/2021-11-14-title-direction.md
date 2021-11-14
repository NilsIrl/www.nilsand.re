---
title: The ultimate solution to the nightmare of truncated tab titles
category: [programming]
---

### Problem

I think we've all been in this situation: whilst navigating the pages of a
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
their titles so that they are readable when the text in the tab bar is
left-aligned or everyone writes them so that they are readable right-aligned.

After some thoughts I came to the conclusion that it was preferable that all
text inside of tab bars be written such that is was readable when the text was
right-aligned (like for the PostgreSQL website) because it allows the text to be
written in a coherent hierarchy: from general to more specific (like is used
for [dates][6], [paths][7], and [paths][8]) and because the (only?) reason some
websites have the more specific information at the start of their title is
so that it is visible when the right side of the title is truncated.

We still have a problem.

It isn't practical to go against the web's (4.88 billion users) unstoppable
momentum: the standardisation dream would involve having to convince everyone
to rewrite their website titles such that they are readable right-aligned and
convince browser vendors to display titles differently, something that is just
simply impossible and even if it was possible, it wouldn't necessarily be
beneficial.

A second solution would be to have a per site configuration: either allow
websites to indicate how they wish their title to be displayed or have an
external database that is loaded on the client side using a browser extension
or by the browser itself.

Neither solutions seem within reach, the first would require extending some
kind of web standard, the second requires convincing browser vendors (and
effectively extending the web standard) to make some changes as there are
currently no APIs to change text direction from an extension.

### Panacea

Except we don't need to. Everything mentioned so far assumes that there is
currently no way to change the alignment of text in tab bars, but in fact there
is, and I've hinted at it by referring to "text direction", because the
solution is that:

![The tab with Arabic text is right aligned and truncated towards the left][10]

Websites that use [right-to-left scripts][9] (like Arabic and Hebrew), have
the text within title bar right-aligned.

And so this leads us to our solution:

```html
<title>&rlm;Nils's website - The ultimate solution to the nightmare of truncated tab titles</title>
```

With this added to web developer's tool belt, hopefully some day, we will never
have to click on 3 different tabs before finding the want we seek.

All credit goes to my friend [Pranav][pranav], who after I mentioned the
problem to him thought about [right-to-left scripts][9] and then was able to
make it work.

`&rlm;` is one of the ways to have a [right-to-left mark][rlm] in HTML, if you
prefer you can also use `&#8207;`, `&#x200F`, or a [raw UTF-8 character][11].

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
