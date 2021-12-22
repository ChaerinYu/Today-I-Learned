
## React
* 유저 인터페이스를 만들기 위한 JavaScript 라이브러리
* 고려해야할 주요 사항
  * 웹팩과 바벨을 구성하고 사용해야 한다.
  * 성능과 SEO(검색엔진최적화)를 위해 정적으로 사전 렌더링을 원할 때가 있다.
  * 코드 스플릿팅([code-splitting](#Code-Splitting))과 같이 프로덕션 단계에서 최적화를 해야 할 필요가 있다.
  * 데이터 저장하기 위해 서버단 코드와 React 앱을 연결해야 한다.

## CSR과 SSR
* SPA(Single Page Application)이 프론트엔드 개발의 대세가 되면서 CSR(Client Side Rendering)을 활용한 서비스들이 등장했다.  
검색엔진들이 CSR로 개발된 페이지를 제대로 인식하지 못하기 때문에 `next.js`와 같은 대체적인 방법을 활용하게 되었다.  
최근 검색엔진의 성능이 조금씩 좋아져 CSR로 구현된 페이지도 조금씩 분석이 가능해지고 있지만 최상위 페이지 노출되기에는 부족하다.  

## Next.js
* React의 SSR(Server-Side-Rendering) 프레임워크로 React 애플리케이션 개발을 좀 더 수월하게 해줄 수 있다.
* 제공하는 기능
  * 다이나믹 라우트를 지원하는 직관적인 페이지 기반의 라우팅 시스템
  * 각 페이지마다 기본적으로 사전렌더링([Pre-rendering](#Pre-rendering)), SSG(Static Generation)과 SSR(Server-Side Rendering)을 지원
  * 빠른 페이지 로딩을 위한 자동 코드 스플릿팅
  * 최적화된 프리페칭([prefetching](#Prefetching))을 이용한 클라이언트 사이드 라우팅
  * Built-in CSS와 Sass 지원, 그리고 다른 CSS-in-JS 라이브러리 지원
  * 개발 환경에서의 빠른 리프레시 지원
  * 서버리스 환경에서 API를 구축하기 위한 API라우트
  * 넓은 확장성

## TypeScript
* JavaScript를 기반으로 정적 타입 문법을 추가한 프로그래밍 언어
* 동적 타입의 인터프리터 언어인 JavaScript와 달리 TypeScript는 **정적 타입의 컴파일 언어**이며 **컴파일러를 통해 JavaScrpit코드로 변환**된다. 
* 코드 작성 단계에서 타입을 체크해서 오류를 확인할 수 있어 **실행 전 오류를 잡아낼 수 있다**.


<br/><br/>


### Code-Splitting
> code splitting을 하면 사용자가 현재 필요로하는 것들만 lazy-load할 수 있으므로 앱의 성능을 크게 향상시킬 수 있습니다.  
> 코드 양이 줄지는 않지만 사용자가 필요로하지 않은 코드를 로드하는 것을 피하고, 초기 페이지 로드시 필요한 코드만 받게 됩니다.  

### Prefetching
> Next.js는 자동으로 백그라운드에서 링크된 페이지의 코드를 프리페치하기 때문에 `Link`컴포넌트를 사용할 때마다 브라우저의 `viewport`에 나타나게 된다.  
> 링크를 클릭하면 링크된 페이지에 대한 코드가 이미 백그라운드에 로드되어 페이지 전환이 거의 바로 이루어지게 되는 것이다.

### Pre-rendering
> 기본적으로, **Next.js는 모든 페이지를 pre-render**한다. 이 말은 즉슨, client-side 자바스크립트와는 다르게 Next.js는 각 페이지에 대해 미리 HTML을 만들어 놓는다.   
> 이러한 Pre-rendering로 인해 좋은 성능과 SEO를 얻을 수 있다. 
> 생성된 HTML에는 최소한으로 필요한 자바스크립트 코드가 있다.  
> 페이지가 브라우저에 의해 로드될 때, 그 자바스크립트 코드가 실행되고 페이지를 동적이게 만들어준다(이 과정을 `hydration`이라고 부른다.)  
> Next.js는 pre-rendering을 `Static Generation`과 `Server-side Rendering`, 이 두 가지 형식으로 제공한다. 이 둘의 **차이점은 언제 HTML이 생성되는지** 이다.  
> Next.js는 **각 페이지마다 어떤 pre-rendering 방식을 사용할지 선택할 수 있게 해준다**. 따라서 특정 페이지에는 Static Genration 방식을, 나머지 페이지에는 Server-side Rendering 방식을 사용할 수 있는 "hybrid"앱을 만들 수 있다.


#### Next.js에서의 Static Generation
* Static Generation은 빌드 타임에 HTML을 생성하는 pre-rendering 방법이다. pre-rendered된 HTML은 각 요청마다 재사용이 가능하다.
* 사용자의 요청보다 먼저 페이지가 렌더링되어도 되는 경우  
* 데이터를 포함해도 되고, 포함하지 않아도 수행이 된다.
* ex) 마케팅 페이지, 블로그 포스트, E-commerce 상품 리스트, 각종 문서 페이지

#### Next.js에서의 Server-side Rendering
* Server-side Rendering은 각 요청마다 HTML을 생성하는 pre-rendering 방법이다.
* 데이터의 업데이트가 많거나 사용자의 요청마다 매번 바뀌는 페이지

그러나 몇몇의 페이지들에서 외부에서 데이터를 fetching 해오지 않으면, 렌더링이 안되는 경우가 있다.  
예를 들어 *파일 시스템에 접근하거나, 외부 API를 사용하거나, 데이터베이스에 쿼리를 날릴 때*이다.  
Next.js는 이러한 경우 **데이터를 사용한 정적 생성도 지원**하고 있다.  
위의 과정을 가능하게 하는 것이 `getStaticProps`이다.


<br/><br/>

[참고](https://gingerkang.tistory.com/118?category=926753)
