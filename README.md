Movie Recommender CRUD

React, FastAPI, MySQL을 연동한 영화 추천 및 데이터 관리 시스템입니다.
기존 회원 목록·영화 검색·추천 기능을 유지하면서 회원, 영화, 평점의 CRUD와 평균 평점 통계를 추가했습니다.

주요 기능

회원 관리

회원 등록, 전체 조회, 이름 검색

회원 정보 수정 및 삭제

회원 삭제 시 해당 회원의 평점도 함께 삭제

영화 관리

영화 등록, 전체 조회, 제목 검색

영화 정보 수정 및 삭제

영화별 평균 평점과 평가 수 조회

영화 삭제 시 해당 영화의 평점도 함께 삭제

평점 관리

회원별 영화 평점 등록

전체·회원별·영화별 평점 조회

평점 수정 및 삭제

평점 입력 범위: 0점~5점

같은 회원과 영화 조합의 중복 평점 등록 방지

영화 추천

회원이 높게 평가한 영화의 제목, 장르, 줄거리를 분석해 유사 영화 추천

TF-IDF와 코사인 유사도를 이용한 콘텐츠 기반 추천

평가 기록이 없는 회원에게는 인기 영화 추천

이미 평가한 영화는 추천 결과에서 제외

기술 스택

구분

기술

Frontend

React 18, Vite 5, React Router, Axios

Backend

Python, FastAPI, SQLAlchemy, Uvicorn

Database

MySQL, PyMySQL

Recommendation

scikit-learn, TF-IDF, Cosine Similarity

프로젝트 구조

movie-recommender-crud/
├── backend_fastapi_movie_recsys/
│   ├── app/
│   │   ├── routers/
│   │   │   ├── movies.py
│   │   │   ├── ratings.py
│   │   │   ├── recommend.py
│   │   │   └── users.py
│   │   ├── db.py
│   │   ├── main.py
│   │   ├── models.py
│   │   ├── recommender.py
│   │   └── schemas.py
│   ├── sql/
│   │   ├── schema.sql
│   │   └── seed.sql
│   ├── .env.example
│   ├── pyproject.toml
│   └── requirements.txt
├── frontend_react_movie_recsys/
│   ├── src/
│   │   ├── components/
│   │   ├── pages/
│   │   ├── api.js
│   │   ├── App.jsx
│   │   ├── main.jsx
│   │   └── styles.css
│   ├── index.html
│   ├── package.json
│   └── vite.config.js
└── README.md

실행 방법

1. MySQL 데이터베이스 준비

MySQL Workbench에서 아래 파일을 순서대로 실행합니다.

backend_fastapi_movie_recsys/sql/schema.sql

backend_fastapi_movie_recsys/sql/seed.sql

seed.sql은 기존 데이터를 초기화하므로 실습용 데이터베이스에서 실행해야 합니다.

2. 백엔드 실행

PowerShell에서 다음 명령어를 실행합니다.

cd C:\bangminjung\04_react_fastapi_mysql\backend_fastapi_movie_recsys
python -m venv .venv
.\.venv\Scripts\python.exe -m pip install -r requirements.txt
Copy-Item .env.example .env

생성된 .env 파일의 MySQL 접속 정보를 현재 환경에 맞게 수정한 뒤 서버를 실행합니다.

.\.venv\Scripts\python.exe -m uvicorn app.main:app --reload --host 127.0.0.1 --port 8000

API 상태 확인: http://127.0.0.1:8000/

Swagger API 문서: http://127.0.0.1:8000/docs

3. 프론트엔드 실행

새 PowerShell 창을 열고 다음 명령어를 실행합니다.

cd C:\bangminjung\04_react_fastapi_mysql\frontend_react_movie_recsys
npm install
npm run dev

브라우저에서 http://localhost:5173에 접속합니다. 프론트엔드를 사용하려면 백엔드가 먼저 실행되어 있어야 합니다.

주요 API

기능

Method

URL

회원 전체 조회·검색

GET

/api/users?name=검색어

회원 등록

POST

/api/users

회원 상세 조회

GET

/api/users/{user_id}

회원 수정

PUT

/api/users/{user_id}

회원 삭제

DELETE

/api/users/{user_id}

영화 전체 조회·검색

GET

/api/movies?title=검색어

영화 등록

POST

/api/movies

영화 상세·평균 평점 조회

GET

/api/movies/{movie_id}

영화 수정

PUT

/api/movies/{movie_id}

영화 삭제

DELETE

/api/movies/{movie_id}

전체·조건별 평점 조회

GET

/api/ratings

회원별 평점 조회

GET

/api/users/{user_id}/ratings

평점 등록

POST

/api/users/{user_id}/ratings

평점 수정

PUT

/api/users/{user_id}/ratings/{movie_id}

평점 삭제

DELETE

/api/users/{user_id}/ratings/{movie_id}

회원별 영화 추천

GET

/api/recommend?user_id=1&limit=12

데이터베이스 관계

users: 회원 정보

movies: 영화 정보

ratings: 회원과 영화의 평점 정보

ratings는 (user_id, movie_id)를 복합 기본키로 사용합니다.

회원 또는 영화 삭제 시 연결된 평점은 ON DELETE CASCADE로 함께 삭제됩니다.

GitHub 업로드 제외 항목

다음 항목은 용량, 자동 생성 파일, 보안 문제 때문에 GitHub에 올리지 않습니다.

.venv/
node_modules/
__pycache__/
.env

환경변수 작성 예시는 .env.example로 제공합니다.

개선 내용

기존 조회 중심 화면을 회원·영화·평점 CRUD 화면으로 확장

프론트엔드와 백엔드의 등록·조회·수정·삭제 기능 연동

회원별·영화별 평점 조건 조회 추가

영화별 평균 평점과 평가 수 집계 추가

삭제 전 확인과 작업 결과 메시지를 제공하도록 사용자 화면 개선

ChatGPT 등 LLM의 도움을 받아 코드 구조와 화면 사용성을 개선하고 직접 실행·검증
