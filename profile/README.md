# 사내 개발 가이드

## 📋 목차
- [개요](#개요)
- [개발 환경 설정](#개발-환경-설정)
- [프로젝트 구조](#프로젝트-구조)
- [개발 워크플로우](#개발-워크플로우)
- [코딩 컨벤션](#코딩-컨벤션)
- [테스트 가이드라인](#테스트-가이드라인)
- [배포 프로세스](#배포-프로세스)
- [문제 해결](#문제-해결)
- [기여 방법](#기여-방법)

## 개요

이 문서는 사내 개발팀을 위한 공용 개발 가이드입니다. 모든 개발자들이 일관된 방식으로 개발할 수 있도록 돕고, 효율적인 협업을 지원하기 위해 작성되었습니다.

### 주요 기술 스택
- **언어**: Kotlin, JavaScript
- **프레임워크**: Spring Boot, React.js
- **데이터베이스**: PostgreSQL, MySQL
- **도구**: IntelliJ IDEA, Cursor AI

## 개발 환경 설정

### 사전 요구사항
- **백엔드 개발**:
  - JDK 17 이상
  - IntelliJ IDEA
  - Gradle
- **프론트엔드 개발**:
  - Node.js (버전 18 이상)
  - npm 또는 yarn
- **공통**:
  - Git
  - Docker (선택사항)

### 로컬 개발 환경 구성

#### 백엔드 (Spring Boot) 설정
```bash
# 저장소 클론
git clone [프로젝트-저장소-URL]
cd [프로젝트명]

# Gradle 빌드
./gradlew build

# 애플리케이션 실행
./gradlew bootRun
```

#### 프론트엔드 (React.js) 설정
```bash
# 프론트엔드 디렉토리로 이동
cd frontend

# 의존성 설치
npm install

# 개발 서버 실행
npm start
```

### IDE 설정
#### 백엔드 개발 (IntelliJ IDEA)
- **필수 플러그인**:
  - Kotlin
  - Spring Boot
  - Database Navigator
  - Git Toolbox

#### 프론트엔드 개발 (Cursor AI 또는 VSCode)
- **필수 확장**:
  - ES7+ React/Redux/React-Native snippets
  - Prettier - Code formatter
  - ESLint
  - Auto Rename Tag

## 프로젝트 구조

```
프로젝트명/
├── backend/                # 백엔드 (Spring Boot - DDD 구조)
│   ├── src/main/kotlin/
│   │   ├── domain/        # 도메인 계층
│   │   │   ├── model/     # 도메인 모델 (Entity, Value Object)
│   │   │   ├── service/   # 도메인 서비스
│   │   │   └── repository/ # 도메인 리포지토리 인터페이스
│   │   ├── application/   # 애플리케이션 계층
│   │   │   ├── service/   # 애플리케이션 서비스
│   │   │   ├── usecase/   # 유스케이스
│   │   │   └── dto/       # 데이터 전송 객체
│   │   ├── infrastructure/ # 인프라스트럭처 계층
│   │   │   ├── persistence/ # 데이터베이스 구현
│   │   │   ├── external/  # 외부 시스템 연동
│   │   │   └── config/    # 인프라 설정
│   │   ├── presentation/  # 프레젠테이션 계층
│   │   │   ├── controller/ # REST 컨트롤러
│   │   │   └── dto/       # API 요청/응답 DTO
│   │   └── shared/        # 공통 계층
│   │       ├── util/      # 유틸리티
│   │       └── exception/ # 예외 처리
│   ├── src/main/resources/ # 설정 파일
│   └── build.gradle.kts   # Gradle 설정
├── frontend/              # 프론트엔드 (React.js)
│   ├── src/
│   │   ├── components/    # 재사용 가능한 컴포넌트
│   │   ├── pages/         # 페이지 컴포넌트
│   │   ├── hooks/         # 커스텀 훅
│   │   ├── utils/         # 유틸리티 함수
│   │   └── styles/        # 스타일 파일
│   ├── public/            # 정적 자산
│   └── package.json       # 프로젝트 설정
├── docs/                  # 문서
└── README.md             # 프로젝트 개요
```

## 개발 워크플로우

### 브랜치 전략
- `main`: 프로덕션 브랜치
- `develop`: 개발 브랜치
- `feature/[기능명]`: 기능 개발 브랜치
- `bugfix/[버그명]`: 버그 수정 브랜치

### 개발 프로세스

1. **이슈 생성**: 작업할 내용에 대한 이슈를 생성합니다
2. **브랜치 생성**: 적절한 브랜치를 생성합니다
   ```bash
   git checkout -b feature/새로운-기능
   ```
3. **개발**: 코드를 작성하고 테스트합니다
4. **커밋**: [커밋 메시지 컨벤션](#커밋-메시지-컨벤션)을 따라 커밋합니다
5. **Pull Request**: 코드 리뷰를 위한 PR을 생성합니다

### 커밋 메시지 컨벤션

```
타입(범위): 간단한 설명

자세한 설명 (선택사항)

Fixes #이슈번호
```

**타입**:
- `feat`: 새로운 기능
- `fix`: 버그 수정
- `docs`: 문서 변경
- `style`: 코드 포맷팅
- `refactor`: 리팩토링
- `test`: 테스트 추가/수정
- `chore`: 기타 변경사항

## 코딩 컨벤션

### 일반 규칙
- 들여쓰기는 2칸 공백 사용
- 세미콜론 사용 필수
- 변수명은 camelCase 사용
- 상수는 UPPER_SNAKE_CASE 사용

### 파일 및 폴더 명명 규칙
- 컴포넌트 파일: PascalCase (예: `UserProfile.jsx`)
- 유틸리티 파일: camelCase (예: `dateUtils.js`)
- 폴더명: kebab-case (예: `user-management`)

### 코드 스타일
#### 백엔드 (Kotlin)
- ktlint 설정을 따라주세요
- 코드 포맷팅: `./gradlew ktlintFormat`
- 린트 체크: `./gradlew ktlintCheck`

#### 프론트엔드 (JavaScript/React)
- ESLint와 Prettier 설정을 따라주세요
- 코드 포맷팅: `npm run format`
- 린트 체크: `npm run lint`

## 테스트 가이드라인

### 테스트 종류
- **단위 테스트**: 개별 함수/컴포넌트 테스트
- **통합 테스트**: 여러 모듈 간의 상호작용 테스트
- **E2E 테스트**: 전체 애플리케이션 플로우 테스트

### 테스트 실행

#### 백엔드 테스트
```bash
# 모든 테스트 실행
./gradlew test

# 특정 테스트 클래스 실행
./gradlew test --tests "*UserServiceTest"

# 테스트 커버리지 확인
./gradlew jacocoTestReport
```

#### 프론트엔드 테스트
```bash
# 모든 테스트 실행
npm test

# 특정 테스트 파일 실행
npm test -- --testNamePattern="UserProfile"

# 테스트 커버리지 확인
npm run test:coverage
```

### 테스트 작성 규칙
- 테스트 파일은 `[파일명].test.js` 형식으로 명명
- 각 기능에 대해 최소 80% 이상의 코드 커버리지 유지
- 테스트는 독립적이고 반복 가능해야 함

## 배포 프로세스

### 개발 환경 배포
```bash
npm run deploy:dev
```

### 프로덕션 배포
1. `develop` 브랜치를 `main`으로 병합
2. 태그 생성: `git tag v1.0.0`
3. 자동 배포 트리거 또는 수동 배포 실행

### 환경별 설정
- **개발**: `config/dev.json`
- **스테이징**: `config/staging.json`
- **프로덕션**: `config/prod.json`

## 문제 해결

### 자주 발생하는 문제들

#### 의존성 설치 문제
```bash
# node_modules 삭제 후 재설치
rm -rf node_modules package-lock.json
npm install
```

#### 포트 충돌 문제
```bash
# 포트 사용 프로세스 확인
lsof -i :3000
# 프로세스 종료
kill -9 [PID]
```

### 로그 확인
- 개발 서버 로그: 터미널에서 확인
- 프로덕션 로그: `logs/` 디렉토리 확인

## 기여 방법

### 이슈 리포팅
1. 프로젝트의 GitHub Issues에서 새 이슈 생성
2. 버그 리포트 또는 기능 요청 템플릿 사용
3. 재현 가능한 예제 코드 포함

### 코드 기여
1. 저장소 포크
2. 새 브랜치 생성
3. 코드 작성 및 테스트
4. Pull Request 생성
5. 코드 리뷰 대응

### 문서 기여
- 오타 수정, 내용 개선, 새로운 가이드 추가 등
- Markdown 형식을 따라 작성

## 연락처 및 지원

- **개발팀 리더**: [팀장 이름] (이메일)
- **사내 개발자 채널**: [Slack/Teams 채널명]
- **기술 문의**: [기술지원 채널 또는 이메일]

---

마지막 업데이트: 2024년 1월

이 문서는 지속적으로 업데이트됩니다. 질문이나 제안사항이 있으시면 언제든 이슈를 생성해주세요!
