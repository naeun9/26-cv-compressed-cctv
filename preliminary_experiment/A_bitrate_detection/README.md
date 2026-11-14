# 사전실험 A: 압축 비트레이트별 YOLO26s 작은 객체 탐지 성능

| 노트북 | 차수 | 인코딩 | 비트레이트 (kbps) | Output |
|---|---|---|---|---|
| `A1_uvg_eval_yolo` | A-1 | 2-pass, medium, GOP 250 (`01_uvg_build_dataset` 압축본) | 4000 / 2000 / 1000 / 500 | `results_all.csv`, `results_bitrate.csv`, `gt_{seq}_720p30.json`, `pred_{seq}_{level}.json`, `overlay_{seq}.png` |
| `A2_uvg_eval2_cctv_encode` | A-2 | CCTV형 1-pass CBR, veryfast, zerolatency, GOP 250 | 4000 ~ 125 + 2-pass 500 (A-1 압축본) | `results2_all.csv`, `results2_bitrate.csv`, `cctv_enc/` |
| `A3_uvg_eval3_gop60` | A-3 (최종) | CCTV형 1-pass CBR, veryfast, zerolatency, GOP 60 | 4000 ~ 125 + 2-pass 500 (GOP 60 재인코딩) | `results3_all.csv`, `results3_bitrate.csv`, `cctv_gop60/` |
| `A_tsbow_encoding_probe` | 근거 | TSBOW 영상 20개 인코딩 설정 확인 (ffprobe) | – | `tsbow_encoding.csv`, `ours_encoding.csv` (선택) |

## 평가

- 평가 시퀀스: HighwayView, JobFair, VolleyballGame, FloorballGame, FloorballTrain, CampusView
- 묶음: Highway+Campus CCTV 클래스 / Volleyball+Floorball 2개 sports ball / 고정 시점 5개 (참고)
- CCTV 클래스: person, bicycle, car, motorcycle, bus, truck

## A-3 인코딩 명령

```
ffmpeg -i ref_720p30.mkv -c:v libx264 -preset veryfast -tune zerolatency -profile:v high -pix_fmt yuv420p \
  -b:v {k}k -maxrate {k}k -bufsize {k}k -g 60 -keyint_min 60 -sc_threshold 0 -an
```

## TSBOW 노트북

- Private 노트북, Kaggle Secrets `HF_TOKEN` 사용 (코드에 토큰 없음)
- 비교 셀은 `cctv_enc` (A-2, GOP 250) 출력을 읽음

## results/

각 노트북 Output의 CSV를 넣는 폴더 (`results_*.csv`, `results2_*.csv`, `results3_*.csv`, `tsbow_encoding.csv`)
