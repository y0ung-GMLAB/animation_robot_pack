# animation_robot_pack

로봇별 애니메이션 · 로봇 팩 보관소 · 받아서 웹 화면에 드래그로 올리는 용도
(로봇 PC 가 git pull 하는 저장소 아님 · 플랫폼 코드는 `robot_web`)

```
<공연>/                      → 공연 애니메이션 폴더 (예: floating_narration_all) · 여러 로봇 공통
  <애니메이션>.json         → 웹 · 애니메이션 재생 화면에 드래그 (모든 로봇 공통 · 로봇마다 자기 motion_id 만 씀)
  <애니메이션>.wav          → 스피커 PC 웹(:8100) · 음원 업로드 · 「애니메이션별 음원」 에서 같은 이름 애니메이션에 연결
floating<N>_pack/            → N번 로봇 PC 의 웹 · 시스템 정보 → 로봇 팩에 폴더째 드래그 (PC 전역 · 교체)
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

- 플로팅 4대 나레이션 공연 · 애니메이션마다 조인트 20개 (`f{N}_neck_yaw` …) · 4대 공통 (각 PC 는 자기 `f{N}_*` 만 씀)
- 로봇 번호 · `Cam_Store` 에서 볼 때 왼쪽부터 f1 KO · f2 ES · f3 FR · f4 DE (2026-10-08)
- `floating_narration_all_9min.json` · 전체 556 s (27801 프레임) · `floating_narration_all_9min.wav` 와 짝
- `floating_narration_all_90s.json` · 앞 93 s 시안 (4650 프레임 · 9min 의 앞부분과 값 같음) · `floating_narration_all_90s.wav` (원본 앞 93.0 s) 와 짝
- 패널 토크쇼식 대화 (KO 질문자 · DE/FR/ES 답변) · 속도 상한 = 사용자 속도 테스트 (고개 좌우 9 °/s · 14 °/s² · 상하 6 · 10 · 눈 10 · 22) · 내보내기 검사 오류 0
- 고개 상하 −10 ~ 10° 안 (2026-10-09 · 고개 상하 전체 ×0.8 비례 축소 · 이전 최대 12.5° · Blender MotorLimit 도 ±10)
- 음원 · **스피커 PC 웹 `http://<스피커 IP>:8100` → 음원 업로드** → 「애니메이션별 음원」 표에서 같은 이름 애니메이션에 연결 · 로봇 PC 에는 안 올림
- 원본 · `floating/floating_1800/blend/floating_narration_all_9min.blend` · `floating_narration_all_90s.blend`

## floating1_pack ~ floating4_pack

- 로봇마다 팩 하나 · 다른 것 · `robot.yaml` 의 `motion_id` 접두어 (`f1_` … `f4_`) · `pack.yaml` name · 얼굴·눈 메쉬 · 나머지 같음 · 4대 모두 1800 모델 기준 (사이즈 다른 로봇이면 그 팩만 교체)
- 얼굴·눈 메쉬 (1.0.10) · `meshes/face.stl` · `eyeball_l.stl` · `eyeball_r.stl` = 그 자리 디자이너 얼굴 (f1 02번 · f2 03번 · f3 04번 · f4 01번 헤드) · 회색 · 텍스처 없음 · 3만 / 5천 삼각형 · 보기용 (질량·관성은 1800 모델 값 그대로)
- 매달림 비틀림 · `env.torsion_k` 300 · `torsion_c` 90 (2026-10-09 · 4대 9min 물리 보기로 눈으로 맞춘 값 · 실측 아님 · 이전 500 / 150)
- `scene.glb` (웹 「Blender 뷰」) · 헤드 4대만 (디자이너 얼굴 + 눈 · 매장 없음 · 15만 삼각형 · 텍스처 1024) + 9min 애니메이션 556 s · 32 MB · 4개 팩에 같은 파일
  - `pack.yaml` `scene_glb.animations` = `floating_narration_all_9min` · `floating_narration_all_90s` (90s 는 9min 앞부분과 같아서 같은 장면 사용)
  - **애니메이션 고치면 다시 만들어야 함**
- 모델 · `floating/floating_1800/sim/fh_1800_wires_R011.xml` (천장 원판 3점 수직 와이어)
- drive · PC 모터 설정 = 플랫폼 기본값 18000 deg/s · 180000 deg/s² (모터축)

## floating_1 ~ 4

- 로봇 전용 테스트 애니메이션 · 팩은 `floating<N>_pack` 사용
- `floating_1800_dir_test` (방향) · `floating_1800_limit_test` (리미트) · 4대 모두 같은 움직임 · id 만 `f{N}_*`
- `floating_no1_motion1` · `floating_no2_motion1` · 옛 `N-k` id 그대로 · 조인트 이름을 `f{N}_*` 로 바꾼 PC 에서는 움직이지 않음
