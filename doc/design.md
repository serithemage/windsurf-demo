# Todo 앱 디자인 문서

## 1. 개요
이 문서는 Todo 앱의 기본적인 설계와 기능을 설명합니다.

## 2. 기능 요구사항
### 핵심 기능
- Todo 항목 생성
- Todo 항목 조회
- Todo 항목 수정
- Todo 항목 삭제
- Todo 항목 완료 상태 토글

### Todo 항목 속성
- 제목 (필수)
- 설명 (선택)
- 생성 일시
- 완료 여부
- 우선순위 (높음, 중간, 낮음)

## 3. 개발 순서
### Phase 1: 프론트엔드 개발 (브라우저 단독 실행)
1. 프로젝트 초기 설정 (React + TypeScript)
2. UI 컴포넌트 구현
   - Todo 입력 폼
   - Todo 리스트 뷰
   - Todo 아이템 컴포넌트
3. 상태 관리 구현
   - localStorage를 사용한 데이터 영속성
   - React Query와 MSW를 활용한 API 구조 준비
4. Mock API 연동

### Phase 2: 백엔드 개발 (선택사항)
1. Express.js 서버 설정
2. 데이터베이스 스키마 구현
3. API 엔드포인트 구현
4. 프론트엔드와 연동

## 4. 기술 스택
### 프론트엔드
- React.js 18
- TypeScript 5
- TailwindCSS (UI 스타일링)
- React Query (상태 관리)
- MSW (Mock Service Worker, API Mocking)
- localStorage (데이터 영속성)

### 백엔드 (선택사항)
- Node.js
- Express.js
- SQLite (데이터 저장)

## 5. Mock API 스펙
프론트엔드는 localStorage를 사용하여 데이터를 저장하고, MSW를 통해 실제 API와 동일한 인터페이스를 제공합니다:

### 데이터 저장소
```typescript
// localStorage 키
const STORAGE_KEY = 'todos';

// 데이터 구조
interface TodoStorage {
    todos: Todo[];
    lastId: number;
}
```

### 응답 데이터 형식
```typescript
interface Todo {
    id: number;
    title: string;
    description?: string;
    createdAt: string;
    completed: boolean;
    priority: 'high' | 'medium' | 'low';
}
```

### Mock API 응답
- GET /api/todos
  ```json
  {
    "todos": Todo[],
    "total": number
  }
  ```
- GET /api/todos/:id
  ```json
  {
    "todo": Todo
  }
  ```
- POST /api/todos
  ```json
  {
    "todo": Todo
  }
  ```
- PUT /api/todos/:id
  ```json
  {
    "todo": Todo
  }
  ```
- DELETE /api/todos/:id
  ```json
  {
    "success": boolean
  }
  ```

## 6. 데이터베이스 스키마
```sql
CREATE TABLE todos (
    id INTEGER PRIMARY KEY AUTOINCREMENT,
    title TEXT NOT NULL,
    description TEXT,
    created_at DATETIME DEFAULT CURRENT_TIMESTAMP,
    completed BOOLEAN DEFAULT FALSE,
    priority TEXT CHECK(priority IN ('high', 'medium', 'low')) DEFAULT 'medium'
);
```

## 7. API 엔드포인트
- GET /api/todos - 모든 Todo 항목 조회
- GET /api/todos/:id - 특정 Todo 항목 조회
- POST /api/todos - 새로운 Todo 항목 생성
- PUT /api/todos/:id - 특정 Todo 항목 수정
- DELETE /api/todos/:id - 특정 Todo 항목 삭제

## 8. UI/UX 설계
- 심플하고 직관적인 인터페이스
- 반응형 디자인으로 모바일 지원
- 드래그 앤 드롭으로 Todo 항목 순서 변경 가능
- 우선순위에 따른 시각적 표시 (색상 구분)
- 완료된 항목은 취소선과 함께 표시
