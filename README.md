<details>
<summary>🇰🇷 한국어로 읽기</summary>

### 안녕하세요. 이주호(Joe Lee)입니다.

보안에 관심을 갖게 된 시작은 제 실수였습니다.

코딩을 하다가 실수로 키를 공개된 곳에 올린 적이 있습니다. 주변 친구들도 비슷한 실수를 한 적이 있었고요.

그때 이런 생각이 들었습니다.

**"나도 이러는데, 다른 사람들도 똑같은 실수를 하고 있지 않을까?"**

그래서 직접 찾아보기 시작했습니다.

여러 오픈소스 도구를 조합해서 공개적으로 배포된 앱을 자동으로 분석하는 시스템을 만들었습니다. 앱 패키지를 내려받아 제 컴퓨터 안에서 분해하고, 실수로 포함된 자격증명이나 설정 파일을 찾는 방식입니다.

처음 돌렸을 때 실제 키들이 나왔습니다.

그런데 너무 많이 나왔습니다.

처음에는 제가 뭔가 잘못 만든 줄 알았습니다. 더미 키나 테스트용 값을 실제 키로 잘못 잡아내는 경우도 있었기 때문입니다. 하나씩 확인하면서 구조 검증 방식을 고쳤고, 그 과정에서 실제 자격증명이 그대로 배포된 사례들이 있다는 것을 알게 됐습니다.

그때부터 단순한 호기심 이상의 일이 됐습니다.

#### 지금까지 발견한 것

현재까지 분석한 앱은 **3,134개**입니다.

* 모바일 앱 1,560개
* 웹 앱 1,574개

분석 과정에서 **2,714개의 노출 항목**을 발견했습니다.

주요 유형은 다음과 같습니다.

* 로그인 없이 접근 가능한 클라우드 DB·스토리지: 약 **1,280건**
* 소스 코드·서버 설정 노출: 약 **310건**
* Slack·Discord 웹훅: 약 **250건**
* Kakao 키: **248건**
* OpenAI 키: **158건**
* GCP 서비스 계정 자격증명: **139건**
* 개인키(PEM): **78건**
* AWS 자격증명: **77건**

그 밖에도 Google OAuth, Telegram, GitHub PAT, Naver 관련 자격증명 등이 있었습니다.

이 숫자는 **"전체 앱 중 몇 퍼센트가 취약하다"는 의미가 아닙니다.**

제가 분석한 대상은 전체 앱 시장을 대표하도록 무작위로 뽑은 표본이 아닙니다. 위 숫자는 제가 조사하면서 발견한 노출들을 집계한 결과일 뿐입니다.

그 안에는 규모가 큰 회사의 앱도 있었고, 아이들이 사용하는 앱도 있었습니다.

특히 놀랐던 것은 **139개 앱에서 GCP 서비스 계정 자격증명이 배포 파일 안에 포함되어 있었다는 점**입니다.

서비스 계정 자격증명은 단순히 공개되어도 되는 식별자가 아닙니다. 해당 계정에 부여된 IAM 권한에 따라 실제 백엔드 리소스에 인증할 수 있는 자격증명입니다.

앱을 받은 사람이 추출할 수 있는 파일 안에 이런 값이 들어가 있어서는 안 됩니다.

#### 제가 지키는 선

제 분석은 **오프라인에서만** 이루어집니다.

공개적으로 배포된 앱 패키지를 받아 제 컴퓨터에서 분석합니다.

라이브 서버를 공격하거나, 다른 사람의 네트워크 트래픽을 가로채거나, 발견한 키를 이용해 로그인하거나 데이터를 열어보지 않습니다.

키가 실제 형태인지 확인해야 할 때도 로컬에서 가능한 **구조 검증까지만** 합니다.

예를 들어 개인키라면 형식과 수학적 구조가 유효한지는 확인할 수 있습니다. 하지만 그 키를 실제 서비스에 사용해 인증을 시도하지는 않습니다.

문제가 심각해 보이면 공개하지 않고 제보합니다.

보통은 **개발자 직접 연락 → 응답이 없으면 KrCERT/KISA → 필요한 경우 해당 플랫폼 사업자** 순서로 알립니다.

Firebase나 Google Cloud 관련 문제라면 Google 측에 알리는 경우도 있습니다.

어느 회사인지, 어떤 앱인지, 발견한 키 값이 무엇인지는 공개하지 않습니다.

#### 왜 계속하냐고 묻는다면

처음부터 돈을 벌려고 시작한 일은 아니었습니다.

그냥 궁금했습니다.

**신기하잖아요.**

어떤 파일 안에 숨어 있던 실수를 발견하고, 누군가 악용하기 전에 알려줄 수 있다는 게 재미있었습니다.

