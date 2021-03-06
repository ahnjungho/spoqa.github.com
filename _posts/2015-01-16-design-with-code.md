---
layout: entry
title: 코드로 디자인하기
author: yeun
author-email: yeun@spoqa.com
description: 회사에서 개발 환경을 갖추고 Git을 사용하기 시작한 지 약 10개월이 되었으므로 그간의 소회를 적어봅니다.
publish: true
---


회사에서 개발 환경을 갖추고 코드로 디자인하기[^1] 시작한 지 약 일 년이 되었으므로 그간의 소회를 적어 보려 합니다. 이 글은 디자이너와 개발자, 관리자 모두를 대상으로 작성되었습니다. 도입 초기, 도입 직후는 어떻게 도입을 해야 하는지, 디자이너는 이 상황을 어떤 식으로 이해하는지를 중점적으로 서술했습니다. 도입 중기, 도입 안정기에서는 디자이너로서 그전과의 차이, 느낀 점 등을 기술했습니다.

## 도입 동기

동기는 아마 많은 비주얼 디자이너[^2]와 개발자[^3]가 공감할 수 있을 것입니다. 

일반적으로 서비스를 만들 때 그 시각적인 영역을 디자인하는 것은 디자이너지만 그것을 실제로 구현하고 세상에 내놓는 것은 개발자입니다. 그렇다 보니 디자이너의 의도가 구현 과정에서 제대로 살아나기 어려운 구조가 형성됩니다. 똑같은 악보를 보고도 전문 피아니스트가 연주하는 것과 프로 야구 선수가 연주하는 것은 다르고, 똑같은 레시피라도 전문 요리사가 요리한 것과 일류 오페라 가수가 만든 요리의 맛이 다르기 마련입니다. 아무리 뛰어난 디자이너와 뛰어난 개발자가 만났더라도 구현 결과물이 시안과 완벽히 일치할 수 없는 것은 어찌 보면 당연한 일입니다.

문제는 이 결과물이 서비스의 질과 긴밀하게 접해 있다는 점입니다. 그러므로 디자이너는 디자이너대로 답답하고 개발자는 개발자대로 억울합니다. 디자이너는 개발자가 시안을 무시하고 대충 작업한다고 생각하기가 쉽고, 반면에 개발자는 해줄 만큼 해줬는데 사소한 것으로 자기를 타박한다고 느끼는 일이 왕왕 발생합니다. 

이런 피곤한 감정 속에서 디자인 시안을 실제로 구현하기 위해 개발과 피드백과 개발과 피드백과 개발의 고난한 과정을 거치고 있노라면, 디자이너도 개발자도 ‘이제 어느 정도 됐으니 이 정도에서 끝내고 싶다.'라는 생각을 하게 됩니다. 두 사람(혹은 그 이상)이 들이는 에너지에 비해 얻는 결실은 너무 작은 것이지요.

이런 이유도 있었습니다. 사정상 많은 인력을 충원해둘 수 없는 스타트업에서는 디자인 구현이 우선순위에서 자주 밀리게 됩니다. 디자이너 입장에선 `#f0f` 으로 잘못 들어간 색은 고려할 것도 없는 ASAP 이슈지만 개발자 혹은 경영자의 입장에서는 터지고 있는 서버를 정상화하고, 새로운 기능을 추가하고, 버그를 잡는 게 우선일 수밖에 없습니다. 그렇다면 이런 간단하고 (디자이너 입장에선) 시급한 이슈들은 디자이너가 처리할 수 있지 않을까요? 

그래서 이 **답답**한 병목을 타개해 보고자 스포카에선 디자이너도 코드에 바로 접근해서 수정 권한을 가져야 한다는 의견이 형성됐고 모두 반겼습니다.

## 도입 초기 - 제반 시스템 갖추기

### 개발 환경 설치

