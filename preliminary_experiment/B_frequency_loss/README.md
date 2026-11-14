# 사전실험 B: 데이터 준비

| 노트북 | 내용 | Output |
|---|---|---|
| `00_uvg_B_data_prep` | A-3에 없던 6개 시퀀스를 A-3과 같은 명령으로 압축 + GT + YOLO26s 예측. `ONLY_REST = False`면 12개 전체 | `bdata/{seq}/cctv_{4000..125}k.mp4`, `bdata/{seq}/2pass_500k.mp4`, `bdata/gt/`, `bdata/pred/`, `bitrate.csv`, `check_a3.csv` |

대상 시퀀스: HighwayDrive, WinterDrive, TrafficLights, OfficeWalk, SpotRobot, BoatReverse
