# bitrap

A single column, minimal style, [Bootstrap] based theme for [Hexo].
Also the default theme for [Hexi].

## Install

Execute the following command and modify `theme` in `_config.yml` to `bitrap`.

```
git clone git://github.com/kywk/hexo-theme-bitrap.git themes/bitrap
```

## Update

Execute the following command to update bitrap.

```
cd themes/bitrap
git pull
```

## Config

Default config:

``` yaml
menu:
  Home: /
  Archives: /archives
  Submenu:
    item1:  //submenu.item1.url
    item2:  //submenu.item2.url

extmenu:
  Archives: /archives
  Submenu:
    item1:  //submenu.item1.url
    item2:  //submenu.item2.url

excerpt_link: Read More

addthis:
  enable: true
  pubid:
  facebook: true
  twitter: true
  google: true
  pinterest: true

fancybox: true

google_analytics:
rss:
```

- **menu** - Main navigation menu
  - **submenu** - Dropdown menu
- **extmenu** - Extended navigation menu, also support submenu
- **excerpt_link** - "Read More" link text at the bottom of excerpted articles
- **twitter** - Twitter widget config
  - **username** - Twitter username
  - **show_replies** - Enable displaying replies
  - **tweet_count** - Tweets display in widget
- **addthis** - Share buttons at the buttom of articles (Powered by [AddThis])
  - **enable** - Enable share buttons
  - **pubid** - Profile ID of [AddThis]
  - **facebook** - Enable Facebook button
  - **twitter** - Enable Twitter button
  - **google** - Enable Google+ button
  - **pinterest** - Enable Pinterest button
- **fancybox** - Enable [Fancybox]
- **google_analytics** - Google Analytics ID
- **rss** - RSS subscription link (change if using Feedburner)

## Features


### Fancybox

![](http://i.minus.com/iHv7h7rZNqHvo.PNG)

[Hexo]: http://zespia.tw/hexo/
[Hexi]: https://github.com/kywk/hexi/
[Bootstrap]: http://twitter.github.io/bootstrap/
[AddThis]: https://www.addthis.com
[Fancybox]: http://fancyapps.com/