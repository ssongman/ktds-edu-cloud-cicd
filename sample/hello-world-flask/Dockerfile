FROM python:3.9.6-slim-buster
WORKDIR /app

# Pip 패키지 설치
COPY requirements.txt requirements.txt
RUN pip3 install -r requirements.txt

# 필요한 파일을 복사

COPY . .
ENTRYPOINT ["python3", "app.py"]

#podman build -t tt --cgroup-manager=cgroupfs .