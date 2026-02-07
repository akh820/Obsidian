 
- 메모리 확인 : free -h

- 도커 재시작 정책 : 인스턴스가 재부팅되거나, 프로세스가 예기치 않게 종료되어도 도커가 알아서 다시 살리게 만드는 명령어.
```bash
docker update --restart unless-stopped 76e1f34464fc
```
- 컨테이너 재시작 
```bash
docker start 76e1f34464fc
```