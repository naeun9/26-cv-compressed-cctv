# TSBOW 확인

TSBOW: CC BY-NC-ND. 영상·이미지는 저장소에 포함하지 않음. Kaggle Secrets `HF_TOKEN` 사용 (코드에 토큰 없음).

| 노트북 | 내용 | Output |
|---|---|---|
| `T1_tsbow_encoding_probe` | TSBOW 영상 20개 인코딩 설정 확인 (ffprobe: 코덱·프로파일·해상도·fps·B프레임·GOP·비트레이트·x264 옵션). 선택 셀은 `A2_uvg_eval2_cctv_encode`의 `cctv_enc`(GOP 250)와 비교 | `tsbow_encoding.csv`, `ours_encoding.csv` |
| `T2_tsbow_val_verify` | `v1.1.0_camera_held_out` val 영상 40개 프로파일, 라벨 프레임 번호 ↔ 영상 프레임 대응 확인. 실행 후 영상·이미지 삭제 | `num1_results/video_profile.csv`, `summary.txt`, `frame_check.csv` |

## T2 결과 (summary.txt)

- val 영상 40개: H.264 High, 1280×720
- GOP(프레임): 250 37개, 241·90·248 각 1개
- 비트레이트(kbps): 최소 311 / 중앙값 1,032 / 최대 2,100
- 라벨 이미지 3,292장 중 영상 프레임 수 범위 안 76장, 픽셀 비교 22장 중 오프셋 0 일치 0장

## results/

`T2_results.zip`: `num1_results/` (위 3개 파일)