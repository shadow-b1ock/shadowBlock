<sub>
<a href="./README_EN.md">English</a> | <a href="#사용자를-위한-소개">사용자를 위한 소개</a>
</sub>

<h4 align=center>
<img  src="https://raw.githubusercontent.com/shadow-b1ock/shadowBlock/master/art/icon.png" height="160" width="160">
</h4>
<p></p>
<h1 align=center>
ShadowBlock<br><br>
</h1>


인터넷 광고에 맞서 나타난 광고 차단 솔루션은 이제 전체 웹 트래픽의 유의미한 비중을 차지하는 하나의 현상이 되었습니다.
이에 대응하여, 광고를 차단하는 유저를 감지하고 이 정보를 단지 통계용으로 수집만 하는 것에서부터, 사이트 이용을 가로막거나, 더 나아가 사용자가 광고 차단 솔루션을 탓할 만큼만 미묘하게 사이트 기능을 망가뜨리는 등 광고를 차단한 유저를 차단하는 (_antiadblock_) 웹 사이트도 늘어나고 있으며, 한편으로는 광고 차단 솔루션이 차단할 수 없도록 광고를 URL 난독화, DOM 난독화 등을 거쳐 내보내는 광고차단 우회(_ad-reinsertion_)를 적용하는 매체도 늘어나고 있습니다.

