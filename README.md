## Next.js App Router Course - Starter

This is the starter template for the Next.js App Router Course. It contains the starting code for the dashboard application.

For more information, see the [course curriculum](https://nextjs.org/learn) on the Next.js Website.

## Chapter 1. Getting Started

새 프로젝트 만들기

```
npx create-next-app@latest nextjs-dashboard --use-npm --example "https://github.com/vercel/next-learn/tree/main/dashboard/starter-example"
```

폴더 구조
![learn-folder-structure](https://github.com/heechul94/learn-next.js/assets/100992153/f2cc99fd-3a90-4f0d-b67c-e68e073988e6)

- /app: 애플리케이션에 대한 모든 경로, 구성 요소 및 논리가 포함되어 있으며 여기서 주로 작업하게 됩니다.
- /app/lib: 재사용 가능한 유틸리티 함수, 데이터 가져오기 함수 등 애플리케이션에서 사용되는 함수가 포함되어 있습니다.
- /app/ui: 카드, 테이블, 양식 등 애플리케이션의 모든 UI 구성 요소가 포함되어 있습니다. 시간을 절약하기 위해 이러한 구성 요소의 스타일이 미리 지정되어 있습니다.
- /public: 이미지와 같은 애플리케이션의 모든 정적 에셋을 포함합니다.
- /scripts: 이후 장에서 데이터베이스를 채우는 데 사용할 시드 스크립트가 포함되어 있습니다.
- Config Files: 또한 애플리케이션 루트에 next.config.js와 같은 구성 파일이 있는 것을 볼 수 있습니다. 이러한 파일의 대부분은 create-next-app을 사용하여 새 프로젝트를 시작할 때 생성되고 사전 구성됩니다. 이 과정에서는 수정할 필요가 없습니다.

## Chapter 2. CSS Styling

### 전역 스타일

최상위 구성 요소에 추가한다.

```
/app/ui/global.css
```

```css
@tailwind base;
@tailwind components;
@tailwind utilities;

input[type='number'] {
  -moz-appearance: textfield;
  appearance: textfield;
}

input[type='number']::-webkit-inner-spin-button {
  -webkit-appearance: none;
  margin: 0;
}

input[type='number']::-webkit-outer-spin-button {
  -webkit-appearance: none;
  margin: 0;
}
```

<hr>

```
/app/layout.tsx
```

```javascript
import '@/app/ui/global.css';

export default function RootLayout({
  children,
}: {
  children: React.ReactNode;
}) {
  return (
    <html lang="en">
      <body>{children}</body>
    </html>
  );
}
```

## Chapter 3. Optimizing Fonts and Images

### 왜 글꼴을 최적화해야 할까요?

글꼴은 웹사이트 디자인에서 중요한 역할을 합니다. 그러나 프로젝트에서 `사용자 정의 글꼴을 사용하면 글꼴 파일을 가져오고 로드해야 하므로` 성능에 영향을 미칠 수 있습니다.

글꼴을 사용하면, 브라우저가 `처음에 대체 또는 시스템 글꼴로 텍스트를 렌더링하고` 로드된 후에 사용자 정의 글꼴로 교체될 때 레이아웃 시프트가 발생합니다. 이 교체로 인해 텍스트 크기, 간격 또는 레이아웃이 변경되어 `주변 요소가 이동할 수 있습니다.`

Next.js는 `next/font` 모듈을 사용할 때 애플리케이션에서 글꼴을 자동으로 최적화합니다. 이 모듈을 사용하면 `글꼴 파일을 빌드 시 다운로드`하고 다른 정적 애셋과 함께 호스팅합니다. 이렇게 하면 사용자가 애플리케이션을 방문할 때 글꼴에 대한 `추가 네트워크 요청이 없으므로` 성능에 영향을 미치지 않습니다.

### 왜 이미지 최적화가 필요한가요?

일반적인 HTML에서는 이미지를 다음과 같이 추가합니다.

```html
<img
  src="/hero.png"
  alt="데스크톱 및 모바일 버전의 대시보드 프로젝트 스크린샷"
/>
```

이런 방법은 다음 사항을 수동으로 수행해야 합니다.

- 이미지가 다양한 화면 크기에 반응적으로 표시되도록 보장합니다.
- 다양한 기기에 대한 이미지 크기를 지정합니다.
- 이미지가 로드됨에 따라 레이아웃 시프트를 방지합니다.
- 뷰포트 외부에 있는 이미지를 지연로드합니다.

`next/image` 컴포넌트를 사용하여 이미지를 자동으로 최적화할 수 있습니다.
<Image> 컴포넌트
`<Image>`컴포넌트는 HTML `<img>`태그의 확장이며 다음과 같은 자동 이미지 최적화 기능을 제공합니다.

- 이미지가 로드될 때 자동으로 레이아웃 시프트 방지.
- 뷰포트에 들어올 때 이미지 크기를 조절하여 작은 뷰포트 기기로 큰 이미지를 전송하지 않도록 함.
- 기본적으로 이미지를 지연로드 (이미지가 뷰포트에 진입할 때 로드).
- 브라우저가 지원하는 경우 WebP 및 AVIF와 같은 현대적인 포맷으로 이미지 제공.

## Chapter 4. Creating Layouts and Pages

### 중첩 라우팅

- Next.js는 <strong>폴더</strong>를 사용하여 중첩된 경로를 만드는 파일 시스템 라우팅을 사용합니다. 각 폴더는 <strong>URL 세그먼트</strong>에 매핑되는 <strong>경로 세그먼트</strong>를 나타냅니다.
- 중첩된 경로를 만들려면 각 폴더를 서로 중첩시키고 내부에 page.tsx 파일을 추가하면 됩니다.
  > ![folders-to-url-segments](https://github.com/heechul94/learn-next.js/assets/100992153/dd52e550-c76a-42c5-8d8d-c2b85b472c7e) > ![dashboard-route](https://github.com/heechul94/learn-next.js/assets/100992153/30579d24-5f60-4742-b412-466348ecf1ec)

### 레이아웃

- Next.js에서 여러 페이지 간에 공유되는 UI를 만들려면 layout.tsx라는 특별한 파일을 사용할 수 있습니다.
- <Layout /> 컴포넌트는 children 속성을 받습니다. 이 자식 요소는 페이지거나 다른 레이아웃일 수 있습니다. 여러분의 경우 /dashboard 내부의 페이지는 자동으로 <Layout /> 내에 중첩될 것입니다

```javascript
import SideNav from '@/app/ui/dashboard/sidenav';
export default function Layout({ children }: { children: React.ReactNode }) {
  return (
    <div className="flex h-screen flex-col md:flex-row md:overflow-hidden">
      <div className="w-full flex-none md:w-64">
        <SideNav />
      </div>
      <div className="flex-grow p-6 md:overflow-y-auto md:p-12">{children}</div>
    </div>
  );
}
```

> ![shared-layout](https://github.com/heechul94/learn-next.js/assets/100992153/fdddeb82-5c0f-4fd9-9ccd-101754c5d852)

- Next.js에서 레이아웃의 장점은 페이지 컴포넌트만 업데이트되고 레이아웃은 다시 렌더링되지 않는다는 것입니다. 이를 `부분 렌더링`이라고 합니다
  > ![partial-rendering-dashboard](https://github.com/heechul94/learn-next.js/assets/100992153/034863cd-4d7a-4957-be55-f3d8b7626b2f)

### 루트 레이아웃

```javascript
import '@/app/ui/global.css';
import { inter } from '@/app/ui/fonts';

export default function RootLayout({
  children,
}: {
  children: React.ReactNode;
}) {
  return (
    <html lang="en">
      <body className={`${inter.className} antialiased`}>{children}</body>
    </html>
  );
}
```

- 필수로 존재해야 합니다.
- 루트 레이아웃에 추가한 모든 UI는 애플리케이션의 모든 페이지에서 공유됩니다.
- 루트 레이아웃에는 <html> 및 <body> 태그를 수정하고 메타데이터를 추가할 수 있습니다

## Chapter 5. Navigating Between Pages

### 페이지 이동 최적화의 필요성

페이지 간에 링크를 만들려면 전통적으로는 <a> HTML 요소를 사용해야 합니다. 하지만 이 방법은 페이지 이동시 전체 페이지가 새로 고침됩니다.

## <Link> 컴포넌트

- Next.js에서는 애플리케이션 내의 페이지 간에 링크를 만들기 위해 <Link /> 컴포넌트를 사용할 수 있습니다. <Link>를 사용하면 JavaScript를 사용하여 클라이언트-사이드 페이지 이동이 가능합니다.
- Link 컴포넌트는 <a> 태그를 사용하는 것과 유사하지만 <a href="…"> 대신에 <Link href="…">를 사용합니다.
- 라우터가 초기 로드 시 모든 애플리케이션 코드를 로드하는 전통적인 React 싱글 페이지 애플리케이션과 다릅니다.
- 페이지 이동 경험을 향상시키기 위해 Next.js는 라우트 세그먼트별로 애플리케이션 코드를 자동으로 분할합니다.
- 라우트별로 코드를 분할하면 페이지가 격리됩니다. 특정 페이지에서 오류가 발생하더라도 나머지 애플리케이션은 여전히 작동합니다.
- production 환경에서 브라우저 뷰포트에 <Link> 컴포넌트가 나타나면 Next.js가 자동으로 연결된 경로의 코드를 백그라운드에서 **사전로드(prefetches)**합니다. 사용자가 링크를 클릭할 때 대상 페이지의 코드는 이미 백그라운드에서 로드되어 페이지 전환이 거의 즉시 발생합니다!
