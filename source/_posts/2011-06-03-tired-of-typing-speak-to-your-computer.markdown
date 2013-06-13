---
author: Peter
comments: true
date: 2011-06-03 16:54:39
layout: post
slug: tired-of-typing-speak-to-your-computer
title: Tired of Typing? Speak to Your Computer!
wordpress_id: 773
categories:
- HTML5
- Speech
- Chrome
teaserimage: /images/2011-06-03-tired-of-typing-speak-to-your-computer/HAL9000_150x150.png

---

For some reason, humans have always dreamt of using natural language to communicate with computers. Quite a number of movies have been made that revolve around this theme, [2001: A Space Odyssey][1] and [I, Robot][2] (named after the [great collection of SF stories][3] by [Isaac Asimov][4] just being two of them.

  [1]: http://en.wikipedia.org/wiki/2001:_A_Space_Odyssey_(film)
  [2]: http://en.wikipedia.org/wiki/I,_Robot_(film)
  [3]: http://en.wikipedia.org/wiki/I_robot
  [4]: http://en.wikipedia.org/wiki/Isaac_Asimov

<!-- more -->
Well, we've come a long way since then and computers are more powerful than ever before. I remember using one of the first versions of [IBM ViaVoice][5] which would quite literally bog down my computer when I tried using it. The quality of speech recognition software has vastly improved and using a clever stack of technology, you can even use speech recognition on your iPhone (the [actual recognition is performed on a server][6], but the effect is stunning nevertheless).

  [5]: http://www.research.ibm.com/hlt/html/body_history.html
  [6]: http://www.tuaw.com/2009/12/08/dragon-dictation-comes-to-the-iphone-wow/

With all the hoopla around HTML 5, it would be quite a surprise if modern browsers didn't have something in store with regard to voice recognition. And sure enough, there is a W3C specification for a [Speech Input API][7]. Looking at the list of authors might give us a hint as to which browser might support this API...

  [7]: http://lists.w3.org/Archives/Public/public-xg-htmlspeech/2011Feb/att-0020/api-draft.html

Using the speech input API is rather easy. All you have to do is to add the `x-webkit-speech` attribute to any `input` tag and you're done. If you're on a speech-enabled browsers (as of this writing, [only Chrome 11][8] supports this out of the box), you can check it out in the input field below. Just click on the microphone icon and start speaking:

  [8]: http://chrome.blogspot.com/2011/04/everybodys-talking-and-translating-with.html

<input name="speechinput" size="40" placeholder="click the mic and start speaking" x-webkit-speech="">

So, the other day I thought, "wouldn't it be cool if I could use voice recognition to look up my contacts on the social networks I am on?". Adding voice recognition support to a website you own is rather easy, as you only have to add the `x-webkit-speech` attribute to the respective input fields. Enhancing foreign sites, however, turns out to be a little bit more involved. Fortunately, Chrome can augment existing websites by way of so-called [Content Scripts][9], which are a part of the [Chrome Extensions API][10].

  [9]: http://code.google.com/chrome/extensions/content_scripts.html
  [10]: http://code.google.com/chrome/extensions/getstarted.html

Writing a Chrome Extension for speech-enabling existing text input fields on just about any website [was a matter of minutes][11], thanks to the good documentation and some [jQuery][12] to walk the DOM. Putting on the finishing touches took me some more time, and I am proud to present you [Speak to Search][13] - a Chrome Extension that lets you talk with your browser. It works with virtually every website that uses regular HTML input fields. By making some smart assumptions, the extension will automatically submit the current form if the input field is a search field. If it is not, the focus will remain in the field and the form will not be submitted. That way, you can fill out e.g. an address form.

  [11]: http://github.com/peterfriese/Speak2Search
  [12]: http://jquery.com/
  [13]: http://chrome.google.com/webstore/detail/peldinpdedgdcbdehomnpfndejpoibeb

Here is a short video of me using Speak to Search to search for some people on Xing and LinkedIn. Please note that the extension is making sure the speech recognition engine is configured to recognize German names on Xing.

{% youtube syMpQqMJcKU %}

Language makes us human - this is a quote from a [video][14] I found during the research for this blog post. I don't necessarily think voice recognition and speech synthesis will make computers more human, but both technologies certainly can help to create a more immersive experience. I am looking forward to seeing a broader use of the new audio capabilities of modern browsers. Feel free to [grab my code from Github][15] and create something new and exciting!

  [14]: http://www.pbs.org/wnet/humanspark/video/spark-blog-video-dr-steven-pinker-language-makes-us-human/212/
  [15]: http://github.com/peterfriese/Speak2Search