현재 이 깃헙 저장소에서 제공하는 브라우저 확장프로그램 ShadowBlock은 광고를 차단하되, 광고차단 차단(antiadblock)을 불가능하게, 즉 웹사이트가 광고 차단 여부를 알 수 없도록 하는 것이 기술적으로 충분히 가능하며, 그것이 브라우저 엔진 레벨에서가 아니라 __확장 프로그램만으로__ 구현이 가능하다는 것을 증명하기 위한 실험적 확장프로그램입니다. 

 - ShadowBlock은 [membrane 패턴](https://tvcutsem.github.io/membranes)을 이용하여 광고 차단 감지에 사용될 수 있는 모든 브라우저 API에 가벼운 hook을 설치합니다. 이를 통해 웹사이트에서 현재 페이지의 상태를 API를 통해 유추할 때 광고가 차단되지 않았다는 결과를 리턴하여 _antiadblock_ 이 불가능하게 합니다.
 - ShadowBlock은 Shadow DOM-bypassing을 적용하여, _ad-reinsertion_ 에서 자주 사용되는 [Shadow DOM](https://developers.google.com/web/fundamentals/web-components/shadowdom)을 이용한 DOM 난독화가 적용된 광고 요소도 정확하게 타겟할 수 있습니다.

## Prior art

ShadowBlock이라는 이름을 붙이고 나서 검색하다 보니 _브라우저 엔진_ 레벨에서 비슷한 시도가 있었고 여기에도 마찬가지로 ShadowBlock이란 이름이 붙어 있다는 것을 알게 되었습니다. 본 확장프로그램의 최초 작성과 명명은 이 결과의 존재를 알지 못하고 이루어졌습니다.
<pre>
ShadowBlock: A Lightweight and Stealthy Adblocking Browser, 
Shitong Zhu, Umar Iqbal, Zhongjie Wang, Zhiyun Qian, Zubair Shafiq and Weiteng Chen
<a href="https://dl.acm.org/doi/10.1145/3308558.3313558">https://dl.acm.org/doi/10.1145/3308558.3313558</a>
</pre> 

위 결과 (ShadowBlock browser)에서는 크롬 엔진의 CSS 부분을 직접 수정하였고, 본 확장프로그램(의 최초 버전) 과 마찬가지로 side-channel attack을 줄이기 위해 CSS visibility 속성을 이용하는 것이 기술적으로 용이하다는 같은 결론에 도달하였습니다.

차이점은, ShadowBlock browser는 기존 광고 필터를 통해 광고 스크립트에서 생성한 페이지 요소를 구별하는 방식으로 ad-targetting module을 구현하였으나, 본 확장프로그램은 일반 광고차단 프로그램으로 차단이 어려운 일부 광고를 직접 target 하고 있습니다. 또한 확장프로그램 레벨에서 구현되었으므로 훨씬 넓은 attack surface를 방어해야 하고, 빠르게 발전하는 Web API로 인해 기술적 복잡도가 더 높아졌습니다. 이러한 과정에서 기존 visibility 속성의 확장프로그램 레벨에서의 적용을 어렵게 만드는 일부 문제점이 발견되었고, 현재 버전의 ShadowBlock은 `clip-path` 속성을 이용하고 있습니다.

<br>
<br>

# 사용자를 위한 소개

ShadowBlock의 목표는 일반적인 광고차단 프로그램으로 차단이 어려운 악성 광고를 사용자로부터 보이지 않도록 가리면서 (shadow), 웹사이트에서 감지할 수 없도록 하는 것입니다. 악성 광고에서 사용하는 기술 중 "Shadow DOM"이 있고, 일반적인 광고차단 툴로는 차단이 어려운 이 Shadow DOM을 이용한 광고를 ShadowBlock으로 차단할 수 있습니다.

ShadowBlock은 소위 말하는 cosmetic filtering, 즉 CSS 기반 차단만 적용하며, 현재 악성 광고를 사용하는 것으로 확인된 몇몇 사이트에만 적용되므로 일반 광고차단 프로그램과 병용하여야 합니다. 일반 광고차단 프로그램의 차단 때문에 웹사이트에서 광고차단이 감지될 수도 있습니다. 대개 이 경우 일부 차단규칙을 해제해주면 해결됩니다.

## 광고가 있었던 공간이 남습니다.

이는 광고가 차단되고 있다는 것을 알 수 없도록 하기 위해 불가결한 조치입니다. 광고의 빈 공간까지 제거하게 되면 광고와 관련 없는 페이지 요소의 위치에 영향이 발생하고, 웹사이트가 광고차단 감지에 매우 열심이라면 언제든지 이 영향을 감지하여 또다시 이미지 로딩 등 정상적인 기능을 _의도적으로_ 망가뜨릴 수 있기 때문에 이는 시한부 조치에 불과합니다.

## 지원 사이트

현재 많은 사용자들이 광고가 차단되지 않음을 호소하고 광고차단 툴에서도 차단을 어려움을 겪는 것으로 알려져 있는 일부 사이트를 지원하고 있습니다. 이 사이트들은 실제로 과거 적어도 한 번 antiadblock과 ad-reinsertion을 적용한 것으로 확인되었습니다.

 - `ppss.kr`
 - `ygosu.com`

현재 ad-targetting module에 위 사이트를 위한 맞춤형 코드가 들어가 있지만, ShadowBlock에서 적용하는 방법론은 일반적인 사이트에도 적용 가능합니다. 광고차단 감지가 끈질기며, 광고에 Shadow DOM을 이용하는 사이트가 있다면 이슈 트래커로 요청 바랍니다.

## 설치하기

### 크롬

<!--
<details>
    <summary>크롬 웹 스토어 버전은 현재 리뷰에 소요되는 기간으로 인해 최신 버전이 아니며, 설치를 권장하지 않습니다.</summary>
--->
<a target="_blank" href="https://chrome.google.com/webstore/detail/jdchbieocpofgppblcgibeckljdgocfh"><img src="./art/cws_badge.png" alt="크롬 웹 스토어에서 ShadowBlock을 설치하세요"></a>
<!--
</details>
--->

직접 설치하려면, [release 페이지](https://github.com/shadow-b1ock/shadowBlock/releases) 에서 `chrome.zip` 파일을 다운받고, 압축을 푸십시오.
크롬 브라우저의 확장프로그램 탭에서 "개발자 모드" 를 활성화하면 "압축 해제된 확장프로그램 로드" 버튼이 나타납니다. 이를 클릭한 후 압축을 푼 디렉토리를 선택하십시오. 

직접 설치한 버전은 자동 업데이트가 지원되지 않고 업데이트하기 위해서는 새 버전을 직접 다운로드하여 설치해야 합니다. 
크롬 웹 스토어에서 설치할 시 자동 업데이트가 적용되나, 웹 스토어 심사에 소요되는 기간으로 인해 release 페이지에서 직접 설치하는 버전이 웹 스토어의 버전보다 최근 버전일 수 있습니다.

#### 모바일 브라우저

모바일 크롬은 확장 프로그램을 지원하지 않습니다. 안드로이드 [키위 브라우저](https://play.google.com/store/apps/details?id=com.kiwibrowser.browser)는 데스크탑 크롬과 똑같이 확장 프로그램을 지원하고 마찬가지로 "개발자 모드" 를 통해 "압축 해제된 확장프로그램 로드" 가 가능합니다. ShadowBlock은 키위 브라우저도 지원하고 있습니다.

### 파이어폭스

파이어폭스 버전은 모질라 공식 홈페이지(https://<!---->addons.mozilla.org)를 통하지 않고 GitHub를 통해 직접 배포됩니다. 파이어폭스 브라우저에서 아래 링크를 클릭하면 GitHub에서 애드온 .xpi 파일 다운로드가 시작되고, 자동으로 설치됩니다. 파이어폭스 버전은 자동으로 최신 버전으로 업데이트 됩니다.

<a target="_blank" href="https://raw.githubusercontent.com/shadow-b1ock/shadowBlock/master/firefox/shadowblock-2.12-an+fx.xpi"><img src="./art/amo_badge.png" alt="파이어폭스 브라우저에 ShadowBlock을 설치하세요."></a>

[release 페이지](https://github.com/shadow-b1ock/shadowBlock/releases)에서 `shadowblock-***-an+fx.xpi` 파일을 직접 다운받아도 됩니다.

## 지원 환경

 - 크롬 기반 브라우저, 버전 88 이상: [CSS Selectors 4 Complex :not()](https://www.chromestatus.com/feature/5014164156186624)
 - 파이어폭스, 버전 84 이상: [support compound selectors and complex selectors within :not() negation pseudo-class
](https://bugzilla.mozilla.org/show_bug.cgi?id=933562)
<br>

#### 이용약관

<sub>
본 깃헙 저장소에서 제공하는 소스코드와 프로그램은 특정 개인, 단체에 어떠한 형태로든 위해를 가하기 위해가 아닌, 순수하게 기술적, 이론적 목적으로 작성되었고, 사용자는 이 사실을 인지하고 프로그램을 사용하는 것입니다. 본 프로그램의 사용으로 인한 효과 또는 피해에 대해 저자는 어떠한 책임 및 의무도 지지 않으며, 사용자는 프로그램을 사용함으로서, 프로그램이 가져올 효과에 대한 자의적인 기대를 가지고 본 프로그램을 사용하는 것입니다. 본 소스코드의 모든 저작권은 깃헙 계정 sha6owblock@gmail.com에 있으며 저작물의 전체 또는 일부를 어떠한 형태로든 복사, 배포하려면 그에 앞서 저작권자의 명시적인 허락을 얻어야 합니다.
</sub>