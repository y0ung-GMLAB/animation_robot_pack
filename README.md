# animation_robot_pack

로봇별 애니메이션 · 로봇 팩 보관소 · 받아서 웹 화면에 드래그로 올리는 용도
(로봇 PC 가 git pull 하는 저장소 아님 · 플랫폼 코드는 `robot_web`)

```
<로봇>/
  animations/*.json   → 웹 · 애니메이션 재생 화면에 드래그 (프로젝트별)
  robot_pack/         → 웹 · 시스템 정보 → 로봇 팩에 폴더째 드래그 (PC 전역 · 교체)
  robot_pack_<n>/     → PC(헤드)마다 motion_id 가 다른 로봇 · n = 헤드 번호 · 그 PC 에 n 번 팩
```

- 로봇 폴더 이름 · 제품·사이즈 단위 (예: `floating_1800`, `floating_1600`)
- 애니메이션 · Blender export 결과 그대로 (`motion_header` JSON Lines · 50 fps)
- 로봇 팩 · 폴더로 저장 · zip 불필요 (화면이 묶어서 올림)
  - 형식 · `robot_web/scripts/sim/README.md` 「로봇 팩 형식」
  - `pack.yaml` 의 `version` · 내용 바꿀 때마다 올림 (웹에 이름·버전 표시)
  - 서버 검사 실패 시 교체 안 됨 · 화면에 이유 목록
  - 팩의 `motion_id` = 애니메이션 파일 id 완전 일치 (다르면 sim 에서 그 축 0° 유지)
  - 올리기 전 검사 · robot_web 에서 `uv run --no-project --with mujoco --with numpy --with pyyaml python scripts/sim/sim_run.py --check <팩 폴더>`

## floating_1800

- 헤드 1~4 · `robot_pack_1` … `robot_pack_4` · 내용 같음 · `motion_id` (`n-1`..`n-5`) · `pack.yaml` name 만 다름
- 애니메이션 id 접두어 = 헤드 번호 (`floating_no2_*` → `2-*` → `robot_pack_2`)
- 모델 · `floating/floating_1800/sim/fh_1800_wires_R011.xml` (천장 원판 3점 수직 와이어)
- drive · PC 모터 설정 = 플랫폼 기본값 18000 deg/s · 180000 deg/s² (모터축)
