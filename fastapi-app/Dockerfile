# Python 3.9 베이스 이미지 사용
FROM python:3.9

# [보안 조치 1] 컨테이너 탈취를 대비해 root 권한이 없는 안전한 사용자(appuser) 생성
RUN useradd -m appuser

# 작업 디렉토리 설정
WORKDIR /app

# [빌드 최적화] requirements.txt를 먼저 복사하여 도커 캐시 활용도 높임
COPY requirements.txt .

# 의존성 설치
RUN pip install --no-cache-dir -r requirements.txt

# 필요한 소스 코드 복사 (보안 핫스팟 2번 관련)
COPY . .

# [보안 조치 1] 작업 디렉토리의 모든 파일 소유권을 appuser에게 부여
RUN chown -R appuser:appuser /app

# [보안 조치 1] 이제부터 root 대신 appuser 계정으로 전환하여 실행
USER appuser

# FastAPI 서버 실행
CMD ["uvicorn", "main:app", "--host", "0.0.0.0", "--port", "8000"]