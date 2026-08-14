# AX 평가 시스템 컬럼 정의서

> 현재 요구사항을 기준으로 작성한 설계안이며, 팀 검토 후 확정한다.  
> FK의 참조 대상은 `허용값`이 아니라 `설명`에 작성한다.  
> `PK/FK` 열에는 PK와 FK만 표시하며, UNIQUE 조건은 `허용값` 또는 `설명`에 작성한다.

| 테이블 | 컬럼 | 한글명 | 데이터 타입 | 길이 | NULL | PK/FK | 기본값 | 허용값 | 설명 | 관련 BR |
|---|---|---|---|---|---|---|---|---|---|---|
| USER | user_id | 사용자 ID | BIGINT | - | N | PK | 자동 증가 | 양수 | 사용자를 고유하게 식별하는 번호 | BR-01~BR-05, BR-10 |
| USER | email | 이메일 | VARCHAR | 254 | N | - | - | 이메일 형식, 중복 불가 | 로그인에 사용하는 이메일 | BR-01~BR-05 |
| USER | password | 비밀번호 | VARCHAR | 128 | N | - | - | Django 암호화 형식 | Django 인증에서 암호화하여 저장하는 비밀번호 | - |
| USER | name | 사용자명 | VARCHAR | 50 | N | - | - | 공백 불가 | 학생 또는 튜터의 이름 | - |
| USER | role | 사용자 역할 | VARCHAR | 20 | N | - | STUDENT | STUDENT, TUTOR | 학생과 튜터의 권한을 구분 | BR-01~BR-05, BR-10 |
| USER | is_active | 활성 여부 | BOOLEAN | - | N | - | TRUE | TRUE, FALSE | 사용자의 로그인 및 서비스 이용 가능 여부 | BR-01~BR-05 |
| USER | created_at | 생성 일시 | TIMESTAMP | - | N | - | 현재 일시 | - | 사용자 계정 생성 일시 | - |
| USER | updated_at | 수정 일시 | TIMESTAMP | - | N | - | 현재 일시 | - | 사용자 정보 최종 수정 일시 | - |
| STUDENT_PROFILE | student_id | 학생 ID | BIGINT | - | N | PK | 자동 증가 | 양수 | 수강생 프로필을 고유하게 식별하는 번호 | BR-01~BR-09 |
| STUDENT_PROFILE | user_id | 사용자 ID | BIGINT | - | N | FK | - | 중복 불가 | USER.user_id를 참조하며 사용자당 학생 프로필 하나만 허용 | BR-01~BR-05 |
| STUDENT_PROFILE | student_number | 수강생 번호 | VARCHAR | 30 | Y | - | - | 값이 있으면 중복 불가 | 교육 과정에서 사용하는 수강생 식별 번호 | - |
| STUDENT_PROFILE | is_enrolled | 과정 참여 여부 | BOOLEAN | - | N | - | TRUE | TRUE, FALSE | 현재 평가 참여 대상 수강생인지 표시 | BR-01~BR-07 |
| STUDENT_PROFILE | created_at | 생성 일시 | TIMESTAMP | - | N | - | 현재 일시 | - | 수강생 프로필 생성 일시 | - |
| STUDENT_PROFILE | updated_at | 수정 일시 | TIMESTAMP | - | N | - | 현재 일시 | - | 수강생 프로필 최종 수정 일시 | - |
| ASSIGNMENT_ROUND | round_id | 평가 회차 ID | BIGINT | - | N | PK | 자동 증가 | 양수 | 평가 회차를 고유하게 식별하는 번호 | BR-01~BR-10 |
| ASSIGNMENT_ROUND | created_by_user_id | 생성자 ID | BIGINT | - | N | FK | - | TUTOR 역할 사용자 | USER.user_id를 참조하며 평가 회차를 생성한 튜터 | BR-10 |
| ASSIGNMENT_ROUND | round_name | 평가 회차명 | VARCHAR | 100 | N | - | - | 공백 불가 | 예: 2주차 AX 평가 | - |
| ASSIGNMENT_ROUND | assignment_name | 과제명 | VARCHAR | 200 | N | - | - | 공백 불가 | 해당 평가 회차에서 수행하는 과제명 | - |
| ASSIGNMENT_ROUND | start_at | 평가 시작 일시 | TIMESTAMP | - | N | - | - | end_at보다 이전 | 평가를 제출할 수 있는 시작 일시 | BR-05 |
| ASSIGNMENT_ROUND | end_at | 평가 종료 일시 | TIMESTAMP | - | N | - | - | start_at보다 이후 | 평가 제출 종료 일시 | BR-05 |
| ASSIGNMENT_ROUND | status | 평가 상태 | VARCHAR | 20 | N | - | DRAFT | DRAFT, READY, IN_PROGRESS, CLOSED, CALCULATED, PUBLISHED | 평가 회차의 진행 상태 | BR-05, BR-10 |
| ASSIGNMENT_ROUND | team_count | 팀 수 | INTEGER | - | N | - | 1 | 1 이상 | 평가 회차에서 구성할 팀 수 | BR-09 |
| ASSIGNMENT_ROUND | publish_team_winner | 팀 1위 공개 여부 | BOOLEAN | - | N | - | FALSE | TRUE, FALSE | 학생에게 팀 1위를 공개할지 설정 | BR-10 |
| ASSIGNMENT_ROUND | publish_team_rank | 전체 팀 순위 공개 여부 | BOOLEAN | - | N | - | FALSE | TRUE, FALSE | 학생에게 전체 팀 순위를 공개할지 설정 | BR-10 |
| ASSIGNMENT_ROUND | publish_personal_score | 개인점수 공개 여부 | BOOLEAN | - | N | - | FALSE | TRUE, FALSE | 학생에게 개인점수를 공개할지 설정 | BR-10 |
| ASSIGNMENT_ROUND | publish_personal_rank | 개인 순위 공개 여부 | BOOLEAN | - | N | - | FALSE | TRUE, FALSE | 학생에게 개인 전체 순위를 공개할지 설정 | BR-10 |
| ASSIGNMENT_ROUND | created_at | 생성 일시 | TIMESTAMP | - | N | - | 현재 일시 | - | 평가 회차 생성 일시 | - |
| ASSIGNMENT_ROUND | updated_at | 수정 일시 | TIMESTAMP | - | N | - | 현재 일시 | - | 평가 회차 최종 수정 일시 | - |
| TEAM | team_id | 팀 ID | BIGINT | - | N | PK | 자동 증가 | 양수 | 팀을 고유하게 식별하는 번호 | BR-01~BR-03, BR-06, BR-09 |
| TEAM | round_id | 평가 회차 ID | BIGINT | - | N | FK | - | - | ASSIGNMENT_ROUND.round_id를 참조 | BR-01~BR-06, BR-09 |
| TEAM | team_number | 팀 번호 | INTEGER | - | N | - | - | 1 이상, 회차 내 중복 불가 | 평가 회차 내 팀 번호 | BR-01, BR-06 |
| TEAM | team_name | 팀명 | VARCHAR | 100 | N | - | - | 회차 내 중복 불가 | 해당 회차에서 사용하는 팀 이름 | BR-01, BR-06 |
| TEAM | created_at | 생성 일시 | TIMESTAMP | - | N | - | 현재 일시 | - | 팀 생성 일시 | - |
| TEAM_MEMBER | member_id | 팀원 소속 ID | BIGINT | - | N | PK | 자동 증가 | 양수 | 회차별 팀 소속 기록을 식별 | BR-01~BR-04, BR-09 |
| TEAM_MEMBER | round_id | 평가 회차 ID | BIGINT | - | N | FK | - | - | ASSIGNMENT_ROUND.round_id를 참조 | BR-01~BR-05, BR-09 |
| TEAM_MEMBER | team_id | 팀 ID | BIGINT | - | N | FK | - | 같은 회차의 팀 | TEAM.team_id를 참조 | BR-01~BR-04, BR-09 |
| TEAM_MEMBER | student_id | 학생 ID | BIGINT | - | N | FK | - | 회차당 한 팀만 허용 | STUDENT_PROFILE.student_id를 참조 | BR-01~BR-04, BR-09 |
| TEAM_MEMBER | assigned_at | 팀 배정 일시 | TIMESTAMP | - | N | - | 현재 일시 | - | 학생이 해당 팀에 배정된 일시 | BR-09 |
| EVALUATION_TEMPLATE | template_id | 평가 템플릿 ID | BIGINT | - | N | PK | 자동 증가 | 양수 | 평가 양식을 고유하게 식별 | BR-06, BR-07 |
| EVALUATION_TEMPLATE | round_id | 평가 회차 ID | BIGINT | - | N | FK | - | - | ASSIGNMENT_ROUND.round_id를 참조 | BR-06, BR-07 |
| EVALUATION_TEMPLATE | template_name | 평가 템플릿명 | VARCHAR | 100 | N | - | - | 공백 불가 | 평가 양식의 이름 | BR-06, BR-07 |
| EVALUATION_TEMPLATE | evaluation_type | 평가 유형 | VARCHAR | 20 | N | - | - | TEAM, PEER | 팀 평가와 개인 평가를 구분 | BR-01~BR-07 |
| EVALUATION_TEMPLATE | is_active | 활성 여부 | BOOLEAN | - | N | - | TRUE | TRUE, FALSE | 해당 템플릿의 사용 가능 여부 | BR-06, BR-07 |
| EVALUATION_TEMPLATE | created_at | 생성 일시 | TIMESTAMP | - | N | - | 현재 일시 | - | 평가 템플릿 생성 일시 | - |
| EVALUATION_QUESTION | question_id | 평가 문항 ID | BIGINT | - | N | PK | 자동 증가 | 양수 | 평가 문항을 고유하게 식별 | BR-06, BR-07 |
| EVALUATION_QUESTION | template_id | 평가 템플릿 ID | BIGINT | - | N | FK | - | - | EVALUATION_TEMPLATE.template_id를 참조 | BR-06, BR-07 |
| EVALUATION_QUESTION | question_text | 평가 문항 내용 | VARCHAR | 500 | N | - | - | 공백 불가 | 평가자가 답변할 평가 문항 | BR-06, BR-07 |
| EVALUATION_QUESTION | display_order | 문항 순서 | INTEGER | - | N | - | 1 | 1 이상, 템플릿 내 중복 불가 | 평가 화면에 표시할 문항 순서 | BR-06, BR-07 |
| EVALUATION_QUESTION | min_score | 최소 점수 | SMALLINT | - | N | - | 1 | 1 | 평가 문항의 최소 허용 점수 | BR-06, BR-07 |
| EVALUATION_QUESTION | max_score | 최대 점수 | SMALLINT | - | N | - | 5 | 5 | 평가 문항의 최대 허용 점수 | BR-06, BR-07 |
| EVALUATION_QUESTION | is_required | 필수 응답 여부 | BOOLEAN | - | N | - | TRUE | TRUE, FALSE | 필수 평가 문항인지 표시 | BR-06, BR-07 |
| TEAM_EVALUATION | team_evaluation_id | 팀 평가 ID | BIGINT | - | N | PK | 자동 증가 | 양수 | 팀 평가 제출 기록을 식별 | BR-01, BR-05, BR-06 |
| TEAM_EVALUATION | round_id | 평가 회차 ID | BIGINT | - | N | FK | - | 진행 중인 회차 | ASSIGNMENT_ROUND.round_id를 참조 | BR-01, BR-05, BR-06 |
| TEAM_EVALUATION | evaluator_student_id | 평가자 학생 ID | BIGINT | - | N | FK | - | 학생 권한 사용자 | STUDENT_PROFILE.student_id를 참조하며 실제 평가자는 수강생 | BR-01, BR-05 |
| TEAM_EVALUATION | target_team_id | 평가 대상 팀 ID | BIGINT | - | N | FK | - | 같은 회차의 다른 팀 | TEAM.team_id를 참조 | BR-01, BR-05, BR-06 |
| TEAM_EVALUATION | template_id | 평가 템플릿 ID | BIGINT | - | N | FK | - | TEAM 유형 템플릿 | EVALUATION_TEMPLATE.template_id를 참조 | BR-06 |
| TEAM_EVALUATION | status | 제출 상태 | VARCHAR | 20 | N | - | DRAFT | DRAFT, SUBMITTED | 임시저장과 최종 제출을 구분 | BR-05 |
| TEAM_EVALUATION | submitted_at | 제출 일시 | TIMESTAMP | - | Y | - | - | 제출 완료 시 필수 | 팀 평가 최종 제출 일시 | BR-05 |
| TEAM_EVALUATION | created_at | 생성 일시 | TIMESTAMP | - | N | - | 현재 일시 | - | 팀 평가 기록 생성 일시 | - |
| TEAM_EVALUATION | updated_at | 수정 일시 | TIMESTAMP | - | N | - | 현재 일시 | - | 팀 평가 최종 수정 일시 | - |
| TEAM_EVALUATION_ANSWER | answer_id | 팀 평가 답변 ID | BIGINT | - | N | PK | 자동 증가 | 양수 | 팀 평가 문항별 답변 식별 | BR-06 |
| TEAM_EVALUATION_ANSWER | team_evaluation_id | 팀 평가 ID | BIGINT | - | N | FK | - | - | TEAM_EVALUATION.team_evaluation_id를 참조 | BR-05, BR-06 |
| TEAM_EVALUATION_ANSWER | question_id | 평가 문항 ID | BIGINT | - | N | FK | - | TEAM 유형 문항 | EVALUATION_QUESTION.question_id를 참조 | BR-06 |
| TEAM_EVALUATION_ANSWER | score | 문항 점수 | SMALLINT | - | N | - | - | 1, 2, 3, 4, 5 | 팀 평가 문항에 입력한 점수 | BR-06 |
| TEAM_EVALUATION_ANSWER | created_at | 생성 일시 | TIMESTAMP | - | N | - | 현재 일시 | - | 문항 답변 저장 일시 | - |
| PEER_EVALUATION | peer_evaluation_id | 개인 평가 ID | BIGINT | - | N | PK | 자동 증가 | 양수 | 개인 상호평가 제출 기록 식별 | BR-02~BR-05, BR-07 |
| PEER_EVALUATION | round_id | 평가 회차 ID | BIGINT | - | N | FK | - | 진행 중인 회차 | ASSIGNMENT_ROUND.round_id를 참조 | BR-02~BR-05, BR-07 |
| PEER_EVALUATION | evaluator_student_id | 평가자 학생 ID | BIGINT | - | N | FK | - | 학생 권한 사용자 | STUDENT_PROFILE.student_id를 참조 | BR-02~BR-05 |
| PEER_EVALUATION | target_student_id | 평가 대상 학생 ID | BIGINT | - | N | FK | - | 본인 제외 같은 팀원 | STUDENT_PROFILE.student_id를 참조 | BR-02~BR-05, BR-07 |
| PEER_EVALUATION | template_id | 평가 템플릿 ID | BIGINT | - | N | FK | - | PEER 유형 템플릿 | EVALUATION_TEMPLATE.template_id를 참조 | BR-07 |
| PEER_EVALUATION | status | 제출 상태 | VARCHAR | 20 | N | - | DRAFT | DRAFT, SUBMITTED | 임시저장과 최종 제출을 구분 | BR-05 |
| PEER_EVALUATION | submitted_at | 제출 일시 | TIMESTAMP | - | Y | - | - | 제출 완료 시 필수 | 개인 평가 최종 제출 일시 | BR-05 |
| PEER_EVALUATION | created_at | 생성 일시 | TIMESTAMP | - | N | - | 현재 일시 | - | 개인 평가 기록 생성 일시 | - |
| PEER_EVALUATION | updated_at | 수정 일시 | TIMESTAMP | - | N | - | 현재 일시 | - | 개인 평가 최종 수정 일시 | - |
| PEER_EVALUATION_ANSWER | answer_id | 개인 평가 답변 ID | BIGINT | - | N | PK | 자동 증가 | 양수 | 개인 평가 문항별 답변 식별 | BR-07 |
| PEER_EVALUATION_ANSWER | peer_evaluation_id | 개인 평가 ID | BIGINT | - | N | FK | - | - | PEER_EVALUATION.peer_evaluation_id를 참조 | BR-05, BR-07 |
| PEER_EVALUATION_ANSWER | question_id | 평가 문항 ID | BIGINT | - | N | FK | - | PEER 유형 문항 | EVALUATION_QUESTION.question_id를 참조 | BR-07 |
| PEER_EVALUATION_ANSWER | score | 문항 점수 | SMALLINT | - | N | - | - | 1, 2, 3, 4, 5 | 개인 평가 문항에 입력한 점수 | BR-07 |
| PEER_EVALUATION_ANSWER | created_at | 생성 일시 | TIMESTAMP | - | N | - | 현재 일시 | - | 문항 답변 저장 일시 | - |
| TEAM_GRADE | team_grade_id | 팀 성적 ID | BIGINT | - | N | PK | 자동 증가 | 양수 | 계산된 팀 성적 결과 식별 | BR-06 |
| TEAM_GRADE | round_id | 평가 회차 ID | BIGINT | - | N | FK | - | 팀당 회차별 한 건 | ASSIGNMENT_ROUND.round_id를 참조 | BR-06 |
| TEAM_GRADE | team_id | 팀 ID | BIGINT | - | N | FK | - | 팀당 회차별 한 건 | TEAM.team_id를 참조 | BR-06 |
| TEAM_GRADE | team_score | 팀 점수 | DECIMAL | 4,2 | Y | - | - | 1.00 이상 5.00 이하 | 다른 팀의 유효한 평가를 집계한 평균점수 | BR-06, BR-08 |
| TEAM_GRADE | team_rank | 팀 순위 | INTEGER | - | Y | - | - | 1 이상 | 해당 평가 회차의 팀 순위 | BR-06, BR-10 |
| TEAM_GRADE | calculated_at | 계산 일시 | TIMESTAMP | - | Y | - | - | - | 팀 성적을 계산한 일시 | BR-06 |
| PERSONAL_GRADE | personal_grade_id | 개인 성적 ID | BIGINT | - | N | PK | 자동 증가 | 양수 | 계산된 개인 성적 결과 식별 | BR-07~BR-09 |
| PERSONAL_GRADE | round_id | 평가 회차 ID | BIGINT | - | N | FK | - | 학생당 회차별 한 건 | ASSIGNMENT_ROUND.round_id를 참조 | BR-07~BR-09 |
| PERSONAL_GRADE | student_id | 학생 ID | BIGINT | - | N | FK | - | 학생당 회차별 한 건 | STUDENT_PROFILE.student_id를 참조 | BR-07~BR-09 |
| PERSONAL_GRADE | team_score | 적용 팀 점수 | DECIMAL | 4,2 | Y | - | - | 1.00 이상 5.00 이하 | 개인 최종점수에 40% 반영할 팀점수 | BR-08 |
| PERSONAL_GRADE | peer_score | 개인 평가점수 | DECIMAL | 4,2 | Y | - | - | 1.00 이상 5.00 이하 | 같은 팀원에게 받은 개인 평가 평균점수 | BR-07, BR-08 |
| PERSONAL_GRADE | team_weight | 팀점수 가중치 | DECIMAL | 4,3 | N | - | 0.400 | 0 이상 1 이하 | 팀점수 반영 비율 | BR-08 |
| PERSONAL_GRADE | peer_weight | 개인점수 가중치 | DECIMAL | 4,3 | N | - | 0.600 | 0 이상 1 이하 | 개인점수 반영 비율 | BR-08 |
| PERSONAL_GRADE | final_score | 개인 최종점수 | DECIMAL | 4,2 | Y | - | - | 1.00 이상 5.00 이하 | 팀점수 40%와 개인점수 60%를 반영한 결과 | BR-08, BR-09 |
| PERSONAL_GRADE | rank | 개인 석차 | INTEGER | - | Y | - | - | 1 이상 | 평가 회차 전체 학생의 개인 순위 | BR-10 |
| PERSONAL_GRADE | seed_score | 다음 회차 시드 | DECIMAL | 6,2 | Y | - | - | 0 이상 | 다음 팀 자동 편성에 사용할 누적 시드 | BR-09 |
| PERSONAL_GRADE | calculated_at | 계산 일시 | TIMESTAMP | - | Y | - | - | - | 개인 성적을 계산한 일시 | BR-07, BR-08 |
| PERSONAL_GRADE | is_confirmed | 성적 확정 여부 | BOOLEAN | - | N | - | FALSE | TRUE, FALSE | 개인 성적의 최종 확정 여부 | BR-08, BR-09 |

## 추가 업무 제약조건

- TEAM_MEMBER의 `team_id`는 같은 `round_id`에 속한 팀이어야 한다.
- 팀 평가자는 본인이 속한 팀을 평가할 수 없다.
- 개인 평가자는 자기 자신을 평가할 수 없다.
- 개인 평가자와 평가 대상 학생은 같은 회차의 같은 팀에 속해야 한다.
- TEAM_EVALUATION의 템플릿은 TEAM 유형이어야 한다.
- PEER_EVALUATION의 템플릿은 PEER 유형이어야 한다.
- 제출 완료 상태에서는 `submitted_at`이 반드시 존재해야 한다.
- `team_weight + peer_weight`는 1.000이어야 한다.
- 개인 최종점수는 `team_score × team_weight + peer_score × peer_weight`로 계산한다.