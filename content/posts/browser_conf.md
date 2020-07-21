---
title: "My Browser Configuration"
date: 2020-07-11T16:00:00+05:30
draft: false
toc: false
images:
tags:
  - Browser
  - Privacy
  - Security
  - Extensions
  - Configuration
---

A Browser is what we use to access the Internet i.e. the World. The technology has advanced to a level that we can mostly access/use everything from our palms. Out of all the software/applications the mostly helpful and useful one is a Browser.

In this post I would like to share my browser configuration and the extensions that I use to make my life a little secure, private and easier.

The two devices that I mostly use are:

1. **Laptop**

  - [Details](#laptop)
  - [Browser Settings](#browser-settings)
  - [Extensions](#extensions)
  - [Flags](#flags)

2. **Android Device**

  - [Details](#android)
  - [AdBlocking and AdFiltering](#and-ads)
  - [Flags](#and-flags)

## My Laptop Browser Configuration: {#laptop}

Details:
* Operating System - Manjaro (Arch-Based)
  - [Website](https://manjaro.org/)
  - [Download](https://manjaro.org/download/)

* Browser - Brave Browser (Chromium Based)
  - [Website](https://brave.com/)
  - [Download](https://brave.com/download/)

---

**Note**

As I use Brave as my default browser most of the configuration will be according to it.

---

### Browser Settings {#browser-settings}

Brave out-of-the-box is configured very neatly, for someone with basic needs may not need to tinker anything expect for the startup settings. Brave has an inbuilt adblocker that blocks almost all the third-party ads and trackers in the website. But Brave can be configured a lot to improve both the security and privacy.

* To access the settings panel use can either type `brave://settings` in the address bar or use the hamburger section present in the right top to access it.

  ![hamburger menu](/img/3_1.png)

* All the settings are mostly intuitive. Go through all the settings and enable/disable it as per your choice. The Following are the ones that I have enabled/disabled:

  - Enabled `Show autocomplete in address bar` (uses some cookies to do so)

    ![autocomplete](/img/3_2.png)

  - Disabled `Show top sites in autocomplete suggestions` and `Show Brave suggested sites in autocomplete suggestions` (prevents the issue of brave referrals)

    ![suggested](/img/3_3.png)

  - Shields section

    - The Shields is activated by default. The default is set to `Simple view`, select the `Advanced view` to get more information shown in the bar.

      ![shields](/img/3_4.png)

    - Block cross-site trackers - Blocks advertisements and trackers that are in the sites.

    - Upgrade connections to HTTPS - It uses [HTTPS Everywhere's](https://www.eff.org/https-everywhere) rule set to achieve this.

    - Block Scripts - Javascript in a website will be blocked (some websites may not work as intended). The default is set to be disabled as most of the websites use Javascript and so do the trackers/ads (I intend to block it).

    - Cookies - It is set to accept only the 1st party ones (the website the user is in), blocking all the other 3rd party cookies. It can be set to Block all to improve privacy, but sometimes comfortability takes the choice.

    - Fingerprinting - Set to Block all Fingerprinting. Enabling this will improve our privacy a lot. More on [Fingerprinting Protection](https://github.com/brave/brave-browser/wiki/Fingerprinting-Protections).

    > Even without the use of cookies, some websites can identify the way your browser and device differ from others in order to recognize you based on your unique combination of these traits. This approach is called “Fingerprinting” (sometimes referred to as “Device Recognition”).

    - For Further reading, read the article by [Brave](https://support.brave.com/hc/en-us/articles/360022806212-How-do-I-use-Shields-while-browsing-).

  - Social media blocking

    - As the section header says it blocks/removes all the tracking and embedded links in a sites that are related to the specified websites.

      ![Social Media Blocking](/img/3_5.png)

  - Search Engine

    - This section lets you select the default search engine. If not initially changed at the startup of the browser.

    - To add a url of choice add `/search?q=%s` at the end of the url.

    - For Example, I use [Whoole Search](https://github.com/benbusby/whoogle-search) a self-hosted search engine runs at `localhost:8888`.

    - To make it as my default search engine, Click the `Add` option in the `Manage search engines` section and add `localhost:8888/search?q=%s`.

  - Extensions Section

    - Not the extensions but the extensions section in the settings.

      ![extensions](/img/3_6.png)

    - `Web3 provider for using Dapps` and `Load Crypto Wallets on startup` are for Crypto and struff.

    - Brave supports an amazing feature `TOR`. It can be used from the hamburger section in the toolbar.

    - `WebTorrent` lets you torrent directly in the browser.

    - Enabled `Widevine` for streaming services.

  - Brave has an option that checks for data breaches, bad extensions and more called `Safety Check`.

  - Enabled `Do Not Track`, which sends a "Do Not Track" request with your browsing traffic.

  - Enabled `Use hardware acceleration when available` for good performances.


### Extensions {#extensions}

* [uBlock Origin](#ublock-origin)
* [uMatrix](#umatrix)
* [uBlock Origin Extra](#uboextra)
* [CleanURLs](#cleanurls)
* [Bypass Paywalls](#bypasspaywalls)
* [Random User-Agent](#random)
* [Tampermonkey](#tampermonkey)
* [Privacy Badger](#pb)
* [Font Fingerprint Defender](#font)
* [AudioContext Fingerprint Defender](#audio)
* [WebGL Fingerprint Defender](#webgl)
* [Change Geolocation(Location Guard)](#geo)
* [Universal Bypass](#ub)
* [Behave!](#behave)
* [CSS Exfil Protection](#css)
* [h264ify](#h264ify)
* [The Great Suspender](#suspender)
* [SponsorBlock for YouTube](#sponsorblock)
* [Redirector](#redirect)

#### *Extension links and configuration*

#### uBlock Origin {#ublock-origin}

  > An efficient blocker add-on for various browsers. Fast, potent, and lean.

  - *Links*:

    * [Source Code](https://github.com/gorhill/uBlock)

    * [Chrome](https://github.com/gorhill/uBlock#chromium)

    * [Firefox](https://github.com/gorhill/uBlock#firefox--firefox-for-android)

  - *My configuration*

    - I have blocked all the 3rd party stuff, which includes `3rd-party`, `3rd-party scripts` and `3rd-party frames` (to get the ability to do this turn on the `I am a advanced user` option in the uBlock settings).

      ![uBlock](/img/3_7.png)

    - *Extension Settings*

      - *Under Privacy section (Enabled):*

        * Disable pre-fetching (to prevent any connection for blocked network requests)

          > Checking this will disable prefetching in your browser. When prefetching is enabled, the browser can still establish connections to remote servers even if the resource from these remote servers are meant to be blocked by uBlock.

          For further reading - [Disable pre-fetching](https://github.com/gorhill/uBlock/wiki/Dashboard:-Settings#disable-pre-fetching)

        * Disable hyperlink auditing

          > Checking this will prevent hyperlink auditing. Hyperlink auditing is best summarized as "phone home" feature (or more accurately "phone anywhere") meant to inform one or more servers of which links you click on (and when).

          For further reading - [Disable hyperlink auditing](https://github.com/gorhill/uBlock/wiki/Dashboard:-Settings#disable-hyperlink-auditing)

        * Prevent WebRTC from leaking local IP addresses

          > Keep in mind that this feature is to prevent leakage of your non-internet-facing IP adresses. The purpose of this feature is not to hide your current internet-facing IP address -- so be cautious to not misinterpret the results of the tests above. For example, if you use a VPN, your internet-facing IP address is that of the VPN, so your ISP-provided IP address should not be visible to outside world with this setting checked. However, if you are not behind any VPN or proxy, your ISP-provided IP address will be visible regardless of this setting.

          For further reading - [Prevent WebRTC from leaking local IP address](https://github.com/gorhill/uBlock/wiki/Dashboard:-Settings#prevent-webrtc-from-leaking-local-ip-address)

        * Block CSP reports

          > You can block network requests made as a result of your browser reporting Content Security Policy violations ("CSP reports") to a remote server (which can be 3rd-party to the site where the violation occurred).

          For further reading - [Block CSP reports](https://github.com/gorhill/uBlock/wiki/Dashboard:-Settings#block-csp-reports)

      - *Filter Lists*

        > The Filter lists pane is where you subscribe to filter lists. The filter lists to which you subscribe will feed uBlock Origin's static filtering engine."

        - I have mostly enabled all the default lists in the settings.

          ![filters](/img/3_8.png)

        - Here are some more filters I use:

          * https://gitlab.com/CHEF-KOCH/cks-filterlist/-/raw/master/CK%27s-FilterList.txt

          * https://www.i-dont-care-about-cookies.eu/abp/

          * https://raw.githubusercontent.com/blocklistproject/Lists/master/facebook.txt

          * https://raw.githubusercontent.com/badmojr/1Hosts/master/complete/hosts.txt

          * https://raw.githubusercontent.com/StevenBlack/hosts/master/alternates/fakenews/hosts

          * https://raw.githubusercontent.com/TheUpBeat/Block-Ad-Web-Internet/master/Facebook_Blocked.txt

          * https://block.energized.pro/blu/formats/hosts

          * https://raw.githubusercontent.com/brave/adblock-lists/master/coin-miners.txt

            For more check out - [FilterLists](https://filterlists.com/)

#### uMatrix {#umatrix}

  > Point and click matrix to filter net requests according to source, destination and type

  * *Links:*

    - [Source Code](https://github.com/gorhill/uMatrix)

    - [Chrome](https://chrome.google.com/webstore/detail/%C2%B5matrix/ogfcmafjalglgifnmanfmnieipoejdcf)

    - [Firefox](https://addons.mozilla.org/firefox/addon/umatrix/)

  * *My Configuration*

    * Same as uBlock, uMatrix also blocks ads and trackers in the website but with more functionality and controllability. I have disabled all the 3rd-party and 1st-party in uMatrix.

      ![uMatrix](/img/3_9.png)

    * And most of other settings are set to default.

    * For more usage refer its [Wiki](https://github.com/gorhill/uMatrix/wiki). There are lot of features to tinker with.

#### uBlock Origin Extra {#uboextra}

  > A companion extension to uBlock Origin: to gain ability to foil early anti-user mechanisms working around content blockers or even a browser privacy settings.
  >
  > The extension is useful only for Chromium-based browsers. There is no need for such an extension so far on Firefox, and thus there is no version for Firefox.

  * *Links:*

    - [Source Code](https://github.com/gorhill/uBO-Extra)

    - [Chrome](https://chrome.google.com/webstore/detail/ublock-origin-extra/pgdnlhfefecpicbbihgmbmffkjpaplco)

  * [Sites on which uBO Extra is useful](https://github.com/gorhill/uBO-Extra/wiki/Sites-on-which-uBO-Extra-is-useful)


* <a id="cleanurls"> **CleanURLs** </a>

  > ClearURLs is an add-on based on the new WebExtensions technology and is optimized for Firefox and Chrome based browsers.

  * *Links:*

    - [Source Code](https://gitlab.com/KevinRoebert/ClearUrls)

    - [Chrome](https://chrome.google.com/webstore/detail/clearurls/lckanjgmijmafbedllaakclkaicjfmnk)

    - [Firefox](https://addons.mozilla.org/firefox/addon/clearurls/)


#### Bypass Paywalls {#bypasspaywalls}

  > Bypass Paywalls is a web browser extension to help bypass paywalls for selected sites.

  * *Lists:*

    - [Source Code](https://github.com/iamadamdev/bypass-paywalls-chrome)

    - Chrome:

      * Download this repo as a [ZIP file from GitHub](https://github.com/iamadamdev/bypass-paywalls-chrome/archive/master.zip).

      * Unzip the file and you should have a folder named `bypass-paywalls-chrome-master`.

      * In Chrome go to the extensions page.

      * Enable Developer Mode.

      * Drag the `bypass-paywalls-chrome-master` folder anywhere on the page to import it (do not delete the folder afterwards).

    - Firefox:

      * Download it from [Github](https://github.com/iamadamdev/bypass-paywalls-chrome/releases/latest/download/bypass-paywalls-firefox.xpi)


  * Lists of sites that can be bypassed - [Websites](https://github.com/iamadamdev/bypass-paywalls-chrome#bypass-the-following-sites-paywalls-with-this-extension)

#### Random User-Agent {#random}

  > Automatically replaces the User-Agent after a specified time interval

  > **Warning**
  >
  >Depending on your threat model, faking your user agent might make you more fingerprintable, not less. There are ways other than User-Agent sniffing to determine what browser you're using, so malicious sites could learn what browser you're really using through other means and then combine that with your randomly changing User-Agent to pretty effectively track you.

  * *Links:*

    - [Source Code](https://github.com/tarampampam/random-user-agent)

    - [Chrome](https://chrome.google.com/webstore/detail/random-hide-user-agent/einpaelgookohagofgnnkcfjbkkgepnp)

    - [Firefox](https://addons.mozilla.org/firefox/addon/random_user_agent/)


#### Tampermonkey {#tampermonkey}

  > Tampermonkey is the most popular userscript manager for Google Chrome.

  * *Links:*

    - [Source Code](https://github.com/Tampermonkey/tampermonkey)

    - [Chrome](https://chrome.google.com/webstore/detail/dhdgffkkebhmkfjojejmpbldmpobfkfo)

  * Scripts I use:

    - [Anti-Adblock Killer](https://greasyfork.org/scripts/735-anti-adblock-killer-reek)

    - [Pinterest without registration](https://greasyfork.org/en/scripts/6325-pinterest-without-registration)

    - [Reddit - Add Removeddit links](https://greasyfork.org/en/scripts/377129-reddit-add-removeddit-links)

    - [Sci-hub button](https://greasyfork.org/en/scripts/370246-sci-hub-button)

    - [AdsBypasser](https://greasyfork.org/en/scripts/4881-adsbypasser)

    - [AntiAdware](https://greasyfork.org/en/scripts/4294-antiadware)

#### Privacy Badger {#pb}

  > Privacy Badger is a browser extension that automatically learns to block invisible trackers. Instead of keeping lists of what to block, Privacy Badger learns by watching which domains appear to be tracking you as you browse the Web.
  >
  >Privacy Badger sends the Do Not Track signal with your browsing. If trackers ignore your wishes, your Badger will learn to block them. Privacy Badger starts blocking once it sees the same tracker on three different websites.
  >
  > Besides automatic tracker blocking, Privacy Badger removes outgoing link click tracking on Facebook and Google, with more privacy protections on the way.

  * *Links:*

    - [Source Code](https://github.com/EFForg/privacybadger)

    - [Chrome](https://chrome.google.com/webstore/detail/privacy-badger/pkehgijcmpdhfbdbbnkijodmdjhbjlgp)

#### Font Fingerprint Defender {#font}

  > Font Fingerprint Defender is a multi-browser addon that let you easily hide your real font fingerprint by reporting a random fake value. According to many tech blogs, completely blocking the fingerprint is not a good idea, therefore reporting a fake value could be the best solution to better protect your privacy. This addon simply adds a small noise to the actual fingerprint and renews it every time you visit a website or reload a page.

  * *Links:*

    - [Website](https://mybrowseraddon.com/font-defender.html)

    - [Chrome](https://chrome.google.com/webstore/detail/audiocontext-fingerprint/fhkphphbadjkepgfljndicmgdlndmoke)

    - [Firefox](https://addons.mozilla.org/en-US/firefox/addon/font-fingerprint-defender/)

* <a id="audio"> **AudioContext Fingerprint Defender** </a>

  > AudioContext Fingerprint Defender is a multi-browser addon that let you easily hide your real audiocontext fingerprint by reporting a random fake value. This addon does not block audiocontext or any other web audio API methods. It simply adds a small noise to the actual fingerprint and renews it every time you visit a website or reload a page.

  * *Links:*

    - [Website](https://mybrowseraddon.com/audiocontext-defender.html)

    - [Chrome](https://chrome.google.com/webstore/detail/audiocontext-fingerprint/pcbjiidheaempljdefbdplebgdgpjcbe)

    - [Firefox](https://addons.mozilla.org/en-US/firefox/addon/audioctx-fingerprint-defender/)

#### WebGL Fingerprint Defender {#webgl}

  > WebGL Fingerprint Defender is a multi-browser add-on that let you easily hide your real WebGL fingerprint by reporting a random fake value. According to many tech blogs, completely blocking WebGL API is not a good idea, therefore reporting a fake fingerprint could be the best solution to better protect your privacy. This addon simply adds a small noise to the actual fingerprint and renews it every time you visit a website or reload a page.

  * *Links:*

    - [Website](https://mybrowseraddon.com/webgl-defender.html)

    - [Chrome](https://chrome.google.com/webstore/detail/olnbjpaejebpnokblkepbphhembdicik)

    - [Firefox](https://addons.mozilla.org/en-US/firefox/addon/webgl-fingerprint-defender/)

#### Change Geolocation(Location Guard) {#geo}

  > Change Geolocation (Location Guard) is a multi-browser addon that let you easily change your geographic location to a desired value. Simply open addon options page and set latitude and longitude for where you want the geolocation to be. Next, reload a page and check your location.

  * *Links:*

    - [Website](https://mybrowseraddon.com/change-geolocation.html)

    - [Chrome](https://chrome.google.com/webstore/detail/change-geolocation-locati/lejoknkbcogjceoniealiipllomkpioe)

    - [Firefox](https://addons.mozilla.org/en-US/firefox/addon/change-geolocation-locguard/)

#### Universal Bypass {#ub}

  > Don't waste your time with compliance. Universal Bypass automatically skips annoying link shorteners.

  * *Links:*

    - [Source Code](https://github.com/Sainan/Universal-Bypass)

    - Chrome:

      * Download [Universal Bypass for Chromium-based browsers.zip](https://github.com/Sainan/Universal-Bypass/releases/download/13.12.4/Universal.Bypass.for.Chromium-based.browsers.zip).

      * Move this zip file in a convenient folder.

      * Open `chrome://extensions` in a new tab.

      * Enable "Developer mode" at the top-right.

      * Drag the zip file from before into the extensions page.

    - [Firefox](https://addons.mozilla.org/en-US/firefox/addon/universal-bypass/)

#### Behave! {#behave}

  > A (Still in Development) monitoring browser extension for pages acting as bad boys.

  * *Links:*

    - [Source Code](https://github.com/mindedsecurity/behave)

    - [Chrome](https://chrome.google.com/webstore/detail/mppjbkhgconmemoeagfbgilblohhcica/)

    - [Firefox](https://addons.mozilla.org/en-US/firefox/addon/behave/)

#### CSS Exfil Protection {#css}

  > Guard against CSS data exfiltration attacks.
  >
  > Guard your browser against CSS Exfil attacks!
  >
  > CSS Exfil is a method attackers can use to steal data from web pages using Cascading Style Sheets (CSS).  This plugin sanitizes and blocks any CSS rules which may be designed to steal data.

  * *Links:*

    - [Source Code](https://github.com/mlgualtieri/CSS-Exfil-Protection)

    - [Chrome](https://chrome.google.com/webstore/detail/css-exfil-protection/ibeemfhcbbikonfajhamlkdgedmekifo)

    - [Firefox](https://addons.mozilla.org/en-US/firefox/addon/css-exfil-protection/)

  * [Stealing Data With CSS: Attack and Defense](https://www.mike-gualtieri.com/posts/stealing-data-with-css-attack-and-defense)

#### h264ify {#h264ify}

  > h264ify is a Chrome/Firefox extension that makes YouTube stream H.264 videos instead of VP8/VP9 videos.

  * *Links:*

    - [Source Code](https://github.com/erkserkserks/h264ify)

    - [Chrome](https://chrome.google.com/webstore/detail/h264ify/aleakchihdccplidncghkekgioiakgal)

#### The Great Suspender {#suspender}

  > "The Great Suspender" is a free and open-source Google Chrome extension for people who find that chrome is consuming too much system resource or suffer from frequent chrome crashing. Once installed and enabled, this extension will automatically suspend tabs that have not been used for a while, freeing up memory and cpu that the tab was consuming.

  * *Links:*

    - [Source Code](https://github.com/greatsuspender/thegreatsuspender)

    - [Chrome](https://chrome.google.com/webstore/detail/the-great-suspender/klbibkeccnjlkjkiokjodocebajanakg)

#### SponsorBlock {#sponsorblock}

  > SponsorBlock is an extension that will skip over sponsored segments of YouTube videos. SponsorBlock is a crowdsourced browser extension that lets anyone submit the start and end times of sponsored segments of YouTube videos. Once one person submits this information, everyone else with this extension will skip right over the sponsored segment.

  * *Links:*

    - [Source Code](https://github.com/ajayyy/SponsorBlock)

    - [Chrome](https://chrome.google.com/webstore/detail/mnjggcdmjocbbbhaepdhchncahnbgone)

    - [Firefox](https://addons.mozilla.org/addon/sponsorblock/?src=external-github)

#### Redirector {#redirect}

  > Redirector is a browser add-on for Firefox, Chrome and Opera. The add-on lets you create redirects for specific webpages, e.g. always redirect http://bing.com to http://google.com. It was originally done by request for someone on the Mozillazine forums. The redirect patterns can be specified using regular expressions or simple wildcards and the resulting url can use substitutions based on captures from the original url.

  * *Links:*

    - [Source Code](https://github.com/einaregilsson/Redirector)

    - [Chrome](https://chrome.google.com/webstore/detail/redirector/ocgpenflpmgnfapjedencafcfakcekcd)

    - [Firefox](https://addons.mozilla.org/en-US/firefox/addon/5064)

  * To Set redirects

    ![redirects](/img/3_10.png)

### Flags {#flags}

* I have enabled many flags to improve my performance, privacy and security. The flags are only experimental features so be careful and enable/disable under your own risk.

* To edit the flags, go to `brave://flags`

#### *Enabled Flags*

  - **#enable-webrtc-hide-local-ips-with-mdns** - Conceal local IP addresses with mDNS hostnames.

  - **#enable-vulkan** - Use vulkan as the graphics backend.

  - **#cookies-without-same-site-must-be-secure** - If enabled, cookies without SameSite restrictions must also be Secure. If a cookie without SameSite restrictions is set without the Secure attribute, it will be rejected. This flag only has an effect if "SameSite by default cookies" is also enabled.

  - **#improved-cookie-controls** - mproved UI in Incognito mode for third-party cookie blocking. – Mac, Windows, Linux, Chrome OS, Android

  - **#improved-cookie-controls-for-third-party-cookie-blocking** - Enables an improved UI for existing third-party cookie blocking users.

  - **#enable-heavy-ad-intervention** - Heavy Ad Intervention
Unloads ads that use too many device resources.

  - **#reduced-referrer-granularity** - If a page hasn't set an explicit referrer policy, setting this flag will reduce the amount of information in the 'referer' header for cross-origin requests.

  - **#treat-unsafe-downloads-as-active-content** - Disallows downloads of unsafe files (files that can potentially execute code), where the final download origin or any origin in the redirect chain is insecure if the originating page is secure

  - **#enable-removing-all-third-party-cookies** - Enables UI on chrome://settings/siteData to remove all third-party cookies and site data.

  - **#prefetch-privacy-changes** - Prefetch requests will not follow redirects, not send a Referer header, not send credentials for cross-origin requests, and do not pass through service workers.

  - **#turn-off-streaming-media-caching** - Reduces disk activity during media playback, which can result in power savings.

  - **#audio-worklet-realtime-thread** - Run Audio Worklet operation on a realtime priority thread for better audio stream stability.

  - **#enable-parallel-downloading** - Enable parallel downloading to accelerate download speed.

  - **#privacy-settings-redesign** - Redesign of the privacy settings card to make it more prominent and and easier to use.

  - **#show-legacy-tls-warnings** - Show security warnings for sites that use legacy TLS versions (TLS 1.0 and TLS 1.1), which are deprecated and will be removed in the future.

  - **#tab-groups** - Allows users to organize tabs into visually distinct groups, e.g. to separate tabs associated with different tasks.

  - **#tab-groups-collapse** (if #tab-groups is enabled) - Allows a tab group to be collapsible and expandable, if tab groups are enabled. – Mac, Windows, Linux, Chrome OS

  - **#enable-noscript-previews** - Enable disabling JavaScript on some pages on slow networks.

  - **#freeze-user-agent** - Set the User-Agent request header to a static string that conforms to the current User-Agent string format but only reveals desktop vs Android and if the 'mobile' flag is set.

#### *Disabled Flags*

  - **#allow-popups-during-page-unload** - When the flag is set to enabled, pages are allowed to show popups while they are being unloaded.

  - **#enable-generic-sensor-extra-classes** - Enables an extra set of sensor classes based on Generic Sensor API, which expose previously unavailable platform features, i.e. AmbientLightSensor and Magnetometer interfaces.

  - **#tab-hover-cards** - Enables a popup containing tab information to be visible when hovering over a tab. This will replace tooltips for tabs

  - **#enable-user-data-snapshot** - Enables taking snapshots of the user data directory after a Chrome update and restoring them after a version rollback

  - **#enable-text-fragment-anchor** - Enables scrolling to text specified in URL's fragment.

  - **#happiness-tracking-surveys-for-desktop-demo** - Enable showing Happiness Tracking Surveys Demo to users on Desktop.

  - **#happiness-tracking-surveys-for-desktop** - Enable showing Happiness Tracking Surveys to users on Desktop.

  - **#raw-clipboard** - Allows raw / unsanitized clipboard content to be read and written.


## My Android Device Browser Configuration {#android}

**Details:**

  * Operating System - ArrowOS (ASOP based)

    - [Website](https://arrowos.net/)

    - [Download](https://arrowos.net/download.html)

  * Browser

    - Brave

      - [Website](https://brave.com/)

      - [Download](https://play.google.com/store/apps/details?id=com.brave.browser&hl=en)

    - Bromite

      - [Website](https://www.bromite.org/)

      - [Download](https://github.com/bromite/bromite/releases)


Most of all the settings are set to default.

### AdBlocking and AdFiltering {#and-ads}

I use AdGuard for filtering, blocking the advertisements and trackers. It has many features including a Firewall and HTTPS Filtering.

  - [Website](https://adguard.com/en/welcome.html)

  - [Download](https://adguard.com/en/adguard-android/overview.html)

### Flags {#and-flags}

#### *Enabled Flags*

  - **#context-menu-copy-image** - Enable copying image to system clipboard via context menu.

  - **#smooth-scrolling** - Animate smoothly when scrolling page content.

  - **#enable-vulkan** - Use vulkan as the graphics backend.

  - **#tls13-hardening-for-local-anchors** - This option enables the TLS 1.3 downgrade hardening mechanism for connections authenticated by local trust anchors. This improves security for connections to TLS-1.3-capable servers while remaining compatible with older servers. Firewalls and proxies that do not function when this is enabled do not implement TLS 1.2 correctly or securely and must be updated.

  - **#enable-autofill-credit-card-ablation-experiment** - If enabled, credit card autofill suggestions will not display.

#### *Disabled Flags*

  - **#enable-games-hub** - Enables viewing and usage of the Games Hub.

  - **#media-history** Enables Media History which records data around media playbacks on websites.


Some flags are set by default to improve the user's experience.


## Conclusion

This is my current configuration. I will be updating it, as learn to tweak it more. If you have any suggestions (or) would like to talk about your stuff (or) to just hang out and chat join our [Discord Server](https://discord.gg/JtfPSBf).
