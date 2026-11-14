# 00 데이터셋 구축

| 노트북 | 내용 | 설정 | Output |
|---|---|---|---|
| `01_uvg_build_dataset` | UVG-VCM 4K·60fps 원본 스트리밍 → 짝수 프레임(30fps) → 1280×720 yuv420p FFV1 무손실 기준 영상. H.264 2-pass(medium, GOP 250) 4000/2000/1000/500 kbps 압축. 원본 라벨 저장 | Internet On, CPU | `uvg_cctv/{seq}/ref_720p30.mkv`, `h264_{k}k.mp4`, `ann_raw.json`, `done.json`, `first.png`/`last.png`, `meta.csv`, `contact_sheet.png` |
| `02_uvg_label_inspect` | `ann_raw.json` 키·객체 구조 확인, 첫 프레임 박스 overlay | Input: `uvg_cctv` | `overlay_{seq}_key{k}.png` |

처리 시퀀스 (12개): HighwayView, CampusView, JobFair, OfficeWalk, TrafficLights, SpotRobot, BoatReverse, VolleyballGame, FloorballTrain, FloorballGame, WinterDrive, HighwayDrive
