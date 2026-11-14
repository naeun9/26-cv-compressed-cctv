# 사전실험

모든 노트북은 Kaggle에서 실행.

## 폴더

| 폴더 | 내용 |
|---|---|
| `00_dataset_build/` | UVG-VCM → 720p·30fps 무손실 기준 영상·라벨, 나머지 6개 시퀀스 압축 |
| `A_bitrate_detection/` | 사전실험 A: 비트레이트별 YOLO26s 작은 객체 탐지 성능 (A-1·A-2·A-3) |
| `B_frequency_loss/` | 사전실험 B: 압축에 따른 주파수 손실, 객체 ROI 비교, 탐지 실패와의 연관 (B-1·B-2·B-3) |
| `CD_band_drift/` | 사전실험 C: 주파수 대역 교체(Oracle), D: 탐지기 내부 feature drift |
| `TSBOW_check/` | TSBOW 영상 인코딩 설정·라벨 프레임 대응 확인 |

## 실행 순서 (Kaggle Input 관계)

```
01_uvg_build_dataset ──▶ uvg_cctv
   ├─▶ 02_uvg_label_inspect
   ├─▶ 03_uvg_rest_seq_encode ──▶ bdata
   ├─▶ A1_uvg_eval_yolo ──▶ gt_{seq}_720p30.json
   ├─▶ A2_uvg_eval2_cctv_encode ──▶ cctv_enc ──▶ T1_tsbow_encoding_probe (비교 셀)
   ├─▶ A3_uvg_eval3_gop60 ──▶ cctv_gop60
   │      └─▶ B1_pre_experiment_b (Input: uvg_cctv, A1, A3)
   └─▶ CD1_cd_preliminary (Input: uvg_cctv, bdata)

T2_tsbow_val_verify (Input 없음, Hugging Face에서 다운로드)
```

## 공통 조건

- 데이터: UVG-VCM (A·B·C·D), TSBOW (TSBOW_check). `Urban Tracker/` 영상은 사전실험 노트북에서 사용하지 않음
- 기준 영상: UVG-VCM 4K·60fps → 짝수 프레임(30fps) → 1280×720 yuv420p FFV1 무손실
- 압축(A-3 이후): libx264, veryfast, zerolatency, high, CBR(`-b:v`=`-maxrate`=`-bufsize`), GOP 60, scene cut 비활성화
- 탐지: YOLO26s, `conf=0.001`, `max_det=300`
- Ultralytics 버전: A·데이터 준비·C·D `8.4.165` 고정, B 미고정