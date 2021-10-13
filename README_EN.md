<sub>
<a href="./README.md">한국어</a> | <a href="#user-friendly-introduction">Go to user-friendly introduction</a>
</sub>

<h4 align=center>
<img  src="https://raw.githubusercontent.com/shadow-b1ock/shadowBlock/master/art/icon.png" height="160" width="160">
</h4>
<p></p>
<h1 align=center>
ShadowBlock<br><br>
</h1>


Adblocking, once started to fight online ads, now occupies significant amount of Internet traffic. Also have risen are publishers which started to detect adblocking users. Some of them are merely collecting this data for statistics, while others block access to the site altogether (_antiadblock_), or goes further by _deliberately_ breaking site's necessary function so subtly that users blame adblockers for the breakage. On the other hand, more publishers are using _ad-reinsertion_ techniques such as URL obfuscation, DOM obfuscation in an attempt to make it hard for adblockers to target ads.

This GitHub repository provides an experimental browser extension __ShadowBlock__, which is a proof-of-concept demonstrating technical possibility of blocking ads while rendering antiadblock impossible, i.e. so that websites cannot detect that ads are being blocked, only at the __layer of browser extensions__.

 - ShadowBlock employs [membrane pattern](https://tvcutsem.github.io/membranes) to install hooks to every browser API that websites may use to query the adblocking status. By returning results indicating that ads are not being blocked, it neutralizes website's _antiadblock_.
 - ShadowBlock implements Shadow DOM bypassing, so that it can target ads in the presence of DOM obfuscation techniques using [Shadow DOM](https://developers.google.com/web/fundamentals/web-components/shadowdom).

## Prior art

After developing first versions of this extension under the name "ShadowBlock", I became aware of a similar research result which implemented stealthy adblocking browser under the same name. This extension was developed without knowing the existence of this result.

<pre>
ShadowBlock: A Lightweight and Stealthy Adblocking Browser, 
Shitong Zhu, Umar Iqbal, Zhongjie Wang, Zhiyun Qian, Zubair Shafiq and Weiteng Chen
<a href="https://dl.acm.org/doi/10.1145/3308558.3313558">https://dl.acm.org/doi/10.1145/3308558.3313558</a>
</pre> 

In this result (ShadowBlock _browser_), Chrome's CSS engine is modified to cater for an additional `visibility` property, and uses it as a basis for hooking v8 bindings. This is the same conclusion that we have arrived at ShadowBlock, namely, `visibility` is preferable for reducing side-channel attack surfaces.

The difference is that, ShadowBlock browser identifies ads via a separate module utilizing Javascript call stacks, but ShadowBlock (extension) need to be provided with hard-coded list of 'worst ads' that ordinary adblockers cannot block. This is more like to the way usual adblockers work depending on filter lists. Next, because ShadowBlock was implemented using browser extension API, it is required to defense a whole lot more attack surfaces, and this increases technical complexity. In doing so, certain issues that prevented completely eliminating side-channel attack for hiding elements via `visibility` CSS properties were emerged, and ShadowBlock now switched to use `clip-path` property for element hiding.

<br>
<br>

# User-friendly introduction

ShadowBlock is an experimental browser extension for websites which tries hard to detect adblock. Certain websites employ techniques such as URL obfuscation, DOM obfuscation in order to make it hard for adblockers to identify and block ads, and they often _deliberately_ break the website's necessary function whenever they detect that their ads are blocked, in a subtle manner so as to manipulate users to blame their adblockers.

The goal of ShadowBlock is to _shadow_ such ads, while being undetectable to website. Such ads often use a web technology "Shadow DOM", which deters original adblockers, and ShadowBlock is well-suited for such ads.

ShadowBlock only applies so-called "cosmetic filtering", or CSS-based element hiding. Also it is enabled on certain websites that are known to have such bad ads - it is expected to be used alongside with ordinary adblockers. Sometimes the ordinary adblocker you were using may trigger website's adblock detection. In such cases, it has to be fixed from the other adblocker by disabling certain rules or filters.

## It does not hide blank ad placeholders.

This is a necessary measure to make ad-blocking undetectable. If ad placeholders are also collapsed, it will leave a very visible effect on other website component's position, and given that website tries very hard to detect the presence of adblock, it will be a matter of time for the website to update its detection logic and intentionaly break its function again.

## Supported websites

This extension supports a few websites as of now to which many users have reported hard-to-block ads and adblock vendors are having difficulty. It has been verified that these websites actually, at least once, deployed some form of antiadblock and ad-reinsertion.

 - `ppss.kr`
 - `ygosu.com`

This extension contains codes that are very specific to websites listed below, but its general methodology can be applied to other websites which contains similar ads. If you want to have a website integrated into this extension, please open a new issue.

_To extension reviewers: please visit https://shadow-b1ock.github.io/test-site-for-reviewers/index.html to test the extension, this extension works on that site too._

## Installation

### Chrome

<!--
<details>
    <summary>Chrome Web Store version is currently not up-to-date due to its review process, and is not recommended to install.</summary>
-->
<a target="_blank" href="https://chrome.google.com/webstore/detail/jdchbieocpofgppblcgibeckljdgocfh"><img src="./art/cws_badge.png" alt="Install ShadowBlock at Chrome Web Store"></a>
<!--
</details>
-->

To install manually, head over to the [release page](https://github.com/shadow-b1ock/shadowBlock/releases), download the `chrome.zip`, and extract it somewhere. 
Go to chrome "Extensions" tab, enable "Developer mode", and click on "Load unpacked extension", and select the directory where you've extracted the downloaded archive.

When it is installed manually from file, it does not automatically update, and you need to download from this page again to install a newer version. Chrome Web Store version is automatically updated, but since CWS review often takes several days, the version available in the release page may be a more recent than the version in CWS.

#### Installation in mobile browsers

Mobile Chrome does not support extensions. Android [Kiwi Browser](https://play.google.com/store/apps/details?id=com.kiwibrowser.browser) supports extensions like the desktop Chrome, and can also "load unpacked extension" using "developer mode". ShadowBlock supports Kiwi Browser.

### Firefox

Firefox version is directly distributed from this repository instead of via the official Firefox Add-Ons page(https://<!---->addons.mozilla.org). Clicking on the below link in a Firefox browser will start downloading the extension's `.xpi` file, and it will be automatically installed. Firefox version will automatically update itself to newer versions.

<a target="_blank" href="https://raw.githubusercontent.com/shadow-b1ock/shadowBlock/master/firefox/shadowblock-2.12-an+fx.xpi"><img src="./art/amo_badge.png" alt="Install ShadowBlock on Firefox"></a>

Alternatively, you can download `shadowblock-***-an+fx.xpi` file from the [release page](https://github.com/shadow-b1ock/shadowBlock/releases).

## Supported browser

 - Any chrome-based browser, version ≥ 88: [CSS Selectors 4 Complex :not()](https://www.chromestatus.com/feature/5014164156186624)
 - Firefox, version ≥ 84: [support compound selectors and complex selectors within :not() negation pseudo-class
](https://bugzilla.mozilla.org/show_bug.cgi?id=933562)
<br>

#### Terms of Usage

<sub>
Source codes and programs provided by this GitHub repository was produced for the sole purpose of technological and theoretical purposes, not to harm any individual or an entity, and users are acknowledging this by using this program. 
In no event shall the author be liable for any claim, damages or other liability, out of or in connection with the program or the use or the other dealings in the program. All copyrights of the source code and the program belongs to the author sha6owblock@gmail.com, and to copy or re-distribute the copyrighted works as a whole or by portion, you should a priori acquire a written permission from the copyright holder.
</sub>