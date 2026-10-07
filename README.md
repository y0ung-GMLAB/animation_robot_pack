# animation_robot_pack

로봇별 애니메이션 · 로봇 팩 보관소 · 받아서 웹 화면에 드래그로 올리는 용도
(로봇 PC 가 git pull 하는 저장소 아님 · 플랫폼 코드는 `robot_web`)

```
<공연>/                      → 여러 로봇이 같이 쓰는 공연 하나 = 폴더 하나 (예: floating_narration_all)
  <공연>.json               → 웹 · 애니메이션 재생 화면에 드래그 (모든 로봇 공통 · 로봇마다 자기 motion_id 만 씀)
  robot_pack_<N>/           → N번 로봇 PC 의 웹 · 시스템 정보 → 로봇 팩에 폴더째 드래그 (PC 전역 · 교체)
<로봇>/animations/*.json     → 한 로봇 전용 테스트 애니메이션 (예: floating_1)
```

- 애니메이션 · Blender export 결과 그대로 (`motion_header` JSON Lines · 50 fps · rad)
- 로봇 팩 · 폴더로 저장 · zip 불필요 (화면이 묶어서 올림)
  - 형식 · `robot_web/scripts/sim/README.md` 「로봇 팩 형식」
  - `pack.yaml` 의 `version` · 내용 바꿀 때마다 올림 (웹에 이름·버전 표시)
  - 서버 검사 실패 시 교체 안 됨 · 화면에 이유 목록
  - 팩의 `motion_id` = 애니메이션 파일 id 완전 일치 (다르면 sim 에서 그 축 0° 유지)
  - 팩은 로봇마다 따로 (`robot.yaml` 의 `motion_id` 가 그 로봇 것) · 한 팩을 여러 PC 에 올리면 미리보기가 다른 로봇 움직임을 보여 줌
  - 올리기 전 검사 · robot_web 에서 `uv run --no-project --with mujoco --with numpy --with pyyaml python scripts/sim/sim_run.py --check <팩 폴더>`

## floating_narration_all

- 플로팅 4대 나레이션 공연 (556 s) · 애니메이션 하나에 조인트 20개 (`f{N}_neck_yaw` …)
- `floating_narration_all.json` · 4대 공통 (각 PC 는 자기 `f{N}_*` 만 씀) · 패널 토크쇼식 대화 (KO 질문자 · DE/FR/ES 답변) · 속도 상한 = 사용자 속도 테스트 (고개 좌우 9 °/s · 14 °/s² · 상하 6 · 10 · 눈 10 · 22)
- `robot_pack_1` ~ `4` · 4대 모두 1800 모델 기준 (사이즈 다른 로봇이면 그 팩만 교체) · 내용 같음 · `robot.yaml` 의 `motion_id` 접두어 (`f1_` … `f4_`) · `pack.yaml` name 만 다름
- `scene.glb` (웹 「Blender 뷰」) · 애니메이션이 구워져 들어가므로 애니메이션 확정 후 각 팩에 추가
- 모델 · `floating/floating_1800/sim/fh_1800_wires_R011.xml` (천장 원판 3점 수직 와이어)
- drive · PC 모터 설정 = 플랫폼 기본값 18000 deg/s · 180000 deg/s² (모터축)

## floating_1 ~ 4

- 로봇 전용 테스트 애니메이션 · 팩은 `floating_narration_all/robot_pack_<N>` 사용
- `floating_1800_dir_test` (방향) · `floating_1800_limit_test` (리미트) · 4대 모두 같은 움직임 · id 만 `f{N}_*`
- `floating_no1_motion1` · `floating_no2_motion1` · 옛 `N-k` id 그대로 · 조인트 이름을 `f{N}_*` 로 바꾼 PC 에서는 움직이지 않음
