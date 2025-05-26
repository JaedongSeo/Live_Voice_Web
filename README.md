# 🗣️ Real-Time Speech Recognition Web Application

## 📌 프로젝트 개요

이 프로젝트는 브라우저에서 마이크 입력을 통해 실시간으로 음성을 텍스트로 변환(STT, Speech-to-Text)하는 웹 애플리케이션입니다. OpenAI Whisper API 또는 로컬 음성 모델을 활용하며, WebSocket을 통해 실시간 스트리밍 처리를 구현합니다.

## 🎯 문제 정의

- **배경**: 음성 기반 인터페이스의 수요 증가와 실시간 처리에 대한 요구
- **문제점**: 일반 STT 시스템은 실시간성이 부족하거나, 설치가 복잡함
- **목표**: 사용자 친화적이고 반응성이 뛰어난 실시간 음성 인식 시스템 구현

## ✅ 요구사항 분석

### 기능 요구사항
- 실시간 음성 입력 처리
- STT 변환 결과 실시간 표시
- 텍스트 로그 저장 또는 활용

### 비기능 요구사항
- 낮은 지연 시간(<1초)
- 다양한 브라우저 호환
- 안정적인 WebSocket 연결

## 🛠 기술 스택 및 아키텍처

- **Frontend**: React.js, JavaScript, MediaRecorder API
- **Backend**: Node.js, Express.js, socket.io
- **AI Model**: Whisper API (OpenAI) 또는 로컬 whisper.cpp
- **Infra**: Docker, Nginx, GitHub Actions

### 아키텍처 흐름
```
사용자 마이크
     ↓
MediaRecorder → WebSocket → Node.js 서버 → Whisper API
                                         ↓
                             실시간 텍스트 결과 반환
```

## 🧠 핵심 처리 메커니즘

- **오디오 캡처**: MediaRecorder로 일정 시간 단위 Blob 생성
- **데이터 전송**: WebSocket으로 서버에 바이너리 데이터 전송
- **서버 처리**: 오디오 → 텍스트 변환 후 WebSocket으로 클라이언트에 반환

## 🧩 주요 코드

### 클라이언트 측 오디오 스트리밍
```javascript
const mediaRecorder = new MediaRecorder(stream);
mediaRecorder.ondataavailable = (event) => {
    socket.emit('audio_chunk', event.data);
};
```

### 서버 측 처리
```javascript
socket.on('audio_chunk', (blob) => {
    const buffer = Buffer.from(blob);
    const text = await transcribe(buffer); // Whisper API 호출
    socket.emit('stt_result', text);
});
```

## 🐞 문제 및 해결

| 문제                           | 해결 방법                             |
|--------------------------------|----------------------------------------|
| WebSocket 연결 불안정          | ping/pong heartbeat 구현               |
| Whisper API 응답 지연          | 오디오 청크 크기 조정 및 버퍼링 최적화 |
| 브라우저 호환성 이슈           | MediaRecorder 지원 브라우저로 제한     |

## ✨ 얻은 인사이트

- 실시간 스트리밍 처리의 병목 구간을 파악하는 능력 향상
- 음성 모델 적용 및 최적화 경험
- 프론트엔드/백엔드 간 실시간 통신 설계 이해도 향상

## 🔮 향후 개선 방향

- 로컬 Whisper 모델 도입으로 외부 API 의존성 제거
- 음성 명령 시스템으로 확장
- 텍스트 요약 및 분석 기능 추가

## 🌐 Colab 주소

https://colab.research.google.com/drive/1nVLTtgvtf9HWiLrm1zoCgW3rpHTAnyhy?usp=sharing
---