누군가는 큰 비용이 발생할 뻔한 일을 피할 수도 있고, 누군가의 개인정보가 위험해지는 걸 막을 수도 있습니다.

어제까지 아무도 모르던 문제가 제가 우연히 발견해서 고쳐진다면, 그 자체로 꽤 멋진 일이라고 생각합니다.

사람은 실수합니다. 저도 실수했고, 제 친구들도 실수했습니다. 그래서 오히려 이런 실수를 자동으로 찾아주는 시스템에 관심을 갖게 됐습니다.

취미로 시작한 일이지만 가끔은 이런 생각도 합니다.

**어떻게 보면 세상의 아주 작은 부분 하나는 내가 구한 것 아닐까.**

저는 좋은 행동도 쌓인다고 생각합니다. 누군가를 도우면 그 사람이 또 다른 사람을 돕고, 그런 것이 계속 이어집니다. 저는 그것을 일종의 덕이나 카르마처럼 생각합니다.

그래서 이 일을 하고 있는 제 자신이 꽤 마음에 듭니다.

무엇보다, **도와주는 데서 기쁨을 느낍니다.**

</details>

# Hi, I'm Joe Lee (이주호).

My interest in security started with a mistake of my own.

While coding, I once pushed a key to a public place by accident. Some friends of mine had made similar mistakes.

That's when this thought hit me:

**"If I'm doing this, aren't other people making the same mistake?"**

So I started looking for myself.

I combined several open-source tools into a system that automatically analyzes publicly distributed apps. It downloads an app package, takes it apart on my own computer, and looks for credentials or config files that got included by accident.

The first time I ran it, real keys came out.

But far too many of them.

At first I thought I'd built something wrong. It was sometimes flagging dummy keys or test values as the real thing. I went through them one by one, fixed how the structural checks worked, and in the process I realized there really were cases where live credentials had been shipped as-is.

That's when it became something more than simple curiosity.

## What I've found so far

So far I've analyzed **3,134 apps**.

* 1,560 mobile apps
* 1,574 web apps

Across them I found **2,714 exposures.**

The main types:

* Cloud DB / storage reachable without any login: about **1,280**
* Source code / server config exposed: about **310**
* Slack / Discord webhooks: about **250**
* Kakao keys: **248**
* OpenAI keys: **158**
* GCP service-account credentials: **139**
* Private keys (PEM): **78**
* AWS credentials: **77**

There were also Google OAuth, Telegram, GitHub PAT, and Naver-related credentials, among others.

These numbers do **not** mean "X% of all apps are vulnerable."

What I analyzed isn't a random sample meant to represent the whole app market. The numbers above are simply a tally of the exposures I happened to find while looking.

Among them were apps from large companies, and apps used by children.

What surprised me most: **139 apps had GCP service-account credentials sitting inside the distributed package.**

A service-account credential isn't just an identifier that's fine to make public. Depending on the IAM permissions granted to that account, it's a credential that can authenticate to real backend resources.

A value like that should never sit inside a file that anyone who downloads the app can pull out.

## The line I hold

My analysis happens **offline only.**

I take publicly distributed app packages and analyze them on my own computer.

I don't attack live servers, I don't intercept anyone's network traffic, and I don't use the keys I find to log in or open up data.

Even when I need to check whether a key is genuine, I only go as far as **structural verification** that's possible locally.

For a private key, for instance, I can check whether its format and mathematical structure are valid. But I don't take that key and try to authenticate with it against a real service.

When something looks serious, I don't publish it. I report it.

Usually in this order:

**contact the developer directly → if there's no reply, KrCERT/KISA → the platform provider when needed.**

For Firebase or Google Cloud issues, I sometimes notify Google.

I don't reveal which company it was, which app, or what the key was.

## If you ask why I keep doing it

I didn't start this to make money.

I was just curious.

**It's fascinating, isn't it?**

Finding a mistake hidden inside some file, and being able to tell someone before it gets abused — that was fun.

Someone might avoid a huge bill they were about to face. Someone's personal data might be kept out of harm's way.

If a problem nobody knew about until yesterday gets fixed because I happened to find it, I think that's a pretty great thing on its own.

People make mistakes. I did, and so did my friends. That's actually what got me interested in a system that finds these mistakes automatically.

It started as a hobby, but sometimes I think this:

**maybe, in a way, I saved one very small part of the world.**

I believe good actions add up too. You help someone, they help someone else, and it keeps going. I think of it as a kind of virtue, or karma.

So I genuinely like the person I am while I'm doing this.

And more than anything,

**I find joy in helping.**

---

**이주호 · Joe Lee**

GitHub: [github.com/joewr21](https://github.com/joewr21)
Email: [thinkaboutjoe@gmail.com](mailto:thinkaboutjoe@gmail.com)
