---
title: "Markdown Template"
keywords: template, markdown, markdown template
date: 2019-11-08 18:26:39 -0800
last_updated: December 26, 2020
tags: [miscellaneous]
summary: "It's very easy to automate decorations with Markdown. That's why I primarily use markdown for this website. I
am selectively expanding on advanced Markdown features here."
sidebar: none
permalink: markdown_template.html
folder: mydoc
---

It's very easy to automate decorations with Markdown since it cleverly adopts a <font face="Lora">WYSIWYG (what you see
is what you get)</font> philosophy, rendering webpages' source files uncluttered. I am selectively expanding on advanced
Markdown features here.

## Latex Formulas with MathJax
Configure MathJax module between script tags in HTML files under `_layouts` folder, and insert `set mathjax: true` in
`_config.yml` or in individual posts where you'd like to enable Latex functionality.

MathJax supports two Latex modes. The <font face="Lora">inline mode</font> embeds mathematical formulas
$\lim_{n=0}^\infty x_n$ into text lines in paragraphs, and the <font face="Lora">display mode</font> allocates dedicated
text areas and centralizes full-featured Latex formulas.

$$
\begin{array}{l}
    {\text { Recall the exponential family form of the Bernoulli distribution }(6.113 \mathrm{d}),} \\
    {\qquad p(x | \mu)=\exp \left[x \log \frac{\mu}{1-\mu}+\log (1-\mu)\right]} \\ 
    {\text { The canonical conjugate prior therefore has the same form }} \\ 
    {\qquad p\left(\mu | \gamma, n_{0}\right)
    =\exp \left[n_{0} \gamma \log \frac{\mu}{1-\mu}+n_{0} \log (1-\mu)-A_{c}\left(\gamma, n_{0}\right)\right]}
\end{array}
$$

## Footnotes
A footnote starts with a caret and an identifier inside brackets[^1] which support multiple paragraphs and code by
indentation[^bignote]. The content of the footnote also starts with a caret and its identifier inside brackets succeeded
by a colon and text. You don’t have to put footnotes at the end of the document. However, the GitHub-flavored Markdown
parser does <font face="Lora">not</font> support footnotes and stacks them at the bottom of the file, in the order of
their appearance.

[^1]: This is the first footnote.  
[^bignote]: Here's one with multiple paragraphs and code.  
    Indent paragraphs to include them in the footnote.  
    `{ my code }`  
    Add as many paragraphs as you like.  

## Task Lists
To create a task list, add dashes (-) and brackets with a space ([ ]) in front of task list items. To select a checkbox,
add an x in between the brackets ([x]).

{% highlight markdown %}
- [x] Write the press release
- [ ] Update the website
- [ ] Contact the media
{% endhighlight %}

The rendered output looks like this:

- [x] Write the press release
- [ ] Update the website
- [ ] Contact the media

## Gists
We can use <font face="Lora">gists</font> to embed GitHub-flavored code blocks that appear exactly as in GitHub repos.
To do so, we sign in and create a gist in [GitHub's gist pages](#https://gist.github.com), copy its unique id, and
follow the instructions dictated by [Jekyll-gist gem's website](#https://github.com/jekyll/jekyll-gist).

{% gist bdd62be479ea2d7ef30821772cdf8af9 %}

## Tips, Notes, Warnings, and Important Alerts
These tips, notes, warnings, and important alerts make use of <font face="Lora">Bootstrap</font> styling and are
available through data references, a remarkable utility provided by the documentation theme, not by Jekyll.

{% include tip.html content="For a better terminal emulator on Windows, use
[Git Bash](https://git-for-windows.github.io/). Git Bash gives you Linux-like control on Windows." %}

{% include note.html content="If you're cloning this theme, you're probably writing documentation of some kind. I have a
blog on technical writing here called <a alt='technical writing blog' href='http://idratherbewriting.com'>I'd Rather Be
Writing</a>. If you'd like to stay updated with the latest trends, best practices, and other methods for writing
documentation, consider <a href='https://tinyletter.com/tomjoht'>subscribing</a>. I also have a site on
<a href='http://idratherbewriting.com/learnapidoc'>writing API documentation</a>." %}

{% include warning.html content="Don't make edits both online using Github's browser-based interface AND offline on your
local machine using your local tools. When you try to push from your local, you'll likely get a merge conflict error.
Instead, make sure you do a pull and update on your local after making any edits online." %}

{% include important.html content="This is my important info." %}

## Troubleshoot
### HTML and Markdown Syntax (In)Compatibilities
The `<font>...</font>` HTML5 tags might introduce unexpected troubles when used in combination with Markdown. An obvious
workaround might be to use `[<font>text</font>](#link)` for links, not to start newlines with `<font>...</font>`
(you might put an unprintable character such as `&#000;` in front), and not to embed `<font>...</font>` in between the
`<center>...</center>` tags (you might put multiple space characters `&nbsp;` in front).

## References
[The Markdown Guide](https://www.markdownguide.org/)

[Markdown Guide Book](https://github.com/mattcone/markdown-guide-book)

{% include links.html %}
