# animation_robot_pack

로봇별 애니메이션 · 로봇 팩 보관소 · 받아서 웹 화면에 드래그로 올리는 용도
(로봇 PC 가 git pull 하는 저장소 아님 · 플랫폼 코드는 `robot_web`)

```
<로봇>/
  animations/*.json   → 웹 · 애니메이션 재생 화면에 드래그 (프로젝트별)
  robot_pack/         → 웹 · 시스템 정보 → 로봇 팩에 폴더째 드래그 (PC 전역 · 교체)
```

- 로봇 폴더 이름 · 제품·사이즈 단위 (예: `floating_1800`, `floating_1600`)
- 애니메이션 · Blender export 결과 그대로 (`motion_header` JSON Lines · 50 fps)
- 로봇 팩 · 폴더로 저장 · zip 불필요 (화면이 묶어서 올림)
  - 형식 · `robot_web/scripts/sim/README.md` 「로봇 팩 형식」
  - `pack.yaml` 의 `version` · 내용 바꿀 때마다 올림 (웹에 이름·버전 표시)
  - 서버 검사 실패 시 교체 안 됨 · 화면에 이유 목록
