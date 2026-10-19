# la

플로팅 헤드 3대 세트 (LA 매장) · f2 · f3 · f4 (f1 안 씀) · 구조는 `ny/` 와 같게: 공연 폴더 · `floating<N>_pack` · `floating<N>_animation`

## 로봇 팩 (1.0.0 · 2026-10-19)

- `floating2_pack` · `floating3_pack` · `floating4_pack` · 로봇 PC 마다 자기 번호 팩을 올림
- 로봇 내용(robot.yaml · model.xml · 얼굴 메쉬 · urdf)은 `ny/floating<N>_pack` 1.0.15 와 같음
- 다른 것은 `scene.glb` · LA 3대만 · 매장 배치 = `D:\blender_files\floating\la\la_floating_audio8.blend` 의 FH_Rig_2/3/4 위치(높이 f3 > f2 > f4 · 세 대가 앞쪽 한 점을 봄) · 관절 노드 15개 위치가 그 파일 리그와 0.01 mm 안에서 같음
- `pack.yaml` `scene_glb.joints` = f2 · f3 · f4 만
- 얼굴 메쉬는 아직 ny 디자이너 헤드
