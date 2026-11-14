# 00 데이터셋 구축

| 노트북 | 내용 | 설정 | Output |
|---|---|---|---|
| `01_uvg_build_dataset` | UVG-VCM 4K·60fps 원본 스트리밍 → 짝수 프레임(30fps) → 1280×720 yuv420p FFV1 무손실 기준 영상. H.264 2-pass(medium, GOP 250) 4000/2000/1000/500 kbps 압축. 원본 라벨 저장 | Internet On, CPU, ffmpeg 6.1.1 | `uvg_cctv/{seq}/ref_720p30.mkv`, `h264_{k}k.mp4`, `ann_raw.json`, `done.json`, `first.png`/`last.png`, `meta.csv`, `contact_sheet.png` |
| `02_uvg_label_inspect` | `ann_raw.json` 키·객체 구조 확인, 첫 프레임 박스 overlay | Input: `uvg_cctv` | `overlay_{seq}_key{k}.png` |
| `03_uvg_rest_seq_encode` | A-3에 없던 6개 시퀀스를 A-3과 같은 명령으로 압축(4000~125 kbps, 2-pass 500) + 720p GT + YOLO26s 예측. `ONLY_REST = False`면 12개 전체 | Input: `uvg_cctv`, GPU T4, Ultralytics 8.4.165 | `bdata/{seq}/cctv_{k}k.mp4`, `bdata/{seq}/2pass_500k.mp4`, `bdata/gt/`, `bdata/pred/`, `bitrate.csv` |

처리 시퀀스 (12개): HighwayView, CampusView, JobFair, OfficeWalk, TrafficLights, SpotRobot, BoatReverse, VolleyballGame, FloorballTrain, FloorballGame, WinterDrive, HighwayDrive

`03_uvg_rest_seq_encode` 대상 (6개): HighwayDrive, WinterDrive, TrafficLights, OfficeWalk, SpotRobot, BoatReverse

## 03 시퀀스별 GT (720p, 실행 로그)

| 시퀀스 | 프레임 | 전체 박스 | 작은 객체 | CCTV 클래스 박스 | CCTV 작은 객체 |
|---|---|---|---|---|---|
| HighwayDrive | 300 | 2,695 | 1,493 | 2,695 | 1,493 |
| WinterDrive | 300 | 2,311 | 950 | 2,311 | 950 |
| TrafficLights | 300 | 6,219 | 4,187 | 2,325 | 765 |
| SpotRobot | 300 | 3,567 | 890 | 339 | 0 |
| OfficeWalk | 300 | 4,065 | 238 | 0 | 0 |
| BoatReverse | 150 | 943 | 146 | 0 | 0 |

CCTV 클래스: person, bicycle, car, motorcycle, bus, truck