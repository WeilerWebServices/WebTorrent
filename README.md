![WebTorrent Icon](WebTorrent.png)

WebTorrent is a streaming torrent client for node.js and the browser. YEP, THAT'S RIGHT. THE BROWSER. It's written completely in JavaScript -- the language of the web -- so the same code works in both runtimes.

In node.js, this module is a simple torrent client, using TCP and UDP to talk to other torrent clients.

In the browser, WebTorrent uses WebRTC (data channels) for peer-to-peer transport. It can be used without browser plugins, extensions, or installations. It's Just JavaScript™. Note: WebTorrent does not support UDP/TCP peers in browser.

Simply include the [`webtorrent.min.js`](https://cdn.jsdelivr.net/npm/webtorrent@latest/webtorrent.min.js) script on your page to start fetching files over WebRTC using the BitTorrent protocol, or `require('webtorrent')` with [browserify](http://browserify.org/). See [demo apps ](https://www.npmjs.com/package/webtorrent#who-is-using-webtorrent-today)and [code examples](https://www.npmjs.com/package/webtorrent#usage) below.

To make BitTorrent work over WebRTC (which is the only P2P transport that works on the web) we made some protocol changes. Therefore, a browser-based WebTorrent client or "web peer" can only connect to other clients that support WebTorrent/WebRTC.

To seed files to web peers, use a client that supports WebTorrent, e.g. [WebTorrent Desktop](https://webtorrent.io/desktop), a desktop client with a familiar UI that can connect to web peers, [webtorrent-hybrid](https://github.com/webtorrent/webtorrent-hybrid), a command line program, or [Instant.io](https://instant.io/), a website. Established torrent clients like Vuze have [already added WebTorrent support](https://wiki.vuze.com/w/WebTorrent) so they can connect to both normal *and* web peers. We hope other clients will follow.

[![Network](https://camo.githubusercontent.com/21fe77acadaba6f57b12d0d89551b07c9253e06589f16dd9554c0de6547379b1/68747470733a2f2f776562746f7272656e742e696f2f696d672f6e6574776f726b2e706e67)](https://camo.githubusercontent.com/21fe77acadaba6f57b12d0d89551b07c9253e06589f16dd9554c0de6547379b1/68747470733a2f2f776562746f7272656e742e696f2f696d672f6e6574776f726b2e706e67)

### [](https://www.npmjs.com/package/webtorrent#features)Features

-   Torrent client for node.js & the browser (same npm package!)
-   Insanely fast
-   Download multiple torrents simultaneously, efficiently
-   Pure Javascript (no native dependencies)
-   Exposes files as streams
    -   Fetches pieces from the network on-demand so seeking is supported (even before torrent is finished)
    -   Seamlessly switches between sequential and rarest-first piece selection strategy
-   Supports advanced torrent client features
    -   magnet uri support via [ut_metadata](https://github.com/webtorrent/ut_metadata)
    -   peer discovery via [dht](https://github.com/webtorrent/bittorrent-dht), [tracker](https://github.com/webtorrent/bittorrent-tracker), [lsd](https://github.com/webtorrent/bittorrent-lsd), and [ut_pex](https://github.com/fisch0920/ut_pex)
    -   [protocol extension api](https://github.com/webtorrent/bittorrent-protocol#extension-api) for adding new extensions
-   Comprehensive test suite (runs completely offline, so it's reliable and fast)

#### [](https://www.npmjs.com/package/webtorrent#browserwebrtc-environment-features)Browser/WebRTC environment features

-   WebRTC data channels for lightweight peer-to-peer communication with no plugins
-   No silos. WebTorrent is a P2P network for the entire web. WebTorrent clients running on one domain can connect to clients on any other domain.
-   Stream video torrents into a `<video>` tag (`webm (vp8, vp9)` or `mp4 (h.264)`)
-   Supports Chrome, Firefox, Opera and Safari.

[![Sauce Labs](https://camo.githubusercontent.com/d7dd2acf4bb7744f304321bef8a13d0303b9f7bf8bddcaf607cb9cd6a47f7b0f/68747470733a2f2f73617563656c6162732e636f6d2f62726f777365722d6d61747269782f776562746f7272656e742e737667)](https://saucelabs.com/u/webtorrent)

### [](https://www.npmjs.com/package/webtorrent#install)Install

To install WebTorrent for use in node or the browser with `require('webtorrent')`, run:

```source-shell
npm install webtorrent
```

To install a `webtorrent` [command line program](https://github.com/webtorrent/webtorrent-cli), run:

```source-shell
npm install webtorrent-cli -g
```

To install a WebTorrent desktop application for Mac, Windows, or Linux, see [WebTorrent Desktop](https://webtorrent.io/desktop).

### [](https://www.npmjs.com/package/webtorrent#ways-to-help)Ways to help

-   Join us in [Gitter](https://gitter.im/webtorrent/webtorrent) or on freenode at `#webtorrent` to help with development or to hang out with some mad science hackers :)
-   [Create a new issue](https://github.com/webtorrent/webtorrent/issues/new) to report bugs
-   [Fix an issue](https://github.com/webtorrent/webtorrent/issues?state=open). WebTorrent is an [OPEN Open Source Project](https://github.com/webtorrent/.github/blob/master/CONTRIBUTING.md)!

### [](https://www.npmjs.com/package/webtorrent#who-is-using-webtorrent-today)Who is using WebTorrent today?

[Lots of folks!](https://github.com/webtorrent/webtorrent/blob/HEAD/docs/faq.md#who-is-using-webtorrent-today)

### [](https://www.npmjs.com/package/webtorrent#webtorrent-api-documentation)WebTorrent API Documentation

[Read the full API Documentation](https://github.com/webtorrent/webtorrent/blob/HEAD/docs/api.md).

### [](https://www.npmjs.com/package/webtorrent#usage)Usage

WebTorrent is the first BitTorrent client that works in the browser, using open web standards (no plugins, just HTML5 and WebRTC)! It's easy to get started!

#### [](https://www.npmjs.com/package/webtorrent#in-the-browser)In the browser

##### [](https://www.npmjs.com/package/webtorrent#downloading-a-file-is-simple)Downloading a file is simple:

```source-js
var WebTorrent = require('webtorrent')

var client = new WebTorrent()
var magnetURI = '...'

client.add(magnetURI, function (torrent) {
  // Got torrent metadata!
  console.log('Client is downloading:', torrent.infoHash)

  torrent.files.forEach(function (file) {
    // Display the file by appending it to the DOM. Supports video, audio, images, and
    // more. Specify a container element (CSS selector or reference to DOM node).
    file.appendTo('body')
  })
})
```

##### [](https://www.npmjs.com/package/webtorrent#seeding-a-file-is-simple-too)Seeding a file is simple, too:

```source-js
var dragDrop = require('drag-drop')
var WebTorrent = require('webtorrent')

var client = new WebTorrent()

// When user drops files on the browser, create a new torrent and start seeding it!
dragDrop('body', function (files) {
  client.seed(files, function (torrent) {
    console.log('Client is seeding:', torrent.infoHash)
  })
})
```

There are more examples in [docs/get-started.md](https://github.com/webtorrent/webtorrent/blob/HEAD/docs/get-started.md).

##### [](https://www.npmjs.com/package/webtorrent#browserify)Browserify

WebTorrent works great with [browserify](http://browserify.org/), an npm package that lets you use [node](http://nodejs.org/)-style require() to organize your browser code and load modules installed by [npm](https://www.npmjs.com/) (as seen in the previous examples).

##### [](https://www.npmjs.com/package/webtorrent#webpack)Webpack

WebTorrent also works with [webpack](https://webpack.js.org/), another module bundler. However, webpack requires the following extra configuration:

```source-js
{
  target: 'web',
  node: {
    fs: 'empty'
  }
}
```

Or, you can just use the pre-built version via `require('webtorrent/webtorrent.min.js')` and skip the webpack configuration.

##### [](https://www.npmjs.com/package/webtorrent#script-tag)Script tag

WebTorrent is also available as a standalone script ([`webtorrent.min.js`](https://github.com/webtorrent/webtorrent/blob/HEAD/webtorrent.min.js)) which exposes `WebTorrent` on the `window` object, so it can be used with just a script tag:

```text-html-basic
<script src="webtorrent.min.js"></script>
```

The WebTorrent script is also hosted on fast, reliable CDN infrastructure (Cloudflare and MaxCDN) for easy inclusion on your site:

```text-html-basic
<script src="https://cdn.jsdelivr.net/npm/webtorrent@latest/webtorrent.min.js"></script>
```

##### [](https://www.npmjs.com/package/webtorrent#chrome-app)Chrome App

If you want to use WebTorrent in a [Chrome App](https://developer.chrome.com/apps/about_apps), you can include the following script:

```text-html-basic
<script src="webtorrent.chromeapp.js"></script>
```

Be sure to enable the `chrome.sockets.udp` and `chrome.sockets.tcp` permissions!

#### [](https://www.npmjs.com/package/webtorrent#in-nodejs)In Node.js

WebTorrent also works in node.js, using the *same npm package!* It's mad science!

NOTE: To connect to "web peers" (browsers) in addition to normal BitTorrent peers, use [webtorrent-hybrid](https://github.com/webtorrent/webtorrent-hybrid) which includes WebRTC support for node.

#### [](https://www.npmjs.com/package/webtorrent#as-a-command-line-app)As a command line app

WebTorrent is also available as a [command line app](https://github.com/webtorrent/webtorrent-cli). Here's how to use it:

```source-shell
$ npm install webtorrent-cli -g
$ webtorrent --help
```

To download a torrent:

```source-shell
$ webtorrent magnet_uri
```

To stream a torrent to a device like AirPlay or Chromecast, just pass a flag:

```source-shell
$ webtorrent magnet_uri --airplay
```

There are many supported streaming options:

```source-shell
--airplay               Apple TV
--chromecast            Chromecast
--mplayer               MPlayer
--mpv                   MPV
--omx [jack]            omx [default: hdmi]
--vlc                   VLC
--xbmc                  XBMC
--stdout                standard out [implies --quiet]
```

In addition to magnet uris, WebTorrent supports [many ways](https://github.com/webtorrent/webtorrent/blob/HEAD/docs/api.md#clientaddtorrentid-opts-function-ontorrent-torrent-) to specify a torrent.

### [](https://www.npmjs.com/package/webtorrent#modules)Modules

Most of the active development is happening inside of small npm packages which are used by WebTorrent.

#### [](https://www.npmjs.com/package/webtorrent#the-node-way)The Node Way™

> "When applications are done well, they are just the really application-specific, brackish residue that can't be so easily abstracted away. All the nice, reusable components sublimate away onto github and npm where everybody can collaborate to advance the commons." --- substack from ["how I write modules"](https://gist.github.com/substack/5075355)

[![node.js is shiny](https://camo.githubusercontent.com/328e6333f6a787e999a9ad7483ce5f9a9100b7ade4df13d125d4ce18f7a295e1/68747470733a2f2f6665726f73732e6e65742f782f6e6f6465322e676966)](https://camo.githubusercontent.com/328e6333f6a787e999a9ad7483ce5f9a9100b7ade4df13d125d4ce18f7a295e1/68747470733a2f2f6665726f73732e6e65742f782f6e6f6465322e676966)

#### [](https://www.npmjs.com/package/webtorrent#modules-1)Modules

These are the main modules that make up WebTorrent:

| module | tests | version | description |
| --- | --- | --- | --- |
| [webtorrent](https://github.com/webtorrent/webtorrent) | [![](https://camo.githubusercontent.com/350213420144bdeddd12dd5a79278d7f5d2fced1e41df2f2a981202ac26f36e6/68747470733a2f2f696d672e736869656c64732e696f2f63692f776562746f7272656e742f776562746f7272656e742f6d61737465722e737667)](https://ci-ci.org/webtorrent/webtorrent) | [![](https://camo.githubusercontent.com/2c94c6f4fcbdef1d9f9a9eea77e8ea6fe5e48daf27ac4fb85649273210845458/68747470733a2f2f696d672e736869656c64732e696f2f6e706d2f762f776562746f7272656e742e737667)](https://www.npmjs.com/package/webtorrent) | torrent client (this module) |
| [bittorrent-dht](https://github.com/webtorrent/bittorrent-dht) | [![](https://camo.githubusercontent.com/c361da280fa8cbc371461c0bb8c5a8ffcf9c053db338c12d1a3c3173e99dfa46/68747470733a2f2f696d672e736869656c64732e696f2f63692f776562746f7272656e742f626974746f7272656e742d6468742f6d61737465722e737667)](https://ci-ci.org/webtorrent/bittorrent-dht) | [![](https://camo.githubusercontent.com/b0c3cbb8ab1a24d063855a3bb41d0e66115ed945f5e77a3f769c48882e197eab/68747470733a2f2f696d672e736869656c64732e696f2f6e706d2f762f626974746f7272656e742d6468742e737667)](https://www.npmjs.com/package/bittorrent-dht) | distributed hash table client |
| [bittorrent-peerid](https://github.com/webtorrent/bittorrent-peerid) | [![](https://camo.githubusercontent.com/c43a82ee68a6243bd75e5d1d1dffc6e1fd099cfeefdb72efffe70cb170dfb2e5/68747470733a2f2f696d672e736869656c64732e696f2f63692f776562746f7272656e742f626974746f7272656e742d7065657269642e737667)](https://ci-ci.org/webtorrent/bittorrent-peerid) | [![](https://camo.githubusercontent.com/e1853f3d48d374ca27d4b079b077d1e622e6e263d1d2e2769323232b94d7ec28/68747470733a2f2f696d672e736869656c64732e696f2f6e706d2f762f626974746f7272656e742d7065657269642e737667)](https://www.npmjs.com/package/bittorrent-peerid) | identify client name/version |
| [bittorrent-protocol](https://github.com/webtorrent/bittorrent-protocol) | [![](https://camo.githubusercontent.com/aaff778d6fee758dd00941fb8a943f3395d855c054ed6ffe4acec80d6b7b6fda/68747470733a2f2f696d672e736869656c64732e696f2f63692f776562746f7272656e742f626974746f7272656e742d70726f746f636f6c2f6d61737465722e737667)](https://ci-ci.org/webtorrent/bittorrent-protocol) | [![](https://camo.githubusercontent.com/a8639c3bd1276f8e955e503eec04d3c4a1cd1c2a1bab883a5712de1a346a473b/68747470733a2f2f696d672e736869656c64732e696f2f6e706d2f762f626974746f7272656e742d70726f746f636f6c2e737667)](https://www.npmjs.com/package/bittorrent-protocol) | bittorrent protocol stream |
| [bittorrent-tracker](https://github.com/webtorrent/bittorrent-tracker) | [![](https://camo.githubusercontent.com/dd75fb97a66c2eebd93f77778b800412b79c7050bb2726472bd4a77edf34c623/68747470733a2f2f696d672e736869656c64732e696f2f63692f776562746f7272656e742f626974746f7272656e742d747261636b65722f6d61737465722e737667)](https://ci-ci.org/webtorrent/bittorrent-tracker) | [![](https://camo.githubusercontent.com/1f4ded3df28ea61329afedcd6c58bad0e6c1a925b6fc3aa63a6fa3d6f496ff39/68747470733a2f2f696d672e736869656c64732e696f2f6e706d2f762f626974746f7272656e742d747261636b65722e737667)](https://www.npmjs.com/package/bittorrent-tracker) | bittorrent tracker server/client |
| [bittorrent-lsd](https://github.com/webtorrent/bittorrent-lsd) | [![](https://camo.githubusercontent.com/ea286ed2ba86f27666d2e08c89975bf7cedbb3a1755c8487872e8a37bd977e45/68747470733a2f2f696d672e736869656c64732e696f2f63692f776562746f7272656e742f626974746f7272656e742d6c73642f6d61737465722e737667)](https://ci-ci.org/webtorrent/bittorrent-lsd)] | [![](https://camo.githubusercontent.com/0266fec0b8f51c296999df55b3c0c8570291cfef24b57f8253b9568f654a6476/68747470733a2f2f696d672e736869656c64732e696f2f6e706d2f762f626974746f7272656e742d6c73642e737667)](https://www.npmjs.com/package/bittorrent-lsd) | bittorrent local service discovery |
| [create-torrent](https://github.com/webtorrent/create-torrent) | [![](https://camo.githubusercontent.com/a91c6bf75a719b052db4573fbb9255e3f27ca8ee408ecf8ef9b5cfbf80dd8d17/68747470733a2f2f696d672e736869656c64732e696f2f63692f776562746f7272656e742f6372656174652d746f7272656e742f6d61737465722e737667)](https://ci-ci.org/webtorrent/create-torrent) | [![](https://camo.githubusercontent.com/0663e018e10e08bacba3c0e458befbedda927f2ea51347366f64aee7058e552a/68747470733a2f2f696d672e736869656c64732e696f2f6e706d2f762f6372656174652d746f7272656e742e737667)](https://www.npmjs.com/package/create-torrent) | create .torrent files |
| [magnet-uri](https://github.com/webtorrent/magnet-uri) | [![](https://camo.githubusercontent.com/b9d5ea42535d0c71491d4b466cee37e5fef159ca6dbe619c269be5b8a6688d54/68747470733a2f2f696d672e736869656c64732e696f2f63692f776562746f7272656e742f6d61676e65742d7572692f6d61737465722e737667)](https://ci-ci.org/webtorrent/magnet-uri) | [![](https://camo.githubusercontent.com/5a8fd54afbb90de07ba03ff4f2cf59e3151355a7bb87688767622f6f33230338/68747470733a2f2f696d672e736869656c64732e696f2f6e706d2f762f6d61676e65742d7572692e737667)](https://www.npmjs.com/package/magnet-uri) | parse magnet uris |
| [parse-torrent](https://github.com/webtorrent/parse-torrent) | [![](https://camo.githubusercontent.com/f2fd89ca44015011b8e7df2e2869cd88d9793256b1cd55dfec4a4f7a9bfbe24b/68747470733a2f2f696d672e736869656c64732e696f2f63692f776562746f7272656e742f70617273652d746f7272656e742f6d61737465722e737667)](https://ci-ci.org/webtorrent/parse-torrent) | [![](https://camo.githubusercontent.com/aa796507317b9c07b3f00b3471b2236b643cd316eac22633953d9e0b50938a5a/68747470733a2f2f696d672e736869656c64732e696f2f6e706d2f762f70617273652d746f7272656e742e737667)](https://www.npmjs.com/package/parse-torrent) | parse torrent identifiers |
| [render-media](https://github.com/feross/render-media) | [![](https://camo.githubusercontent.com/51198cebe789440a56201dbe43a547a2437eca194c535d0da2c378c0ae834f77/68747470733a2f2f696d672e736869656c64732e696f2f63692f6665726f73732f72656e6465722d6d656469612f6d61737465722e737667)](https://ci-ci.org/feross/render-media) | [![](https://camo.githubusercontent.com/310f74cec76f8fb5a54abb8565032e11014e5c93c5fe9beac4a9500cd6cc2771/68747470733a2f2f696d672e736869656c64732e696f2f6e706d2f762f72656e6465722d6d656469612e737667)](https://www.npmjs.com/package/render-media) | intelligently render media files |
| [torrent-discovery](https://github.com/webtorrent/torrent-discovery) | [![](https://camo.githubusercontent.com/9d6b001174a87995c0fb46a394f9f00a0f119b42cf50a12957bf476668f94408/68747470733a2f2f696d672e736869656c64732e696f2f63692f776562746f7272656e742f746f7272656e742d646973636f766572792f6d61737465722e737667)](https://ci-ci.org/webtorrent/torrent-discovery) | [![](https://camo.githubusercontent.com/7c98dea96b99e3d1759a24781d6194fd76825b9f6625ff020e4f0719b598c922/68747470733a2f2f696d672e736869656c64732e696f2f6e706d2f762f746f7272656e742d646973636f766572792e737667)](https://www.npmjs.com/package/torrent-discovery) | find peers via dht, tracker, and lsd |
| [ut_metadata](https://github.com/webtorrent/ut_metadata) | [![](https://camo.githubusercontent.com/17525349b5c1725972006b1f81ac80fecd50eb0fd904765ad4fe22bbb934d67a/68747470733a2f2f696d672e736869656c64732e696f2f63692f776562746f7272656e742f75745f6d657461646174612f6d61737465722e737667)](https://ci-ci.org/webtorrent/ut_metadata) | [![](https://camo.githubusercontent.com/0e7fdfac00328443d52aa29b2729e4f75319ad27f3fe15f3feca5d7cc6560976/68747470733a2f2f696d672e736869656c64732e696f2f6e706d2f762f75745f6d657461646174612e737667)](https://www.npmjs.com/package/ut_metadata) | metadata for magnet uris (protocol extension) |
| [ut_pex](https://github.com/webtorrent/ut_pex) | [![](https://camo.githubusercontent.com/97738d469019f5c6d8314aa83489b304a754450d9f88967de5ca70ea52a3317a/68747470733a2f2f696d672e736869656c64732e696f2f63692f776562746f7272656e742f75745f7065782e737667)](https://ci-ci.org/webtorrent/ut_pex) | [![](https://camo.githubusercontent.com/7bb671d3b777077b7c3ddda84e741f16cf4a69ab39187cff89b6491fae50825a/68747470733a2f2f696d672e736869656c64732e696f2f6e706d2f762f75745f7065782e737667)](https://www.npmjs.com/package/ut_pex) | peer discovery (protocol extension) |

#### [](https://www.npmjs.com/package/webtorrent#enable-debug-logs)Enable debug logs

In node, enable debug logs by setting the `DEBUG` environment variable to the name of the module you want to debug (e.g. `bittorrent-protocol`, or `*` to print all logs).

```source-shell
DEBUG=* webtorrent
```

In the browser, enable debug logs by running this in the developer console:

```source-js
localStorage.debug = '*'
```

Disable by running this:

```source-js
localStorage.removeItem('debug')
```

---
