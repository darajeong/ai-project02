# 1. 테이블 상세 정의서

## 1-1. 테이블명
STUDENT

## 1-2. 한글명
학생

## 1-3. 목적
회원가입한 수강생 정보를 저장하고, 팀 소속 및 개인 평가 대상 학생을 식별한다.

## 1-4. 관련 BR
- BR-01: 학생은 본인이 속한 팀을 확인할 수 있다.
- BR-04: 학생은 같은 팀에 속한 팀원을 확인할 수 있다.
- BR-05: 학생별 개인 평가를 중복 없이 관리한다.

## 1-5. 주요 프로그램
- 학생 회원가입 및 로그인
- 내 팀 조회
- 팀원 조회
- 개인 평가 조회

## 1-6. 주요 컬럼

| 컬럼명 | 한글명 | 역할 |
|---|---|---|
| student_id | 학생 ID | 학생을 고유하게 식별하는 PK |
| name | 학생 이름 | 학생 이름 |
| email | 이메일 | 로그인 및 학생 식별용 이메일 |
| password | 비밀번호 | 로그인 인증 정보 |

## 1-7. PK
- `student_id`

## 1-8. FK
- 없음

## 1-9. 다른 테이블과 관계
- STUDENT (1) : TEAM_MEMBER (N)
- STUDENT (1) : PERSONAL_GRADE (N)

## 1-10. 제약조건
- NOT NULL: `name`, `email`, `password`
- UNIQUE: `email`
- CHECK: 없음

## 1-11. 이 테이블이 없으면 발생하는 문제
- 학생 계정과 학생 정보를 관리할 수 없다.
- 팀 소속 및 개인 평가의 대상 학생을 식별할 수 없다.

# 2. 테이블 상세 정의서

## 2-1. 테이블명
TUTOR

## 2-2. 한글명
튜터

## 2-3. 목적
회원가입한 튜터 정보를 저장하고, 평가 회차 생성자 및 평가자를 식별한다.

## 2-4. 관련 BR
- 튜터는 평가 회차를 생성한다.
- 팀 평가와 개인 평가에는 평가자가 필요하다.
- BR-05: 평가자별 중복 평가를 방지한다.

## 2-5. 주요 프로그램
- 튜터 회원가입 및 로그인
- 평가 회차 생성
- 팀 평가
- 개인 평가

## 2-6. 주요 컬럼

| 컬럼명 | 한글명 | 역할 |
|---|---|---|
| tutor_id | 튜터 ID | 튜터를 고유하게 식별하는 PK |
| name | 튜터 이름 | 튜터 이름 |
| email | 이메일 | 로그인 및 튜터 식별용 이메일 |
| password | 비밀번호 | 로그인 인증 정보 |

## 2-7. PK
- `tutor_id`

## 2-8. FK
- 없음

## 2-9. 다른 테이블과 관계
- TUTOR (1) : ASSIGNMENT_ROUND (N)
- TUTOR (1) : PERSONAL_GRADE (N)
- TUTOR (1) : TEAM_GRADE (N)

## 2-10. 제약조건
- NOT NULL: `name`, `email`, `password`
- UNIQUE: `email`
- CHECK: 없음

## 2-11. 이 테이블이 없으면 발생하는 문제
- 평가 회차의 생성자와 평가자를 식별할 수 없다.
- 튜터 계정 정보를 관리할 수 없다.

# 3. 테이블 상세 정의서

## 3-1. 테이블명
ASSIGNMENT_ROUND

## 3-2. 한글명
과제/평가 회차

## 3-3. 목적
튜터가 생성한 과제 또는 평가의 회차, 진행 기간 및 상태를 관리한다.

## 3-4. 관련 BR
- 튜터는 여러 평가 회차를 생성할 수 있다.
- 모든 팀 구성과 평가 데이터는 특정 회차에 연결된다.
- BR-05: 평가 중복 여부는 회차별로 판단한다.

## 3-5. 주요 프로그램
- 평가 회차 생성 및 수정
- 평가 기간 관리
- 회차별 팀 구성
- 회차별 평가 관리

## 3-6. 주요 컬럼

| 컬럼명 | 한글명 | 역할 |
|---|---|---|
| round_id | 회차 ID | 평가 회차를 고유하게 식별하는 PK |
| tutor_id | 생성 튜터 ID | 회차를 생성한 튜터 |
| round_name | 회차명 | 과제 또는 평가 회차의 이름 |
| start_date | 시작일시 | 평가 시작 일시 |
| end_date | 종료일시 | 평가 종료 일시 |
| status | 상태 | 회차 진행 상태 |

## 3-7. PK
- `round_id`

## 3-8. FK
- `tutor_id` → `TUTOR.tutor_id`

