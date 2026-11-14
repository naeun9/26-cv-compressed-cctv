# 26-cv-compressed-cctv

압축 CCTV 영상(H.264)에서의 작은 객체 탐지 — 압축 영향 분석, 주파수 기반 feature 보정 모듈

## 폴더

| 폴더 | 내용 |
|---|---|
| `preliminary_experiment/` | 사전실험 노트북 (데이터셋 구축, A, B, C·D, TSBOW 확인) |
| `Urban Tracker/` | Urban Tracker 영상 3개 (`rouen_video.avi`, `sherbrooke_video.avi`, `stmarc_video.avi`) |

## 데이터

| 데이터 | 내용 | 출처 |
|---|---|---|
| UVG-VCM | 무압축 원본 → 720p·30fps 무손실 기준 영상, 비트레이트별 H.264 압축 | https://tie-ultravideo.rd.tuni.fi/UVG-VCM |
| TSBOW | 실제 CCTV 압축 영상 인코딩 설정·라벨 확인 (CC BY-NC-ND, 저장소에 미포함) | https://huggingface.co/datasets/SKKUAutoLab/TSBOW |
| Urban Tracker | 고정 카메라 도시 교통 영상 3개 — rouen 1024×576 MPEG-4 25fps 630프레임, sherbrooke 800×600 H.264 30fps 4000프레임, stmarc 1280×720 H.264 29.97fps 2000프레임 | https://www.polymtl.ca/litiv/node/12 |

## 탐지기

- YOLO26s (Ultralytics, COCO 사전학습 가중치 `yolo26s.pt`)

## 저장소에 포함하지 않는 것

- 영상 원본·파생 영상 (`*.yuv`, `*.mkv`, `*.mp4`, `*.avi`), 추출 프레임 — `Urban Tracker/` 3개 영상은 예외
- 모델 가중치 (`*.pt`)

## 인용

Urban Tracker

> J.-P. Jodoin, G.-A. Bilodeau, N. Saunier, "Urban Tracker: Multiple Object Tracking in Urban Mixed Traffic," IEEE Winter Conference on Applications of Computer Vision (WACV), pp. 885–892, 2014. doi:10.1109/WACV.2014.6836010

```bibtex
@inproceedings{jodoin2014urban,
  author    = {Jodoin, Jean-Philippe and Bilodeau, Guillaume-Alexandre and Saunier, Nicolas},
  title     = {Urban Tracker: Multiple Object Tracking in Urban Mixed Traffic},
  booktitle = {IEEE Winter Conference on Applications of Computer Vision (WACV)},
  pages     = {885--892},
  year      = {2014},
  doi       = {10.1109/WACV.2014.6836010}
}
```