하지만 강력한 동기에 비해 첫 도입이 쉽지만은 않았습니다. 허들은 우선 개발 환경 설치부터였습니다. 이 문제는 [Docker](https://www.docker.com)를 사용하면 해결된다고 알고 있습니다만 그 당시에는 회사에 Docker를 도입하기 전이었기 때문에, 저와 제 컴퓨터에 개발 환경을 세팅해 준 개발자는 반나절 정도는 다른 업무를 할 수 없었던 것으로 기억합니다. 제 경우엔 제 어려움을 잘 들어주는 동료 개발자[^4]가 있었기 때문에 비교적 수월하게 도움을 받을 수 있었지만, 그렇지 않았더라면 좀 더 설치가 어렵지 않았을까 생각됩니다. 만약 가능하다면 Docker 등을 사용해 개발 환경 설치 프로세스를 먼저 갖춰 놓는 것이 시간 단축에 도움이 될 것입니다.

### 서버 켜기

설치 이후에도 서버를 켜는 일에서 자주 난관을 맞았습니다. 순수한 디자이너의 시선에서 이 과정은 아래와 같이 느껴졌습니다.

1. 개발자가 일러준 터미널 명령어를 노트에 적어 둔다.
2. 서버를 켜려고 할 때마다 적어둔 명령어를 순서대로 입력한다.
3. 뭔가 문제가 생긴다.
4. 개발자를 부른다.
5. 서버가 켜졌다.

이 과정에서 궁극적인 문제 상황은 아래와 같습니다. 

* `3.`에서 일어난 문제가 뭔지 모른다. 
* `3.`에서 일어난 문제를 해결하는 방법을 모른다.

서로 다른 업무를 하는 중에 서버를 켜 달라고 부탁하는 것도 귀찮은 일일 테니 나중에는 개발자가 사용하는 **해결책 명령어**(`pip install -e.`등등)를 따로 적어 두고 문제가 발생할 때마다 하나씩 입력해보았습니다. 그렇게 해도 되지 않을 때는 `4.` `5.`를 반복했습니다.

지금은 **해결책 명령어** 리스트가 꽤 쌓인 데다, 자주 발생하는 문제 상황은 원인을 인지하고 있어서(Postgres를 켜지 않았다든가) 처음만큼 개발자를 부르는 일이 많진 않습니다. 하지만 이 역시 GUI로 서버를 켜고 끄는 환경을 구축할 수 있다면 도입 초기에 만나는 허들을 많이 완화할 수 있을 것입니다.

### Git 이해하기 [^5]

회사에서는 디자이너에게 Git을 이해시키기 위해 따로 스터디를 개설했습니다. 이 과정에서 [GitHub](https://GitHub.com)과 [SourceTree](http://www.SourceTreeapp.com), [Sublime Text](http://www.sublimetext.com)를 사용했습니다. 

#### Git

이 글을 읽는 비개발자를 위해 잠시 설명하자면 Git은 '버전 관리 시스템'입니다. 계좌에 돈을 이체하거나 출금하면 거래 명세에 몇 시 몇 분에 누가 어떻게 돈을 옮겼는지 기록이 남는 것과 비슷합니다. 코드를 수정하면 그 수정 명세를 볼 수 있고, 그게 어떤 것을 위한 수정이었는지 노트도 남길 수 있습니다. 좀 더 자세한 설명을 원하는 사람들은 [이 문서](http://Git-scm.com/book/ko/v1/%EC%8B%9C%EC%9E%91%ED%95%98%EA%B8%B0)를 참고해 보시면 도움이 될 듯합니다. (혹은 지나가는 개발자에게 물어보는 게 빠를 수도 있습니다.) Git은 그 개념을 완벽히 이해하지 않더라도 사용하는 데 큰 지장은 없습니다. <del>(만 부들부들하는 개발자가 있으려나요?)</del> 마치 디자이너가 벡터의 렌더링 방식을 이해하지 못해도 디자인을 하는 데 큰 어려움이 없는 것과 같습니다. <del>(부들부들)</del>

#### GitHub [^6]

GitHub이 무엇인진 모르더라도 개발 직군과 밀접히 일하는 디자이너라면 한 번쯤은 들어봤을 겁니다. [GitHub](https://GitHub.com)이나 [BitBucket](https://bitbucket.org)은 버전 관리를 쉽게 해주는 서비스입니다. 디자이너라면 [Pixelapse](https://www.pixelapse.com)의 개발자 버전이라고 생각하면 됩니다. GitHub을 사용하면 코드의 히스토리를 관리하는 것 외에도 `이슈`를 적어 어떤 일을 해야 하는지 효과적으로 정리할 수 있습니다. 일종의 to-do 시스템이라고 볼 수도 있습니다. 

스포카에선 디자이너가 개발 환경을 갖추기 전부터 크리에이터팀 전원이 이슈 베이스의 업무 프로세스[^7]를 구축하고 있어서 GitHub에서 이슈를 만들고 어사인과 라벨을 설정하는 일은 자유롭게 하고 있었습니다. PR에 대해서는 이슈와 비슷하지만 좀 더 코드와 밀접하게 연결된 무엇이고, 그것에 이슈를 언급하면 레퍼런스가 되며 `merge` 상태가 되면 `완료` 됐다는 정도만 알고 있었습니다.

처음 PR을 올리고 머지하는 과정에서는 어떻게 내가 수정한 내용을 PR로 만드는지 헷갈렸습니다. '첫 페이지에 나타나는 초록색 버튼을 누르면 된다'고 외웠는데 그 '초록색 버튼'이 간혹 나타나지 않을 때도 있었기 때문입니다. 그러나 PR을 올린 후에는 이슈를 생성하는 것과 크게 다르지 않으므로 별다른 어려움은 없었습니다.

#### SourceTree

각자가 수정한 코드를 공용의 서버로 올리는 걸 쉽게 해 주는 GUI 툴입니다. 이 작업을 커맨드라인으로 하는 개발자들도 많지만 비개발 직군이 쉽게 따라 할 수 없으므로(게다가 한 번의 실수로 코드를 다 날려 먹을 수도 있으니) GUI툴을 사용했습니다. 디자인은 [GitHub for Mac](https://mac.GitHub.com)이 더 예쁜데 [SourceTree](http://www.SourceTreeapp.com)가 기능이 더 좋다고 하여 선택했습니다. 

Git이나 GitHub은 디자이너가 잘 이해하지 못해도 사용이 별로 어렵지 않았지만, SourceTree는 뭐가 뭔지 이해하는데 좀 시간이 걸렸습니다. (정확히 적으면 SourceTree 프로그램에서 **보이는** Git 시스템을 이해하기 어려웠습니다.) 이 과정에서도 서버를 켤 때와 마찬가지로 컨플릭트가 나거나, 왜인지 모르게 푸쉬가 안되는 등 디자이너가 이해할 수 없는 문제 상황이 자주 발생했습니다. 개발자의 인내심이 필요한 부분입니다. (하지만 스포카의 경우엔 디자이너가 코드를 수정한다는 기쁨에 찬 개발자들이 '스타일 만지는 거에 비하면 그까짓 거’하는 자세로 적극적으로 도와주었습니다.) 

가장 이해하기 어려웠던 개념은 여러 저장소가 있고 거기에 push와 pull을 하는 것이었습니다. 그래프에 익숙해지는데도 시간이 걸렸던 것 같습니다. 이런 개념들은 설명을 듣는 것만으론 부족했고 계속 뭔가를 수정하고 commit, push하면서 익혀야 했습니다.

각 회사의 작업 방식에 따라 다르긴 하겠지만, 제 경험에 비추어보면 commit, push, pull, checkout, branch, stash 만 사용할 수 있어도 큰 문제는 없었습니다. 처음 개발 환경을 도입하고 나서 몇 달이 지나 merge에 대해서 알았고, reset과 rebase는 배웠지만 어려워서 아직 제대로 이해하지 못하고 있습니다만 일을 하는데 별 장애는 못 느끼고 있습니다.

### 코드의 위치 찾기

간과하기 쉬운 부분이지만 처음에는 '서버 켜기'와 '푸쉬 하기' 보다도 더 자주 막혔던 부분입니다. 어딘가를 고치고 싶은데 그게 어느 파일에 적혀있는지 찾기가 어려웠습니다. 
처음 개발환경을 세팅할 때 개발자도 디자이너도 수정해봤자 CSS나 HTML 코드만 고치면 되겠다고 생각했지만, 실상은 JavaScript에서 스타일을 먼저 던져주는 경우, Python 혹은 다른 언어들로 클래스가 삽입되는 경우 등등 '무엇이', '어디에' 있는지 아는 건 녹록잖은 일이었습니다. 

이 부분은 아직도 완전히 해결되진 못했지만, 자주 코드를 보다 보니 직감(?) 이 생겨 대강 어디에 뭐가 있는지를 찾을 수 있게 되었습니다. 디자이너가 개발환경을 세팅할 때, 대강의 코드 구조를 교육해주는 것이 좋겠습니다.

## 도입 직후 - 작은 것 고쳐나가기

이제 본격적으로 디자이너가 스스로 헤쳐나가야 하는 부분입니다. 바로 코드를 직접 고쳐나가는 일입니다. 개발 환경이 세팅되고서 가장 먼저 한 일은 개발자가 부트스트랩으로 대강 만든 사내 서비스의 디자인을 대폭 수정하는 일이었습니다. 처음 개발 환경을 도입했을 때 HTML과 CSS에 대한 지식수준은 별로 좋지 않았고 그 외의 언어들에는 지식이 전혀 없었습니다. 정확한 이해를 돕기 위해 그 당시의 제 지식수준을 상세히 적어봅니다.

##### HTML

* `div`와 `span`의 차이를 대강 이해하고 있었다.
* `legend` `form` `fieldset`등을 몰랐다. 회사 코드에서 처음 봤을 때는 뭔가 `div` 비슷한 거라고 이해했다.
* `button`이 버튼 모양의 요소를 뜻한다고 이해하고 있었다.[^8]
* `input type`에 `text`는 문자, `radio`는 라디오버튼, `checkbox`는 체크 박스라는 정도로 이해하고 있었다. 그 외의 타입은 몰랐다.
* `ul` `ol` `li`가 리스트와 관련된 서로 뭔가 다른 거라고 이해하고 있었다.
* `table`과 관련된 `thead` `tbody` `tr` `th` `td`를 전혀 몰랐다.
* `class`와 `id`가 다르다는 것은 알지만, 뭐가 다른지는 몰랐다.


##### CSS

* `Inspect Element`를 켜고, 내용을 보는 방법을 알고 있었다.
* `class` 이름을 앞에 적고 `{ }`안에 스타일을 쓴다고 이해하고 있었다.
* 셀렉터 앞에 `.`을 찍으면 class고 `#`을 쓰면 id라고 이해하고 있었다.
* `font` `font-size` `background`등의 기본적인 스타일 옵션을 이해하고 있다.
* `HTML tag` `class` `id`를 제외한 셀렉터에 대해서는 몰랐다.
* `margin` `padding`이 간격에 대한 것이라고 이해하고 정확한 차이는 몰랐다.
* `pseudo class`는 전혀 몰랐다.

보면 알겠지만, 아주 기본적인 내용만 이해하고 있었습니다. ‘일주일 만에 배우는 HTML과 CSS’ 같은 기성 강의를 들으면 얻을 수 있는 지식수준이었습니다. <del>(적으면서도 놀랍군요.)</del> 그러므로 아마 백지의 문서에 0번째 줄부터 스타일을 작성하라고 했다면 훨씬 어려웠거나 좌절하고 시도하지 않았을 것입니다. 하지만 기존의 코드를 보면서 이미 작성된 스타일 코드에 값만 바꿔주는 일은 그리 어렵지 않았습니다. 물론 디자이너가 머릿속에서 상상하는 부드러운 모바일 대응과 패러럴랙스 스크롤 같은 것은 구현할 수 없었지만 단지 색상과 보더, 여백 옵션을 수정하는 것만으로도 큰 디자인 개선을 이룰 수 있었습니다.

이렇듯 지식이 부족한데도 문제없이 오히려 순조롭게 도입이 가능했던 이유로 몇 가지를 유추해보면, 아래와 같습니다.

* 스타일 코드를 수정할 수 있는 충분한 프론트 엔드 인력이 없다.

> 서툴더라도 디자이너가 스타일을 손 보는 게 팀 전체의 생산성에 도움이 되는 상황이었습니다. 고급 인력인 개발자가 색상이나 여백을 조정하는데 많은 시간을 들일 수 없었기 때문입니다. 반면에 디자인팀에서는 스타일 수정을 우선순위가 높은 것으로 판단했기 때문에 인력을 충분히 투입할 수 있었습니다.

* 그럼에도 불구하고 헤매고 있을 때 옆에서 도와줄 수 있는 개발자가 있다.

> 정말 인력이 부족해서 디자이너가 난관을 맞이했을 때조차 혼자서 해결해야 했다면 의욕이 저하되고 생산성도 무척 떨어졌을 것입니다. 하지만 다행히도 개발자에게 간간이 질문을 하고 도움을 받을 수는 있었습니다. 모두가 바쁠 경우에는 구글에서 해당 내용을 찾아 이걸 읽어보라고 링크를 던져주기도 했습니다. 코드를 보는 것에 조금 익숙해진 후에는 개발자가 직접 수정해주는 것보다 스스로 배우고 공부할 수 있도록 소스를 제공해 주는 것이 좋았습니다.

* 수정해야 할 스타일이 무척 많았다.

> 아주 미세한 조정을 손보는 게 아니라 하나도 디자인되지 않은 스타일을 손보는 것이었기 때문에, 낮은 지식수준에서도 고칠 수 있는 영역이 많이 있었습니다. 그래서 어떤 부분을 어떻게 수정해야 할지 구상하기 쉬웠고, 가시적으로 변화가 많으니 디자이너 입장에서도 보람이 컸습니다.

* 스타일을 수정할 서비스가 부트스트랩 기반의 웹이었다.

> iOS, Android 혹은 데스크탑 프로그램이었다면 새로운 언어에 대한 장벽을 넘기 힘들었을 것입니다. 또한, 디자인을 다 뜯어고쳐야 했지만, 부트스트랩 자체는 매우 정리가 잘 된 framework이기 때문에 이상하게 쓰인 코드를 보고 혼란에 빠지는 일은 없었습니다. 오히려 보고 배울 점이 많았습니다.

* 스타일 코드를 고치고자 하는 디자이너의 동기가 강력했다.

> 앞서 적었듯이 개발 인력이 부족했기 때문에, 디자인팀 입장에선 ASAP인 이슈들이 꽤 오랫동안 해결되지 못한 채 남아있었습니다. 몇 달 동안 말해도 우선순위에서 밀려 고쳐지지 않던 디자인을 직접 수정할 때의 카타르시스는 가장 강력한 동기일 것입니다.

코드로 디자인을 수정하는 초기 단계에서는 이것을 하고자 하는 디자이너의 동기와 그것을 지지해주는 동료들이 가장 중요하다고 생각합니다. 코드에 대해 잘 모른다 하더라도 CSS는 직관적인 언어기 때문에 생각만큼 장벽이 높진 않습니다. 당연히 처음부터 엄청난 애니메이션이나 트랜지션 효과를 구현할 수는 없으므로 난이도 대비 가시적 효과가 큰 수정(예를 들면 폰트를 굴림에서 SD 고딕으로 바꾼다든가)부터 시작해보는 것이 좋습니다.

언어는 제가 회사 코드를 처음 접했을 때는 CSS로 되어있었고 그 후 얼마지 않아 LESS로 변경했다가 현재는 SCSS를 사용하고 있습니다. 처음 문법을 이해하기는 CSS가 간단해서 좋지만, 사용하기는 SCSS가 훨씬 편하므로 SCSS기반에서 처음에는 CSS를 사용하다가 점차 SCSS를 사용하는 것을 추천합니다.

## 도입 중기 - 사고의 변화

모든 디자이너는 자신의 시안 속에서 일정한 규칙을 만들어 철저하게 지킵니다. 일테면 레이아웃을 12 column으로 잡고 gutter를 30px 씩 준다든가, 파란색은 `#5e89d6` 로만 사용한다든가, 폰트 크기는 16px과 32px만 쓴다든가 하는 기준입니다. 그것을 개발자가 구현하는 데는 크게 두 가지 문제가 있습니다.

* 디자이너가 세운 규칙이 뭔지 모른다.
* 디자이너가 세운 규칙이 실제로 코드로 구현되기 부적절하다.

경험적으로 디자이너가 세운 규칙이 코드로 구현되기 부적절 해서, 개발자가 그 규칙을 '규칙이라고' 이해하지 못하는 상황이 잦았습니다. 이는 지식의 차이 때문에 발생합니다. 가장 대표적인 예가 버튼의 크기입니다. 

### 디자이너(그래픽 툴)와 개발자(코드)의 차이 이해

제가 코드에 대한 지식이 전혀 없을 때 버튼 크기를 `높이 40px` `가로 100px` 그리고 `텍스트는 12px로 중앙정렬`이라고 생각했습니다. 왜냐하면, 그래픽 툴에서는 `40px * 100px` 의 사각형 레이어를 그리고 텍스트 레이어를 만들어서 그 둘을 중앙정렬 시키기 때문입니다.

<img src="/images/2015-01-06/wrong_guide.png"
     style="margin-right:auto; margin-left:auto;" />

그리고 위와 같이 가이드를 쳐서 개발자에게 줍니다. 그러면 그걸 받은 개발자는 `-_- (...)` < 이런 표정을 지은 뒤 묵묵히 시안과는 다른, 그러나 나름 노력한 버튼을 구현합니다.
당연히 버튼의 크기는 **대략** `40px * 100px`입니다. 그러면 디자이너는 대체 왜 이런 간단한 내용도 정확히 구현할 수 없는 것인가 충격에 빠지게 됩니다. 디자이너가 자신의 의도를 실현하기 위해 전달했어야 하는 가이드는 - 궁극적으로 수립했어야 하는 규칙은 다음과 같습니다.

<img src="/images/2015-01-06/right_guide.png"
     style="margin-right:auto; margin-left:auto;" />

이런 일이 디자인 시안을 코드로 구현하는 과정에서 비일비재하게 일어납니다.

웹 디자이너가 그래픽 툴에서 선을 하나 그릴 때는 높이 1px 짜리의 길쭉한 박스를 그려 넣으면 됩니다. 무척 쉬운 일이고 그 수정 또한 쉽습니다. 하지만 개발자가 그것을 구현할 때는 그 크기만큼의 `div`에 `padding`과 `margin`을 계산해야 합니다. 그러나 대체로 디자이너들은 `padding`과 `margin`을 **여백**이라는 하나의 개념으로 생각하기 때문에 규칙이 정확하지 않은 시안으로 그것을 정확히 구현하는 데는 한계가 있습니다. 마치 금박을 사용한 인쇄물 시안을 CMYK로만 출력해서 원하는 결과물을 얻어낼 수 없는 것과 같습니다.

코드로 디자인을 수정하기 시작하면서 얻은 가장 큰 성과는 구현할 수 있는 규칙을 이해하게 됐다는 점입니다. 나아가 어느 정도 HTML의 구조와 스타일 코드 작성에 눈이 익으니 스스로 규칙을 세울 수 있게 되었습니다. 이렇게 세운 규칙은 그래픽툴 속에서 세운 규칙보다 더 체계적이며 실현 가능하고 문제 상황에 집중되어 있습니다.

### 병렬적 사고

대부분의 디자인 프로세스는 직렬적으로 진행됩니다.

1. 1차 시안 완성
2. 2차 시안 완성
3. 3차 시안 완성
4. 최종 시안 완성

그러나 코드로 디자인을 구현할 때는 병렬적으로 사고해야 합니다.

- 네비바 완성
- 컨텐츠 완성
- 버튼 완성
- 푸터 완성

병렬적 사고방식이 디자인에 언제나 도움이 된다고 말하기는 힘듭니다. 디자인이라는 분야의 특성상 모든 디자인 구성요소가 서로에게 긴밀한 영향을 주고받기 때문입니다. 그래서 디자인을 코드로 하면서부터는 반병렬적인 방식을 사용하고 있는데, 아래와 같습니다.

- 1차 시안 완성
  - 네비바 완성
  - 컨텐츠 완성
  - 버튼 완성
  - 푸터 완성

- 2차 시안 완성
  - 네비바 완성
  - 컨텐츠 완성
  - 버튼 완성
  - 푸터 완성

  ...

마치 직렬적 디자인과 같아 보이지만, 각 요소(네비바, 컨텐츠, 푸터 등)들을 단계적으로나마 완결짓고, 그것을 여러 번 반복한다는 점에서 차이가 있습니다. 이는 코드로 디자인하는 장점이라기보다는 툴이 달라서 발생하는 차이라고 생각합니다. 같은 어도비 제품이라도 일러스트레이터에서 레이어를 관리 하는 것과 포토샵에서 관리 하는 방법이 다른 것처럼 툴이 사고방식을 전환시키는 것입니다. 지금은 익숙해졌지만, 코드로 디자인을 처음 시작할 때는 디자인을 영역별로 나눠 생각한다는 점이 혼란스러웠습니다. 어디서 단계를 끊어야 할지 판단이 어려웠기 때문입니다.

반 병렬적 작업 방식의 좋은 점은 짧은 시간 안에 중간 결과물을 얻어낼 수 있는 점입니다. 전통적인 방식에선 디자이너가 완결된 시안을 만들기 위해서 여러 고민과 시도를 해보는 동안 팀원들은 대체 결과물이 언제 나오는지 이제나저제나 하게 됩니다. 작업하는 디자이너도 그런 사실을 알기 때문에 고민이 길어질수록 조급한 마음이 들게 마련입니다. 하지만 병렬적 디자인을 하면 부분적으로 완결을 짓기 때문에 다른 팀원이 진행 상황을 이해하기가 쉽습니다. 또한 '이것은 100% 완성된 시안이 아니다.'라는 것을 모두가 알기 때문에 디자이너 입장에서도 더 마음 편하게 시안을 공유할 수 있습니다. 비록 나중에는 수정하게 되더라도 팀원들이 현재 디자이너가 무슨 작업을 하고 있고, 그게 어떻게 디벨롭되어가는지 같은 그림을 그릴 수 있는 것입니다.

## 도입 안정기 - 사고의 확장

8개월 정도가 지난 시점에서는 어느 정도 코드에 익숙해지고 개발 문서를 대강 읽을 수 있게 되었습니다. 도입 직후와 비교했을 때, 새로 알게 된 점들은 다음과 같습니다.

##### HTML, JavaScript 등등

* 위에 적은 도입 직후에 몰랐던 것들을 이해하게 됐다.
* HTML 속에 섞여 있는 다른 언어들을 보고 판별할 수 있다.
* API 문서를 보고 HTML 외의 언어로 구성된 라이브러리의 스타일을 수정할 수 있다.
* class를 일정하게 구성하고 계획할 수 있다.
* 서버와 연동되지 않는 선에서 스스로 HTML 문서를 작성할 수 있다.
* SVG 아이콘을 제작해서 적용할 수 있다.

##### CSS, SCSS

* 위에 적은 도입 직후에 몰랐던 것들을 이해하게 됐다.
* CSS, SCSS로 스타일 코드를 작성할 수 있다.
* 모바일 대응에 대한 레이아웃을 스스로 설계하고 구현할 수 있다.
* 원하는 스타일이 있으면 찾아서 구현할 수 있다.
* [CSS-tricks](http://CSS-tricks.com)등을 보고 이해할 수 있다.

##### 그 외

* 전보다 literacy가 상승했다.
* 버전 관리의 중요성을 알게 됐다.
* 디자인 가이드라인이 지향할 방향을 이해하게 됐다.

**도입 초기**에는 현재 무슨 일이 일어나고 있는지 몰랐다면, **직후**에는 무엇을 해야 하는지 몰랐습니다. 그러던 것이 **중기**즈음에 다다라서 무엇을 어떻게 해야 하는지 아는 상태로 변했고, **안정기**에는 나아가 미래에 어떻게 해야 할지를 계획할 수 있게 되었습니다.

publish 된 결과물이 같아 보이더라도 어떤 방법을 사용했는지에 따라 미래에 확장 가능할 수 있고 전혀 쓸 수 없을 수도 있습니다. 일테면 로고를 Smart Object로 만들어 놓으면 수정 시 모든 시안에 일괄 반영되는데 일반 Layer로 만들어놓으면 일일이 로고가 들어간 부분을 찾아서 수정해줘야 하는 것과 같습니다. 디자인 코드 역시 같은 상황이 발생합니다. 하지만 무척 경험이 많지 않고서는 개발자가 미래를 대비한 계획을 잘 세우기 어렵습니다. 디자인의 비전이나 디테일의 지향점, 구체적인 결과물의 형태를 마치 디자이너처럼 미리 구상할 수 없기 때문입니다. 따라서 최악의 경우에는 디자인을 교체할 때마다 코드를 새로 짜야 하는 상황이 발생할 수 있습니다. 주로 모바일 뷰를 새로 만든다거나, 외주로 작업한 경우에 빈번히 발생하는 일입니다.

디자이너가 직접 코드로 디자인하기 시작한 지 8-10개월 즈음에는 스타일 코드가 관리되어야 할 방향을 설정하게 되었습니다. 기본적인 규칙으로 가져갈 스타일과 그 변칙들을 함수화하고 각 서비스, 각 화면의 코드가 어떤 문서에 어떻게 적혀있어야 할지를 구상할 수 있게 되었습니다. 약 1년 전과 비교하면 실로 놀라운 변화입니다.

스포카에서는 이제 디자이너가 코드로 디자인하는 것이 안정기를 지나, 정착기에 접어들고 있습니다. 결론적으로 코드로 디자인을 하면서 얻는 장점이 단점보다 훨씬 크기 때문에 앞으로도 이 방향을 고수해나갈 예정입니다.

마지막으로 그간 느꼈던 장단점을 정리해보겠습니다.

## 결론

### 장점 - 속도의 향상

믿기 힘들겠지만, 코드로 디자인을 하는 게 그래픽툴로 디자인을 하는 것보다 훨씬 빠릅니다. 구체적으로 표현하면 코드로 한번에 수정이 되는 부분과 그래픽 툴에서 한 번에 수정이 되는 부분이 다른데, 전자가 커버하는 영역이 훨씬 큽니다. 예를 들면 아래와 같은 상황입니다. (그래픽 툴이나, 코드에서나 가장 일반적이고 간단한 상황을 가정했습니다.)

- 모든 버튼의 모서리 값을 수정한다.
  - 그래픽 툴: 모든 버튼의 path를 선택한 다음에 모서리 값을 수정한다.
  - 코드: `$button-radius-base` 의 값을 수정한다.

- 모든 선의 색상을 수정한다.
  - 그래픽 툴: 하나의 solid color 레이어를 모든 선 레이에 clipping 시켜놓고 solid color를 수정한다.
  - 코드: `$border-color-base`를 수정한다.

- 정렬되어있는 요소 중 하나의 크기를 수정한다.
  - 그래픽 툴: 크기를 수정한 후 정렬이 흐트러진 요소들을 다시 정렬한다.
  - 코드: 크기를 수정하면 나머지는 알아서 따라 움직인다.

- `더 보기 >` 라고 되어있는 링크의 기호를 `더 보기 +` 라고 수정한다.
  - 그래픽 툴: 텍스트 레이어를 선택해 `>`를 지우고 `+`라고 친다. 모든 텍스트 레이어에 반복한다.
  - 코드: `.more-info:after`의 `content`를 `>`에서 `+`로 바꿔준다.

- 전체 여백을 수정한다.
  - 그래픽 툴: 모든 레이어를 선택해 원하는 만큼 옮겨준다.
  - 코드: `.wrapper`의 `padding`을 원하는 px 값으로 수정한다.

반면에 코드로 수정하는 것이 더 번거로운 것은 다음과 같습니다.

- 컬러를 고르기: 이미 정해진 컬러값이 없다면 코드로 컬러를 고르는 것은 거의 불가능합니다.
- 원하는 크기의 선 그리기: 때에 따라 box 모델을 새로 그려야 할 수 있습니다.
- 원하는 위치로 오브젝트를 옮기기: display 설정에 따라 오브젝트가 마음대로 움직이지 않을 수 있습니다.
- 모르는 기능을 찾기: 무언가를 하고 싶어서 그것을 인터넷상에서 찾을 때 코드에 익숙하지 않으면 해결책을 판단하고 실행하기 어려울 수 있습니다.

예로 든 것처럼 같은 결과물을 얻기 위해 작업해야 하는 방식이 전혀 다르기 때문에 속도의 차이가 발생합니다. 하지만 그래픽 툴로 그리는 것은 '시안'인데 반해, 코드로 구현하는 것은 실제 '결과물'이기 때문에 총 작업 시간은 코드로 하는 것이 더 짧습니다. 다른 측면으로는 개발자가 '지금 하고 있는 일을 모두 완료하고 남는 시간에 텍스트 정렬을 수정해주는 것'보다 '디자이너가 헤매면서 left라고 쓰인 어딘가를 찾아 right라고 써주는 것'이 더 빠르기 때문이기도 합니다.

같은 작업을 하는데 드는 시간이 더 짧다는 것은, 다시 말하면 같은 시간 안에 더 많은 시안을 만들어보고 더 많은 디테일을 챙길 수 있다는 뜻입니다. 디자이너는 더 많은 디자인을 해볼 수 있어서 좋고, 회사의 입장에서는 같은 기간 내에 더 완성도 높은 결과물을 얻을 수 있어서 좋습니다. 더불어 디자이너가 디자인 구현까지 담당하게 되면서 그 시간에 개발자는 다른 개발 업무에 집중할 수 있으므로 전체적인 생산성이 상승하는 효과가 있습니다.

### 장점 - 가이드의 종말

디자이너들이라면 가이드를 치는 스트레스를 모두 알고 있습니다.

- 어떤 색상은 Hex로 알려달라고 하고 어떤 색상은 RGBA로 알려달라는데 무슨 차이지?
- 1~2px 요소는 크기가 작아서 가이드 정보를 쓰기가 애매하군.
- 문자로는 표현이 안 되는 정보들이 있는데...
- 같은 속성을 가지는 폰트 스타일을 모든 텍스트에 일일이 써줘야 할까?
- 애니메이션 효과는 어디다 어떻게 써야 좋을까?
- 이건 구현된걸 보고 조정하고 싶은데...
- 시각 정보를 문자로 옮기다 보니 너무 복잡해졌는데 이걸 개발자가 보고 이해할 수 있을까?

이런 여러 고민으로 때로는 시안을 치는 것보다 가이드를 치는 게 시간이 더 걸릴 때도 있습니다. 코드로 디자인을 하면서 좋았던 점 중에 하나는 더이상 가이드를 칠 필요가 없다는 점입니다. **네. 가이드를 칠 필요가 없습니다.** 시안이 즉 결과물이기 때문입니다. 따라서 위와 같은 고민을 할 필요가 없습니다. 이 역시 총 작업 시간을 단축해주는 부분이기도 합니다.

### 장점 - 의도한 디자인 반영

디자인을 의도한 대로 구현할 수 있고, 앞서 설명했듯이 디자인 의도를 계획할 수 있습니다. 제품이 시안대로 구현되지 않아서 수정을 요청하면 개발자에게 자주 듣게 되는 말 중에 하나가 "그게 그렇게 안 되어 있다." 입니다. 코드로 디자인을 하면서 비로소 그게 무슨 말인지 깨닫게 되었는데, 말 그대로 그렇게 되어있지 않은 것입니다. 디자인에 비유하자면 다음과 같은 상황입니다.

- 200px 짜리 logo.jpg로 대형 출력물 시안을 만들어달라는 요구
- 인쇄용으로 폰트를 깨서 전송해줬더니 왜 텍스트를 수정할 수 없느냐는 문의
- 아웃포커싱 된 사진의 뒷배경에 초점을 맞춰달라는 요구

이런 상황은 크게 두 가지 원인이 있습니다.

1. 애초에 그런 일(대형 출력, 텍스트 수정)이 생길 것을 예상하지 못하고 작업을 했다.
2. 계획 자체가 실현 불가능(초점의 변화)하다.

코드로 디자인을 하면 위 두 가지 상황에 유연하게 대응하게 됩니다. 의도에 따라 코드 설계를 직접 할 수 있고, 실현 불가능한 계획이 뭔지 알게 되므로 애초에 그런 계획을 하지 않습니다.

이 두가지 원인을 해결함으로써 [3px 논쟁](https://www.gv.com/lib/design-details)[^9] 과 같은 일을 줄일 수 있습니다. 더이상 디자인 버그를 수정하기 위해 '디자인 버그를 잡는 날'이라던가, '디자인 버그 스펙 시트' 등을 만들 필요가 없습니다. 발견하면 스스로 고치고 배포하면 됩니다. 이런 작업 과정은 당연히 제품의 퀄리티 향상과 디자이너의 역량 상승에 이바지합니다. (부수적으로 개발자의 정신 건강에도 도움이 됩니다.)

### 단점 - 교육 비용

디자이너가 코드를 배우는데 비용이 들어갑니다. HTML, CSS들을 배우는 비용도 들어가지만, Git이나 회사의 배포 시스템을 숙지하는데도 짧지 않은 시간이 필요합니다. 스포카에서는 디자이너들이 Git을 사용할 수 있는 정도로 이해하는데 3개월 정도의 시간이 걸렸고, 원하는 디자인을 스스로 할 수 있게 되는 데는 6~8개월이 걸렸습니다. 이 기간에는 동료들(주로 개발자)의 전폭적인 지지가 필요합니다. 초기에는 서버를 켜는 일로만 하루에 네다섯 번씩 개발자를 찾게 되기 때문입니다. 그러므로 만약 디자이너의 동기가 강력하지 않거나, 디자인팀과 개발팀의 소통이 어려운 구조, 교육에 시간을 투자할 수 없는 환경이라면 도입하기 부적절할 수 있습니다. 

### 단점 - 특성의 차이

모든 디자이너가 문자로 디자인하는데 익숙할 수는 없습니다. 개발자마다 선호하는 언어나, 툴이 다르듯이 디자이너도 다양한 성향을 가지고 있습니다. 디자이너들을 예술가에 가까운 디자이너와 건축가에 가까운 디자이너로 나눌 수 있다면 코드로 디자인을 하는 것은 건축가에 가까운 디자이너에게 적합한 방법입니다. 따라서 모든 디자이너가 그래픽 툴이 아닌 코드로 디자인을 해야만 하는 것은 아닙니다. 디자이너 스스로 자신의 성향에 대해 고민해보고, 적절한 디자인 방법을 선택하면 됩니다.

### 단점 - 개발 의존성

그래픽 툴의 가상 공간이 아닌 실제 코드 상에서 디자인을 하다 보니 개발에 의존성이 생기게 됩니다. 일종의 레이어 생성을 개발자가 해줘야 하는 것입니다. 아무리 코드를 직접 쓰더라도 디자이너는 디자이너이기 때문에 서버를 연결해 필요한 데이터를 출력하거나, 외부의 API를 붙이는 것을 할 수는 없습니다. 그래서 개발이 선행되어야 디자인을 할 수 있는 상황이 발생합니다.

## 마치며

코드로 디자인을 하기가 쉽지만은 않습니다. 살면서 한 번도 들어본 적 없는 프로그램들을 익혀야 하고, 예쁘지도 않은 terminal을 보며 얘가 뭐라고 하는 건가 골머리를 싸매야 하기도 합니다. 하지만 이미 많은 디자이너들이 자신이 만든 작업물이 구현되는 과정에 참여하길 원하고 있습니다. 아마 그만큼 많은 개발자들이 이해할 수 없는 가이드를 보며 해독하는 작업에 스트레스를 느끼고 있을 것입니다. 스포카도 그러한 진통을 겪었기에 디자이너도 코드를 다루자는 결단을 내렸고 그 결과는 실로 고무적이었습니다. 디자인/레이블 버그가 점차 사라지는 것을 시작으로 얼마 지나지 않아 생산성을 약 3배 정도[^10] 상승시킬 수 있었습니다. 심지어 디자이너가 코드에 전혀 익숙하지 않았을 때도 전체 생산성은 상승했습니다. 애초에 목적은 '간단한 디자인 버그를 고쳐보자' 정도였지만 지금은 '앞으로 스포카의 스타일 코드는 어때야 하는가'를 고민하면서 되돌아보건데, 코드로 디자인을 한다는 것은 무척 현명한 선택이었습니다.

외주로 작업한다든가, 팀 간의 구분이 확고한 회사 문화 등 모두가 코드로 디자인을 할 수 있는 환경은 아닐 것입니다. 하지만 지난 일 년 동안 새로운 방식으로 작업함으로써 새로이 느낀 점과 배운 점이 컸기에 좀 더 많은 디자이너, 개발자 그 외에 PM이던가 기획자라던가 혹은 매니저들이 디자인을 하고 그것을 구현하는 방법을 한번 고민해봤으면 하는 바람입니다. 

[^1]: 야심 차게 '코드로 디자인하기'라고 적었지만, 실제로 제가 관여했던 많은 부분은 HTML과 SCSS로 구성된 웹이었습니다. 따라서 본 포스트에는 iOS, Android의 디자인 구현에 대한 내용은 없습니다. 하지만 앞으로도 스포카에서는 디자이너가 HTML, SCSS 이외의 언어를 배워나가며 직접 구현과 배포에 참여할 계획이고, 포스트의 내용도 코드 기반의 환경 도입과 그래픽 툴과의 차이에 집중하고 있으므로 'HTML과 SCSS로 디자인하기'보다는 광의적인 '코드로 디자인하기'라고 지었습니다. 이 글에서 '코드로 디자인을 한다'는 것은 개발부터 배포까지를 모두 뜻합니다.

[^2]: 디자이너라는 명칭이 서로 전혀 다른 직군인 UX 디자이너와 UI 디자이너를 아우르기 때문에 비주얼 디자이너라고 명시적으로 적었습니다. 이후부터 '디자이너'라는 표기는 비주얼 디자이너만을 지칭하도록 하겠습니다. 마찬가지로 '디자인'이라는 용어도 '시각 디자인'만을 지칭합니다.

[^3]: 개발자라는 명칭도 '디자이너'처럼 왈가불가가 많은 것으로 알고 있습니다. 이 포스팅에서는 프로그래머, 소프트웨어 엔지니어, 디벨로퍼, 퍼블리셔 등등을 모두 통틀어서 개발자라고 지칭합니다.

[^4]: Jay는 제가 코드로 디자인을 하는데 실로 엄청난 도움을 주었습니다. 이 포스팅을 빌어 고마운 마음을 전합니다.

[^5]: 이 내용은 오로지 제가 이해한 것을 바탕으로 하므로 정확하지 않은 서술이 있을 수 있습니다. 이상한 점을 알려주시면 바로 수정하도록 하겠습니다.

[^6]: 이 글은 GitHub 을 기준으로 작성되었지만, 현재 스포카는 Jira와 Stash를 사용하고 있습니다.

[^7]: 이슈를 기반으로 한 업무 프로세스는, 디자이너뿐 아니라 영업 등 비개발 직군에게도 무척 효과적이었으며 각 직군 간의 신뢰도를 높이는데 이바지했습니다.

[^8]: 이건 아직도 충격적입니다. 처음엔 아무것도 모르고 `a`나 `input`들을 모두 `button`으로 바꾸기도 했습니다.(...)

[^9]: 원글에 이어 여러 개발자들의 반응이 있던 아티클입니다. 그 중 스포카의 개발자인 [홍민희님의 글](http://blog.dahlia.kr/post/77136470479)도 같이 링크합니다.

[^10]: 구체적으로 어떤 기술적, 방법적인 차이가 생산성 향상에 기여했는지도 같이 이야기하고 싶지만 지면 관계상 다음을 기약합니다.

