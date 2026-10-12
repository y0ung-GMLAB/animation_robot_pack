# animation_robot_pack

로봇별 애니메이션 · 로봇 팩 보관소 · 받아서 웹 화면에 드래그로 올리는 용도
(로봇 PC 가 git pull 하는 저장소 아님 · 플랫폼 코드는 `robot_web`)

```
<공연>/                      → 공연 애니메이션 폴더 (예: floating_all_narration) · 여러 로봇 공통
  <애니메이션>.json         → 웹 · 애니메이션 재생 화면에 드래그 (모든 로봇 공통 · 로봇마다 자기 motion_id 만 씀)
  <애니메이션>.wav          → 스피커 PC 웹(:8100) · 음원 업로드 · 「애니메이션별 음원」 에서 같은 이름 애니메이션에 연결
floating<N>_pack/            → N번 로봇 PC 의 웹 · 시스템 정보 → 로봇 팩에 폴더째 드래그 (PC 전역 · 교체)
floating<N>_animation/animations/*.json → N번 로봇 전용 테스트 애니메이션 (파일 이름 floating<N>_*)
```

- 애니메이션 · Blender export 결과 그대로 (`motion_header` JSON Lines · 50 fps · rad)
- 로봇 팩 · 폴더로 저장 · zip 불필요 (화면이 묶어서 올림)
  - 형식 · `robot_web/scripts/sim/README.md` 「로봇 팩 형식」
  - `pack.yaml` 의 `version` · 내용 바꿀 때마다 올림 (웹에 이름·버전 표시)
  - 서버 검사 실패 시 교체 안 됨 · 화면에 이유 목록
  - 팩의 `motion_id` = 애니메이션 파일 id 완전 일치 (다르면 sim 에서 그 축 0° 유지)
  - 팩은 로봇마다 따로 (`robot.yaml` 의 `motion_id` 가 그 로봇 것) · 한 팩을 여러 PC 에 올리면 미리보기가 다른 로봇 움직임을 보여 줌
  - 올리기 전 검사 · robot_web 에서 `uv run --no-project --with mujoco --with numpy --with pyyaml python scripts/sim/sim_run.py --check <팩 폴더>`

## floating_all_narration

- 플로팅 4대 나레이션 공연 · 애니메이션마다 조인트 20개 (`f{N}_neck_yaw` …) · 4대 공통 (각 PC 는 자기 `f{N}_*` 만 씀)
- 로봇 번호 · `Cam_Store` 에서 볼 때 왼쪽부터 f1 KO · f2 ES · f3 FR · f4 DE (2026-10-08)
- `floating_narration_all_9min.json` · 전체 556 s (27801 프레임) · `floating_narration_all_9min.wav` 와 짝
- `floating_narration_all_90s.json` · 앞 93 s 시안 (4650 프레임 · 9min 의 앞부분과 값 같음) · `floating_narration_all_90s.wav` (원본 앞 93.0 s) 와 짝
- 패널 토크쇼식 대화 (KO 질문자 · DE/FR/ES 답변) · 속도 상한 = 사용자 속도 테스트 (고개 좌우 9 °/s · 14 °/s² · 상하 6 · 10 · 눈 10 · 22) · 내보내기 검사 오류 0
- 고개 상하 −10 ~ 10° 안 (2026-10-09 · 고개 상하 전체 ×0.8 비례 축소 · 이전 최대 12.5° · Blender MotorLimit 도 ±10)
- 시작 자세 = 모든 축 0 (2026-10-12 · 이전엔 1프레임이 듣는 자세라 최대 6° 떨어져 있었음 · 처음 5 s(253 프레임) 안의 시작 동작만 0 에서 출발하도록 고침 · 그 뒤는 그대로)
- 음원 · **스피커 PC 웹 `http://<스피커 IP>:8100` → 음원 업로드** → 「애니메이션별 음원」 표에서 같은 이름 애니메이션에 연결 · 로봇 PC 에는 안 올림
- 원본 · `floating/floating_1800/blend/floating_narration_all_9min.blend` · `floating_narration_all_90s.blend`

