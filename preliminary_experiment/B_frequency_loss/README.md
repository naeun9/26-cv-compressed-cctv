# 사전실험 B: 압축에 따른 주파수 손실과 작은 객체 탐지 실패

노트북: `B1_pre_experiment_b` (B-1·B-2·B-3을 한 노트북에서 실행)

| 항목 | 내용 |
|---|---|
| Input | `uvg_cctv` (기준 영상), `A3_uvg_eval3_gop60` (`cctv_gop60` 압축본), `A1_uvg_eval_yolo` (GT). 가중치는 Input 폴더에서 찾은 `yolo26s.pt` |
| 시퀀스 | HighwayView, CampusView, JobFair, VolleyballGame, FloorballGame, FloorballTrain |
| 비트레이트 | 4000 / 2000 / 1000 / 500 / 250 / 125 kbps (GOP 60) |
| 프레임 | 5프레임 간격 |
| Y 추출 | `cv2.VideoCapture` BGR → `cv2.COLOR_BGR2YCrCb` Y 채널 |
| 환경 | GPU T4, PyWavelets, Ultralytics (버전 미고정) |

## B-1 전체 프레임 주파수 손실

- 8×8 블록 DCT: 대각선(u+v) 0–14, 요약 대역 DC(0)·Low(1–2)·Mid(3–6)·High(7–14)
- 2레벨 Haar Wavelet: LL2, LH2, HL2, HH2, LH1, HL1, HH1
- 지표: 참조/압축/오차 에너지, 에너지 비율, normalized_error, MSE, Y-PSNR

## B-2 객체 ROI vs 배경 ROI

- Object ROI: GT 박스
- Background ROI: 같은 크기 박스를 무작위 위치에 배치, 모든 GT 박스와 겹치지 않는 위치만 사용 (최대 30회 시도, `SEED = 42`)
- 크기: Small(<32²), Medium(32²~96²), Large(≥96²)

## B-3 주파수 오차와 탐지 실패

- YOLO26s `imgsz=640`, `conf=0.001`, `max_det=300`, 입력 `cv2.VideoCapture` BGR 프레임
- 참조 영상에서 검출된 GT만 대상, 같은 클래스·IoU ≥ 0.5 매칭, 판정 conf 0.25(주)·0.10(민감도)
- (sequence, frame, annotation_id, bitrate_kbps)로 B-2 Object ROI 측정값과 결합

## B_results.zip

| 폴더 | 파일 |
|---|---|
| `B1_results/` | `freqB1_frame.csv`, `freqB1_bitrate.csv`, `freqB1_psnr.csv`, `B1_overall_DCT_energy_ratio.png`, 시퀀스별 `{seq}_DCT_summary_error.png`·`{seq}_Wavelet_error.png` |
| `B2_results/` | `B2_roi_raw.csv` (154 MB), `B2_roi_summary.csv`, `B2_DCT_{Low,Mid,High}_size.png` |
| `B3_results/` | `B3_detection_status.csv`, `B3_miss_rate.csv`, `B3_frequency_comparison.csv`, `B3_frequency_detection_merged.csv` (79 MB), `B3_within_stratum_difference.csv`, `B3_miss_rate.png`, `B3_small_DCT_error_difference.png` |
