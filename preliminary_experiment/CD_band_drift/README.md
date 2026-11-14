# 사전실험 C·D: 주파수 대역 교체(Oracle)와 탐지기 내부 Feature Drift

노트북: `CD1_cd_preliminary`

| 항목 | 내용 |
|---|---|
| Input | `uvg_cctv` (기준 영상), `bdata` (`03_uvg_rest_seq_encode` Output) |
| 압축 영상 | `bdata`에 해당 시퀀스가 없으면 A-3과 같은 명령으로 재인코딩 (평가 5개 시퀀스는 재인코딩본 사용) |
| 시퀀스 | CCTV: HighwayView, CampusView / Ball: VolleyballGame, FloorballGame, FloorballTrain |
| 비트레이트 | 1000 / 500 / 250 kbps |
| 프레임 | 5프레임 간격 |
| 탐지 | Ultralytics 8.4.165, YOLO26s, `imgsz` 640·1280, `conf=0.001`, `max_det=300` |
| 입력 경로 | ffmpeg yuv420p 디코딩 → 640은 `cv2.INTER_AREA`로 640×360 축소 → OpenCV I420→BGR |
| 매칭 | conf 0.25, IoU 0.5, 같은 클래스 |

## C: 주파수 대역 교체

- 8×8 블록 DCT (Y), 대역 DC(0)·Low(1–2)·Mid(3–6)·High(7–14)
- 조건: Baseline / Low·Mid·High 원본 계수로 교체 / High_remove (압축 High 계수 0) / Reference
- 대역 교체 조건의 색차(U·V)는 압축본, Reference만 원본
- 지표: APs·APm·APl, 회복량, 작은 객체 회복률 (track 단위 bootstrap 1,000회)

## D: Feature Drift

- 원본·1000·500·250 kbps를 한 배치로 입력, forward hook
- 층: Backbone stride 4·8·16·32 (layer 2·4·6·10), Neck P3·P4·P5 (Detect 입력)
- 지표: relative error, cosine similarity, 채널 정규화 MSE, 작은 객체 영역 버전
- 주입 위치 후보: 프레임별 작은 객체 영역 drift와 작은 객체 미탐률의 Spearman ρ, (시퀀스, 비트레이트) 그룹 평균

## CD_results.zip

| 파일 | 내용 |
|---|---|
| `C_ap.csv` | 그룹·비트레이트·조건·입력 크기별 AP |
| `C_object.csv` | 객체별 탐지 여부 |
| `C_object_recovery.csv` | 작은 객체 회복률, bootstrap 95% 구간 |
| `C_per_sequence.csv` | 시퀀스별 회복률 |
| `D_feature_drift.csv` | 프레임·층별 drift |
| `D_frame_miss.csv` | 프레임별 작은 객체 미탐률 |
| `D_summary.csv` | 그룹·비트레이트·층별 평균 drift |
| `D_injection_candidates.csv` | 층별 Spearman ρ 평균 |
| `C_fig_band_APs_{cctv,ball}.png`, `D_fig_drift_{cctv,ball}.png` | 그림 |
| `cd_lib.py` | 노트북 공통 함수 |