## floating1_pack ~ floating4_pack

- 로봇마다 팩 하나 · 다른 것 · `robot.yaml` 의 `motion_id` 접두어 (`f1_` … `f4_`) · `pack.yaml` name · 얼굴·눈 메쉬 · 얼굴 무게중심·관성 · 눈알 회전 중심 · 기구(목·눈 모터·링크)는 4대 같음 (1800 CAD)
- 얼굴·눈 메쉬 (1.0.10) · `meshes/face.stl` · `eyeball_l.stl` · `eyeball_r.stl` = 그 자리 디자이너 얼굴 (f1 02번 · f2 03번 · f3 04번 · f4 01번 헤드) · 회색 · 텍스처 없음 · 3만 / 5천 삼각형
- 로봇별 물리 (1.0.11 · 2026-10-12)
  - 얼굴 245 kg (4대 같음) · 무게중심·관성 = 그 로봇 `face.stl` 균일 껍질 (1.0.10 까지는 1800 Fusion 얼굴 껍질)
  - 눈알 회전 중심 (유니버설 조인트) = Blender 리그 눈 본 머리 · x ±243.9 / ±236.8 / ±248.2 / ±277.1 mm (f1~f4) · y −381 · z 157.2 · 눈알 바디(20 kg)는 통째로 같이 이동 · 링크 비율(눈 좌우 0.955 · 상하 −0.56)은 그대로
  - 정지 목 상하 토크(0°) · f1 189 · f2 236 · f3 120 · f4 149 Nm (1.0.10 공통 78 Nm · 얼굴 무게중심이 앞으로 옴 · 균일 껍질 가정이라 실측 필요)
- 매달림 비틀림 · `env.torsion_k` 300 · `torsion_c` 90 (2026-10-09 · 4대 9min 물리 보기로 눈으로 맞춘 값 · 실측 아님 · 이전 500 / 150)
- `scene.glb` (웹 「Blender 뷰」 · 1.0.13 · robot_web 수정 목록 98) · 헤드 4대 모델만 (디자이너 얼굴 + 눈 · 매장 없음 · 15만 삼각형 · 텍스처 1024) · **애니메이션 없음** · 21 MB · 4개 팩에 같은 파일
  - 웹이 지금 보는(재생 중인) 애니메이션 json 값으로 노드를 돌림 → **새 애니메이션마다 glb 다시 만들 필요 없음** (dir test 도 바로 보임 · json 에 없는 로봇은 기본 자세)
  - `pack.yaml` `scene_glb.joints` · 20개 (`f1_neck_yaw` … `f4_eye_yaw_r`) · 노드 이름 = 조인트 이름 · 축 = 노드 로컬 (목·눈 좌우 [0, 0, 1] · 목·눈 상하 [1, 0, 0])
  - 노드 기본 자세 = 모든 값 0 · 웹 방식(기본 자세 × 축 회전)으로 9min 전체를 돌려 예전 구운 장면과 같음 확인 (차이 0.00002°)
  - 얼굴·눈 메쉬나 리그를 바꿀 때만 다시 만듦
- 모델 · `floating/floating_1800/sim/fh_1800_wires_R011.xml` (천장 원판 3점 수직 와이어)
- drive · PC 모터 설정 = 플랫폼 기본값 18000 deg/s · 180000 deg/s² (모터축)

## floating1_animation ~ floating4_animation

- 로봇 전용 테스트 애니메이션 · 팩은 `floating<N>_pack` 사용
- `floating<N>_dir_test` (방향 확인) · 4대 모두 같은 움직임 · id 만 `f{N}_*` (그 로봇 5축만 들어 있음)
  - 17 s (850 프레임) · 목 좌우 +5 → 목 상하 −5 → 눈 상하 −5 → 눈 좌우 +7 (눈알 각도) 차례로 쌓고 1 s 씩 유지 → 모두 0 · 속도 상한 안 (검사 오류·경고 0)
  - 원본 · `floating/floating_1800/blend/floating_dir_test.blend` (리그 4대 · 내보낸 20축을 로봇별 5축으로 나눔)
