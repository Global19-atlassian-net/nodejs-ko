---
category: articles
title: "Node.js 8: 디버깅과 네이티브 모듈 생태계의 많은 개선점"
author: Node.js Foundation
ref: "Node.js 8: Big Improvements for the Debugging and Native Module Ecosystem"
refurl: https://medium.com/the-node-js-collection/node-js-8-big-improvements-for-the-debugging-and-native-module-ecosystem-58454861f2fc
translator: <a href="https://github.com/kazikai" target="_blank">kazikai</a>
---
<!--
We are excited to announce Node.js 8.0.0 today. The new improvements and features of this release create the best workflow for Node.js developers to date. Highlighted updates and features include adding Node.js API for native module developers, async_hooks, JS bindings for the inspector, zero-filling Buffers, util.promisify and more.
-->
오늘 Node.js 8.0.0을 발표했습니다. 이번 릴리스의 새로운 개선점과 기능들은 오늘날 Node.js 개발자들에게 최고의 작업흐름을 만들었습니다.
주목할만한 업데이트와 기능은 네이티브 모듈 개발자들을 위한 Node.js API와 async_hooks, 인스펙터를 위한 JS 바인딩, zero-filling 버퍼, util.promisify 등이 포함됩니다.

![](https://cdn-images-1.medium.com/max/800/1*6-_PzFOl9FRNZPn-LEOi4Q.jpeg)

<!--Throwing confetti now that we have Node.js 8!-->
우리는 이제 Node.js 가지고 꽃길만 갑시다.

<!--
The Node.js 8 release, replaces version 7 in our current release line. The Node.js release line will become a Node.js Long Term Support (LTS) release in October 2017 (more details on [LTS strategy here](https://github.com/nodejs/LTS)). The LTS release line is focused on stability and security and is best for those who want guaranteed stability when they upgrade and/or are using Node.js in the enterprise.
-->
Node.js 8 릴리스는 현재 릴리스 라인에서 버전 7을 대체 합니다.
이 Node.js 릴리스 라인은 2017년 10월에 장기 지원(LTS) 릴리스가 될 것입니다. (조금 더 상세히 알고 싶다면 [LTS 전략보기](https://github.com/nodejs/LTS)). LTS 릴리스 라인은 안정성과 보안에 집중하고 있으므로 기업에서 Node.js를 사용하거나 업그레이드 할 때 안정성을 보장받고자 하는 사람들에게는 가장 적합합니다.

<!--
> _Those who need stability and have complex production environments (i.e. medium and large enterprises) should wait until Node.js 8 goes into LTS before upgrading it for production._
-->
> _안정성이 필요하고 복잡한 상용 환경들을 가지고 있는 곳들은 (즉 중대형 업체들) 상용에 업그레이드 하기 전에 Node.js 8이 LTS 로 갈 때까지 기다려야 합니다._

<!--
Now that we’ve provided this PSA, let’s dive into the interesting updates in this release.
-->
지금 우리는 이 PSA 를 제공했으니, 이번 릴리스에서 흥미로운 업데이트를 살펴보겠습니다.

<!--
**Native Modular Ecosystem Gets a Boost**
-->
**네이티브 모듈러 생태계가 추진력을 얻음**

<!--
The much awaited[Node.js API (N-API)](https://medium.com/the-node-js-collection/ibm-intel-microsoft-mozilla-and-nodesource-join-forces-on-node-js-48e21ffb697d) will be added as an experimental feature to this release — it will be behind a flag. This is an incredibly important technology as it will eliminate breakage that happens between major releases lines with native modules.
-->
많이 기다려온 [Node.js API(N-API)](https://medium.com/the-node-js-collection/ibm-intel-microsoft-mozilla-and-nodesource-join-forces-on-node-js-48e21ffb697d)가 이번 릴리스에서 실험적인 기능으로 추가 될 것입니다 - 플래그를 통해 사용할 수 있게 될 겁니다. 이것은 네이티브 모듈들이 있는 주요 릴리스 라인 사이에서 일어나는 손상을 제거할 수 있는 매우 중요한 기술입니다.

<!--
Although native modules (modules written in C or C++ and directly bound to the Chrome V8) are a small portion of the massive modular ecosystem, 30 percent of all modules rely indirectly on native modules. Every time Node.js has a major release update, package maintainers have to update these dependencies.
-->
비록 네이티브 모듈들은(C나 C++로 작성되고 크롬 V8에 직접 바인딩 되어있음) 대규모의 모듈러 생태계의 일부분이지만 모든 모듈의 30%는 네이티브 모듈에 간접적으로 의존합니다. Node.js가 주요 릴리스를 매번 업데이트 할 때마다, 패키지 소유자는 이 의존성을 업데이트해야만 합니다.

<!--
These efforts would not be possible without significant contributions from Google, IBM, Intel, Microsoft, nearForm, NodeSource and individual contributors. Read the full details around these efforts and this technology [here](https://medium.com/@nodejs/n-api-next-generation-node-js-apis-for-native-modules-169af5235b06).
-->
이러한 노력은 Google, IBM, Intel, Microsoft, nearForm, NodeSource와 개인 기여자들의 많은 기여 없이는 불가능했을 것입니다. 이 노력과 기술들에 대해 자세히 알고 싶다면 [여기](https://medium.com/@nodejs/n-api-next-generation-node-js-apis-for-native-modules-169af5235b06)를 읽어보세요.

<!--
> _Anyone who builds or uses native modules should test out the N-API feature._
-->
> _네이티브 모듈을 구축하거나 사용하는 사람이라면 누구든지 N-API 기능을 테스트해야 합니다._

<!--
**Welcome, V8 5.8**
-->
** V8 5.8을 환영합니다.**

<!--
Node.js 8 ships with V8 5.8, a significant update to the JavaScript runtime that includes major improvements in performance and developer facing APIs. V8 5.8 is guaranteed to have forwards ABI compatibility with V8 5.9 and the upcoming V8 6.0, which will help ensure stability of the Node.js native addon ecosystem. During Node.js 8’s lifetime, the Node.js Project plans to move to 5.9 and possibly 6.0.
-->
Node.js 8은 성능과 개발자가 주로 보는 API의 주요 개선사항을 포함하고 있는 자바스크립트 런타임의 상당한 업데이트인 V8 5.8과 같이 나왔습니다. V8 5.8은 Node.js의 네이티브 추가 생태계의 안정성을 보증하기 위해 V8 5.9와 곧 나올 V8 6.0의 ABI 호환성을 보장합니다. Node.js 8이 있는 동안 Node.js 프로젝트는 5.9와 가능하면 6.0으로 이동할 계획입니다.

<!--
The V8 5.8 engine also helps set up a pending transition to the new [Turbofan and Ignition](https://v8project.blogspot.com/2017/05/launching-ignition-and-turbofan.html) compiler pipeline, which leads to lower memory consumption and faster startup across Node.js applications. Although this has existed in previous versions of V8, TurboFan and Ignition will be enabled by default for the first time in V8 5.9\. The new compiler pipeline represents such a significant change that the [Node.js Core Technical Committee (CTC) chose to postpone](https://medium.com/the-node-js-collection/node-js-8-0-0-has-been-delayed-and-will-ship-on-or-around-may-30th-cd38ba96980d) the Node.js 8 release in order to better accommodate it.
-->
또한 V8 5.8 엔진을 사용하면 Node.js 애플리케이션들을 빠르게 시작하고 메모리 소모량도 적은 새로운 [Turbofan과 Ignition](https://v8project.blogspot.com/2017/05/launching-ignition-and-turbofan.html) 컴파일러 파이프라인으로 전환할 수 있습니다. V8의 이전 버전에서 이러한 기능들이 존재하지만 Turbofan과 Ignition은 V8 5.9에서 처음으로 기본설정이 될 것입니다. 새로운 컴파일러 파이프 라인은 이를 더 잘 수용하기 위해 [Node.js 코어 기술 위원회(CTC)에서 Node.js 8 릴리스를 연기](https://medium.com/the-node-js-collection/node-js-8-0-0-has-been-delayed-and-will-ship-on-or-around-may-30th-cd38ba96980d)하기로 결정할 만큼 중대한 변화입니다.

<!--
**Buffer Improvements**
-->
**버퍼 향상**

<!--
The zero-filling Buffer (num) and a new Buffer (num) are added by default. The benefit of the zero-filling Buffer helps with security and privacy to prevent information leaks. However, the downside with this buffer is that folks using it will take performance hits, but this can be avoided by migrating to buffer.allocUnsafe(). It is suggested that Node.js users only use this function, if they are aware of the risks and know how to avoid those problems.
-->
zero-filling 버퍼(num)와 새로운 버퍼(num)가 기본적으로 추가되었습니다. zero-filling 버퍼의 이점은 정보 유출을 방지하기 위한 보안 및 개인 정보 보호에 도움이 됩니다. 그러나 이 버퍼의 단점은 사용하는 사람들이 성능을 발휘할 수 있도록 buffer.allocUnsafe()로 마이그레이션하여 피할 수 있다는 것입니다. Node.js 사용자들은 이러한 위험을 인식하고 그러한 문제들을 피하는 방법을 알고 있는 경우에만 이 기능을 사용하는 것이 좋습니다.

<!--
**WHATWG URL Parser is Now Stable**
-->
**WHATWG URL 구문 분석기가 안정화되었습니다**

<!--
WHATWG URL parser goes from experimental status to fully supported in this version, allowing people to use a URL parser that is compliant to the spec and more compatible with the browser. This new URL implementation matches the URL implementation and API available in modern web browsers like Chrome, Firefox, Edge and Safari, allowing code using URLs to be shared across environments.
-->
WHATWG URL 구문 분석기는 실험적인 상태로 이 버전에서 완전하게 지원되며, 사람들이 규격을 준수하고 브라우저와 호환되는 URL 구문 분석기를 사용할 수 있습니다. 이 새로운 URL 구현은 Chrome, Firefox, Edge 및 Safari와 같은 최신 웹 브라우저에서 사용할 수 있는 URL 구현 및 API와 일치하며 URL을 사용하는 코드를 여러 환경에서 공유 할 수 있습니다.

<!--
**Performance, Security and Interface Boost in npm@5**
-->
**npm5로 인해 성능, 보안과 인터페이스 향상**

<!--
Npm, Inc. [recently announced the release of version 5.0.0](https://medium.com/npm-inc/npm-5-is-now-latest-d674e9e3b0ec) of the npm client and we are happy to include this new version within Node.js 8.
-->
[최근에 5.0.0 버전 릴리스를 발표](https://medium.com/npm-inc/npm-5-is-now-latest-d674e9e3b0ec)한 npm 클라이언트의 주식회사 Npm과 우리는 Node.js 8 안에 새로운 버전이 포함돼서 기쁩니다.

<!--
Common package management tasks such as package installation and version updates are now up to five times faster; lockfiles ensure consistent installations across development environments; and a self-healing cache with automatic error recovery protects against corrupted downloads. npm@5 also introduces SHA-512 code verification.
-->
패키지 설치와 버전 업데이트 같은 일반적인 패키지 관리 과제들은 지금 5배나 빨라졌습니다. lockfiles는 개발 환경 전반에 걸쳐 일관된 설치를 보장합니다. 그리고 자동 오류 복구 기능을 가진 복구 캐쉬는 손상된 다운로드를 방지합니다. npm 버전 5 또한 SHA-512 코드 검증을 도입하였습니다.

<!--
“Since npm first shipped with Node.js in 2011, our mission has been to reduce friction for Node.js developers and help people build amazing things. Using Node.js 8 with npm@5 will make modular software development dramatically faster and easier — it’s the largest performance improvement ever,” said Isaac Z. Schlueter, CEO of npm, Inc. “We’re proud of our commitment to the Node.js community, and collaboration to bring innovative products to market. I’m excited to see what comes next.”
-->
“npm이 2011년 Node.js와 함께 처음 나왔을 때부터, 우리의 목적은 개발자들의 마찰을 줄이고 사람들이 대단한 것들을 만드는 것을 도와 주는 것이었습니다. npm 5버전과 함께한 Node.js 8을 사용하는 것은 모듈러 소프트웨어 개발을 극적으로 빠르고 쉽게 만들어 줄 것입니다 - 최대의 성능 향상을 이룰 수 있습니다.” 라고 npm 주식회사의 CEO Isaac Z. Schlueter가 말했습니다. “우리는 Node.js 커뮤니티에 대한 우리의 헌신적인 노력과 혁신적인 제품을 시장에 나오기 위한 협력을 자랑스럽게 여깁니다. 저는 다음에 무엇이 나올지 기대됩니다.”

<!--
**Insights to the Tooling Ecosystem and Debugging**
-->
**도구의 생태계와 디버깅에 대한 통찰력**

<!--
This release line will provide deep insight via the new tracing and async tracking features. The experimental ‘async_hooks’ module (formerly ‘async_wrap’) received a major update in Node.js 8\. This diagnostics API allows developers to monitor the operation of the Node.js event loop, tracking asynchronous request and handles through their complete lifecycle and enabling better diagnostic tools and other utilities.
-->
이 릴리스는 새로운 트레이싱과 비동기 트랙킹 기능 같은 깊은 통찰력을 제공합니다. 실험적으로 ‘async_hooks’ 모듈은 (이전에는 ‘async_wrap’) Node.js 8에서 주요 업데이트를 받습니다. 이
진단 API는 비동기 요청을 트랙킹 하고 그것의 완전한 생명주기를 제어하여 개발자들이 Node.js 이벤트 루프에서의 동작을 모니터링 할 수 있게 하고 더 나은 진단 툴과 다른 유틸리티들을 활성화 할 수 있습니다.

<!--
These additions, along with the removal of the legacy debugger (which is replaced by the newer CLI debugger that landed in v7) make it easier to debug and track changes within Node.js, allowing commercial and open source tooling vendors to pinpoint performance degradation in Node.js applications.
-->
이러한 추가 기능은 레거시 디버거를 제거하는 것(v7에 새로 추가된 최신 CLI 디버거로 대체)과 더불어 디버그를 손쉽게 할 수 있도록 하고 상업용 및 오픈 소스 툴 벤더가 성능 저하를 일으키는 애플리케이션의 성능 저하를 찾아낼 수 있도록 하기 위해 Node.js 내의 변경 사항을 추적합니다.

<!--
Another experimental feature added to this release includes JS bindings for the inspector. The new inspector core module enables developers to leverage the debug protocol used by the Chrome inspector in order to inspect currently running JavaScript code.
-->
인스펙터를 위한 JS 바인딩과 같은 다른 실험적인 기능들도 이번 릴리스에 포함됩니다. 새로운 인스펙터 핵심 모듈은 개발자가 현재 실행 중인 자바스크립트 코드를 검사하기 위해 Chrome 인스펙터에서 사용하는 디버그 프로토콜을 활용할 수 있게 해 줍니다.

<!--
**Improved Support for Promises**
-->
**Promises를 위한 향상된 지원**

<!--
Node.js includes a new util.promisify() API that allows developers to wrap callback APIs to return Promises with little overhead, using a standard API.
-->
Node.js는 API를 사용하여 콜백 API를 반환할 수 있도록 개발자가 콜백 API를 래핑 할 수 있는 새로운 util.promisify() API를 포함하고 있습니다.

<!--
For all of our major updates, please go to our technical blog and read more [here](https://nodejs.org/en/blog/release/v8.0.0/).
-->
주요 업데이트를 모두 보려면 기술 블로그를 보거나 [여기](https://nodejs.org/en/blog/release/v8.0.0/)를 읽어보시기 바랍니다.
