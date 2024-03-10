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
