# n8n 실습 환경

3장 실습에서 사용하는 PostgreSQL과 n8n을 Docker Compose로 실행합니다.

## 구성

- `db`: PostgreSQL 17
  - `n8n`: n8n 내부 데이터베이스
  - `harumarket`: 업무 데이터베이스
  - `n8n_exam`: 예제 데이터베이스
- `n8n`: n8n 2.43.7 (`http://localhost:5678`)

Compose 프로젝트 이름은 `n8n-personal`이며, 데이터는 다음 명명 볼륨에 저장됩니다.

- `personal_db_data` (실제 Docker 볼륨: `n8n-personal_personal_db_data`): PostgreSQL 데이터
- `personal_n8n_data` (실제 Docker 볼륨: `n8n-personal_personal_n8n_data`): n8n 설정 파일

## 시작 전 설정

Docker와 Docker Compose가 설치되어 있어야 합니다. 다음 명령은 저장소 루트에서 실행합니다.

```powershell
Set-Location "실습/03/env"
Copy-Item "환경변수_예시.env" ".env"
```

생성된 `.env` 파일을 열어 `< >`로 표시된 자리표시자를 실제 값으로 바꿉니다. 특히 `N8N_ENCRYPTION_KEY`는 저장된 자격 증명을 복호화하는 데 필요하므로 한 번 정한 뒤 변경하거나 잃어버리지 않도록 보관합니다.

## 실행

`compose.yml`이 있는 `실습/03/env` 디렉터리에서 실행합니다.

```console
docker compose up -d
```

컨테이너 상태를 확인합니다.

```console
docker compose ps
```

서비스가 준비되면 브라우저에서 `http://localhost:5678`에 접속합니다.

## 종료

컨테이너와 네트워크만 내립니다. 명명 볼륨의 데이터는 유지됩니다.

```console
docker compose down
```

## 데이터까지 초기화

컨테이너와 네트워크뿐 아니라 이 Compose 프로젝트의 명명 볼륨도 삭제합니다.

```console
docker compose down -v
```

이 명령을 실행하면 PostgreSQL 데이터베이스와 n8n 설정이 삭제되어 복구할 수 없습니다. Docker 이미지와 다른 Compose 프로젝트의 리소스는 삭제하지 않습니다.