## 3-9. 다른 테이블과 관계
- TUTOR (1) : ASSIGNMENT_ROUND (N)
- ASSIGNMENT_ROUND (1) : TEAM (N)
- ASSIGNMENT_ROUND (1) : TEAM_MEMBER (N)
- ASSIGNMENT_ROUND (1) : PERSONAL_GRADE (N)
- ASSIGNMENT_ROUND (1) : TEAM_GRADE (N)

## 3-10. 제약조건
- NOT NULL: `tutor_id`, `round_name`, `start_date`, `end_date`, `status`
- UNIQUE: 없음
- CHECK: `start_date <= end_date`

## 3-11. 이 테이블이 없으면 발생하는 문제
- 팀, 개인 평가, 팀 평가를 어느 평가 기간의 데이터인지 구분할 수 없다.
- 평가 데이터의 회차별 중복 방지와 기간 관리가 불가능하다.

# 4. 테이블 상세 정의서

## 4-1. 테이블명
TEAM

## 4-2. 한글명
팀

## 4-3. 목적
특정 평가 회차에서 구성된 팀 정보를 저장한다.

## 4-4. 관련 BR
- 한 평가 회차에는 여러 팀이 존재할 수 있다.
- 학생은 회차별로 하나의 팀에만 소속된다.
- 팀 평가는 평가 대상 팀을 기준으로 관리한다.

## 4-5. 주요 프로그램
- 회차별 팀 생성 및 관리
- 팀원 배정
- 팀 평가

## 4-6. 주요 컬럼

| 컬럼명 | 한글명 | 역할 |
|---|---|---|
| team_id | 팀 ID | 팀을 고유하게 식별하는 PK |
| round_id | 회차 ID | 팀이 속한 평가 회차 |
| team_name | 팀명 | 회차 내 팀 이름 |

## 4-7. PK
- `team_id`

## 4-8. FK
- `round_id` → `ASSIGNMENT_ROUND.round_id`

## 4-9. 다른 테이블과 관계
- ASSIGNMENT_ROUND (1) : TEAM (N)
- TEAM (1) : TEAM_MEMBER (N)
- TEAM (1) : TEAM_GRADE (N)

## 4-10. 제약조건
- NOT NULL: `round_id`, `team_name`
- UNIQUE: `UNIQUE(round_id, team_name)`
- CHECK: 없음

## 4-11. 이 테이블이 없으면 발생하는 문제
- 특정 회차의 팀 구성 정보를 저장할 수 없다.
- 팀원 소속과 팀 평가의 대상 팀을 관리할 수 없다.

# 5. 테이블 상세 정의서

## 5-1. 테이블명
TEAM_MEMBER

## 5-2. 한글명
팀원 소속

## 5-3. 목적
학생과 팀의 소속 관계를 회차별로 관리한다.

## 5-4. 관련 BR
- BR-01: 학생은 본인이 속한 팀을 확인할 수 있다.
- BR-04: 학생은 본인과 같은 팀에 속한 팀원을 확인할 수 있다.
- 한 학생은 같은 회차에서 하나의 팀에만 소속될 수 있다.

## 5-5. 주요 프로그램
- 팀원 배정
- 내 팀 조회
- 팀원 목록 조회

## 5-6. 주요 컬럼

| 컬럼명 | 한글명 | 역할 |
|---|---|---|
| member_id | 팀원 소속 ID | 팀원 소속 정보를 고유하게 식별하는 PK |
| round_id | 회차 ID | 팀 소속이 발생한 평가 회차 |
| team_id | 팀 ID | 학생이 소속된 팀 |
| student_id | 학생 ID | 팀에 소속된 학생 |

## 5-7. PK
- `member_id`

## 5-8. FK
- `round_id` → `ASSIGNMENT_ROUND.round_id`
- `team_id` → `TEAM.team_id`
- `student_id` → `STUDENT.student_id`
- `(team_id, round_id)` → `TEAM(team_id, round_id)`

## 5-9. 다른 테이블과 관계
- ASSIGNMENT_ROUND (1) : TEAM_MEMBER (N)
- TEAM (1) : TEAM_MEMBER (N)
- STUDENT (1) : TEAM_MEMBER (N)

## 5-10. 제약조건
- NOT NULL: `round_id`, `team_id`, `student_id`
- UNIQUE: `UNIQUE(round_id, student_id)`
- CHECK: 없음

## 5-11. 이 테이블이 없으면 발생하는 문제
- 학생의 회차별 팀 소속을 알 수 없다.
- 본인 팀 및 같은 팀원을 조회할 수 없다.
- 한 학생이 같은 회차에 여러 팀에 중복 소속되는 것을 방지할 수 없다.

