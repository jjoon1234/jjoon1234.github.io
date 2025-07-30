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

#### 1. index.js파일은 아래처럼 작성함
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
