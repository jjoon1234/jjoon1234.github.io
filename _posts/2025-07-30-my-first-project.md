---
layout: post
title: "일정 관리 웹 프로젝트"
category: project
---

## 프로젝트 개요
2학년 2학기 React를 사용한 웹 개발 (개인)프로젝트

## 사용 기술
- HTML
- CSS
- React
- JS

## 주요 소스코드

#### 1. index.js
- 애플리케이션의 시작점 (Entry Point)
React 앱이 실행될 때 가장 먼저 호출되는 파일로 App.js 컴포넌트를 가져와 웹 페이지의 'root'라는 ID를 가진 DOM 요소에 렌더링(표시)하는 역할을 합니다. 즉, 우리 눈에 보이는 앱 화면 전체를 불러오는 출발점입니다.
<details>
<summary>index.js(클릭해서 열기)</summary>
  <script src="https://gist.github.com/jjoon1234/308b92b817a78f52f1e26e6fa76f3295.js"></script>
</details>

#### 2. App.js
- 애플리케이션의 메인 허브 (Main Hub)
이 앱의 가장 핵심적인 파일로, 거의 모든 기능의 로직과 상태 관리를 담당합니다.
1. 상태 관리: 현재 날짜, 전체 일정 목록, 사용자 로그인 여부, 사용자 정보 등 앱의 모든 데이터를 관리합니다.
2. 컴포넌트 렌더링: 로그인 상태에 따라 Login.js/Signup.js 컴포넌트 또는 메인 캘린더 화면을 조건부로 보여줍니다.
3. 이벤트 핸들링: 일정 추가, 삭제, 날짜 선택 등 사용자의 모든 행동(이벤트)에 대한 처리 함수를 포함합니다.
4. 데이터 영속성: localStorage를 사용하여 사용자의 일정과 계정 정보를 브라우저에 저장하고 불러옵니다. 이로써 사용자가 브라우저를 닫았다가 다시 열어도 데이터가 유지됩니다.
<details>
<summary>App.js(클릭해서 열기)</summary>
  <script src="https://gist.github.com/jjoon1234/9d79a8f6e98f3dce63d6f9ec7ea124c9.js"></script>
</details>

#### 3. Login.js
- 로그인 기능 컴포넌트
사용자가 아이디와 비밀번호를 입력하여 로그인할 수 있는 UI와 관련 로직을 담당합니다. 사용자가 입력한 정보는 App.js로 전달되어 실제 로그인 처리가 이루어집니다.
<details>
<summary>Login.js(클릭해서 열기)</summary>
  <script src="https://gist.github.com/jjoon1234/065d807148e0d83692eadc42f9f4d4f7.js"></script>
</details>

#### 4. Signup.js
- 회원가입 기능 컴포넌트
새로운 사용자가 계정을 생성할 수 있는 UI와 관련 로직을 담당합니다. Login.js와 유사하게, 입력된 정보는 App.js로 전달되어 사용자 목록에 추가됩니다.
<details>
<summary>Signup.js(클릭해서 열기)</summary>
  <script src="https://gist.github.com/jjoon1234/68912f478a663b102b341f2f8d5f96e8.js"></script>
</details>

#### 5. App.css
- 메인 스타일 시트
App.js와 그 하위 컴포넌트(캘린더, 버튼, 입력창, 일정 목록 등)의 세부적인 디자인을 정의합니다. 앱의 전반적인 레이아웃, 색상, 글꼴 크기, 애니메이션 효과 등이 포함되어 있습니다.
<details>
<summary>App.css(클릭해서 열기)</summary>
  <script src="https://gist.github.com/jjoon1234/6551d54f75d9f799e3cf7c996d0aa286.js"></script>
</details>

#### 6. index.css파일은 아래처럼 작성함
- 전역 스타일 시트
애플리케이션 전체에 적용되는 기본적인 스타일을 정의합니다. 주로 전체 페이지의 기본 글꼴, 여백 초기화 등 가장 바탕이 되는 스타일을 설정합니다.
<details>
<summary>index.js(클릭해서 열기)</summary>
  <script src="https://gist.github.com/jjoon1234/bf4802ea357ab24459ca0c089750e67a.js"></script>
</details>

## 동작 구조(Workflow)
<img alt="image" style="max-width: 100%; height: auto;" src="https://github.com/user-attachments/assets/ca4179bb-2a33-41a7-8a42-de77952a6c75" />


1. 시작 (index.js)
  사용자가 웹사이트에 접속하면, index.js가 App.js 컴포넌트를 웹 페이지에 띄워줍니다.

2. 메인 앱 로딩 (App.js)
  App.js가 실행되면서 가장 먼저 localStorage를 확인하여 현재 로그인된 사용자가 있는지 파악합니다.

3. 로그인 상태 확인 및 화면 분기
  로그인된 사용자가 없을 경우: App.js는 Login.js와 Signup.js 컴포넌트를 화면에 보여줍니다. 사용자는 로그인을 하거나 새로 가입할 수 있습니다.

  로그인된 사용자가 있을 경우: App.js는 해당 사용자의 일정 데이터를 localStorage에서 불러온 후, 캘린더와 일정 관리 UI를 화면에 보여줍니다.

4. 사용자 상호작용과 데이터 흐름

  로그인/회원가입: Login.js 또는 Signup.js에서 사용자가 정보를 입력하고 버튼을 클릭하면, 해당 정보가 App.js의 함수로 전달됩니다. App.js는 이 정보를 바탕으로 사용자 인증 및 상태 변경을 처리하고 localStorage에 데이터를 기록합니다.

  일정 관리: 로그인된 사용자가 날짜를 클릭하거나, 새로운 일정을 입력하고 추가 버튼을 누르면 모든 처리는 App.js에 있는 함수들이 담당합니다. App.js의 상태가 변경되면 React가 자동으로 화면을 다시 렌더링하여 변경사항(일정 추가/삭제 등)을 즉시 보여줍니다.

5. 스타일 적용 (App.css, index.css)

  이 모든 과정에서 보여지는 화면의 모든 시각적 요소들은 App.css와 index.css에 정의된 스타일에 따라 디자인됩니다. 이를 통해 사용자에게 일관되고 정돈된 UI를 제공합니다.

  결론적으로 App.js가 앱의 두뇌 역할을 하며 모든 데이터와 로직을 통제하고, Login.js와 Signup.js는 특정 기능(사용자 인증)을 담당하는 부품 역할을 합니다. index.js는 이 모든 것을 조립하여 실행하는 출발점이며, CSS 파일들은 앱에 옷을 입히는 역할을 한다고 볼 수 있습니다.