# 6. 테이블 상세 정의서

## 6-1. 테이블명
PERSONAL_GRADE

## 6-2. 한글명
개인 평가

## 6-3. 목적
특정 회차에서 평가자가 학생에게 부여한 개인 평가 결과를 저장한다.

## 6-4. 관련 BR
- 개인 평가는 평가자와 대상 학생이 필요하다.
- BR-05: 동일 평가자가 동일 회차의 동일 학생을 중복 평가할 수 없다.

## 6-5. 주요 프로그램
- 개인 평가 등록 및 수정
- 학생별 개인 점수 조회
- 회차별 개인 순위 조회

## 6-6. 주요 컬럼

| 컬럼명 | 한글명 | 역할 |
|---|---|---|
| personal_grade_id | 개인 평가 ID | 개인 평가 결과를 고유하게 식별하는 PK |
| round_id | 회차 ID | 평가가 진행된 회차 |
| evaluator_tutor_id | 평가자 튜터 ID | 개인 평가를 수행한 튜터 |
| student_id | 대상 학생 ID | 개인 평가를 받는 학생 |
| final_score | 최종 점수 | 개인 평가 점수 |
| rank | 순위 | 회차 내 개인 평가 순위 |

## 6-7. PK
- `personal_grade_id`

## 6-8. FK
- `round_id` → `ASSIGNMENT_ROUND.round_id`
- `evaluator_tutor_id` → `TUTOR.tutor_id`
- `student_id` → `STUDENT.student_id`

## 6-9. 다른 테이블과 관계
- ASSIGNMENT_ROUND (1) : PERSONAL_GRADE (N)
- TUTOR (1) : PERSONAL_GRADE (N)
- STUDENT (1) : PERSONAL_GRADE (N)

## 6-10. 제약조건
- NOT NULL: `round_id`, `evaluator_tutor_id`, `student_id`, `final_score`
- UNIQUE: `UNIQUE(round_id, evaluator_tutor_id, student_id)`
- CHECK: `final_score >= 0`, `rank > 0`

## 6-11. 이 테이블이 없으면 발생하는 문제
- 개인 평가의 평가자, 대상 학생, 점수를 관리할 수 없다.
- 동일 평가자가 같은 학생을 중복 평가하는 것을 방지할 수 없다.

# 7. 테이블 상세 정의서

## 7-1. 테이블명
TEAM_GRADE

## 7-2. 한글명
팀 평가

## 7-3. 목적
특정 회차에서 평가자가 팀에 부여한 팀 평가 결과를 저장한다.

## 7-4. 관련 BR
- 팀 평가는 평가자와 대상 팀이 필요하다.
- BR-05: 동일 평가자가 동일 회차의 동일 팀을 중복 평가할 수 없다.

## 7-5. 주요 프로그램
- 팀 평가 등록 및 수정
- 팀별 평가 점수 조회
- 회차별 팀 순위 조회

## 7-6. 주요 컬럼

| 컬럼명 | 한글명 | 역할 |
|---|---|---|
| team_grade_id | 팀 평가 ID | 팀 평가 결과를 고유하게 식별하는 PK |
| round_id | 회차 ID | 평가가 진행된 회차 |
| evaluator_tutor_id | 평가자 튜터 ID | 팀 평가를 수행한 튜터 |
| team_id | 대상 팀 ID | 팀 평가를 받는 팀 |
| team_score | 팀 점수 | 팀 평가 점수 |
| team_rank | 팀 순위 | 회차 내 팀 평가 순위 |

## 7-7. PK
- `team_grade_id`

## 7-8. FK
- `round_id` → `ASSIGNMENT_ROUND.round_id`
- `evaluator_tutor_id` → `TUTOR.tutor_id`
- `team_id` → `TEAM.team_id`
- `(team_id, round_id)` → `TEAM(team_id, round_id)`

## 7-9. 다른 테이블과 관계
- ASSIGNMENT_ROUND (1) : TEAM_GRADE (N)
- TUTOR (1) : TEAM_GRADE (N)
- TEAM (1) : TEAM_GRADE (N)

## 7-10. 제약조건
- NOT NULL: `round_id`, `evaluator_tutor_id`, `team_id`, `team_score`
- UNIQUE: `UNIQUE(round_id, evaluator_tutor_id, team_id)`
- CHECK: `team_score >= 0`, `team_rank > 0`

## 7-11. 이 테이블이 없으면 발생하는 문제
- 팀 평가의 평가자, 대상 팀, 점수를 관리할 수 없다.
- 해당 회차에 존재하지 않는 팀을 평가하는 문제를 막을 수 없다.
- 동일 평가자가 같은 팀을 중복 평가하는 것을 방지할 수 없다.