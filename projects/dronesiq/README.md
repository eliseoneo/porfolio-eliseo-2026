# dronesiq

Bridge Vexta Drones IQ — receive video streams from drones and external devices via RTMP, convert to video files, and process KMZ geospatial data.

## Components

| Component | Description |
|-----------|-------------|
| **Unified Launcher** | Service launcher for starting multiple components in one command |
| **Webhook Server** | HTTP server for receiving event callbacks and logging structured data |
| **MQTT Client** | Client for pub/sub communication with broker for device telemetry |
| **RTMP Server** | Server for receiving video streams from external sources |
| **RTMP Receiver** | Legacy receiver for RTMP streams with API integration |
| **Stream Recorder** | Module for converting RTMP streams to various video formats |
| **KMZ Reader** | Tool for reading and extracting data from KMZ geospatial files |
| **Frame Extractor** | Utility for extracting image frames from video files |
| **WebRTC Receiver** | Server for accepting WebRTC streams from browsers |
| **ngrok Tunnels** | Scripts for exposing local services via secure tunnels |
| **Device Fetch** | Service for retrieving and caching device information |
| **Flight Task** | Component for processing flight task data from MQTT streams |
| **Bubble Callback** | Integration for posting results to external endpoints |
| **Bubble Flight Status** | Service for sending flight status updates |
| **S3 media frames** | Module for managing media frames stored in cloud storage |

## Architecture

```
                        External Sync Service
                        ┌──────────────────────────────────┐
                        │  Webhook caller  (HTTP POST)      │
                        │  MQTT Broker     (pub/sub)        │
                        │  Stream forwarder (RTMP push)     │
                        └───────┬──────────┬──────────┬─────┘
                                │          │          │
                           (webhook)    (MQTT)     (RTMP)
                                │          │          │
                        ┌───────┴──────────┴──────────┴─────┐
                        │  dronesiq (this project)           │
                        │                                    │
                        │  Webhook Server        :port       │
                        │  MQTT Client           (outbound)  │
                        │  RTMP Server           :port        │
                        │  Auth handler          :port        │
                        │  Stream capture        (pull)      │
                        └───────┬──────────┬──────────┬─────┘
                                │          │          │
                                ▼          ▼          ▼
                        webhook_logs/  mqtt_logs/  recordings/
```

## Prerequisites

- Python 3.10+
- FFmpeg for video processing
- Docker + Docker Compose for containerized services
- Open ports for RTMP and HTTP services

## Recent updates — VLM integration and inference

Summary of changes for vision-language model integration and automated findings:

| Area | Change |
|------|--------|
| **VLM → findings parse chain** | Text responses parsed to structured detections for automated processing |
| **VLM response schema** | Standardized JSON format for detections with coordinates and metadata |
| **Finding gate** | Automated creation of findings based on detection results |
| **Confidence handling** | Default values and validation for confidence scores |
| **Findings payload** | Structured data including observations and metadata |
| **Multi-model support** | Integration with different AI models for inference |
| **Batch processing** | Support for processing multiple detections efficiently |
| **Environment variables** | Configuration options for different deployment scenarios |
| **Image source selection** | Choice between different image sources for inference |
| **S3 integration** | Cloud storage integration for media management |
| **Waypoint matching** | Geospatial processing for flight path analysis |

## Inference image source: local vs cloud storage

System supports multiple sources for inference images:

- **Local capture**: Images extracted from RTMP video streams
- **Cloud storage**: Images retrieved from remote storage with fallback logic
- **Retry mechanisms**: Polling and prefetching for reliable image retrieval
- **Waypoint alignment**: Matching images to specific flight waypoints
- **Fallback policies**: Graceful degradation when primary sources unavailable

## Quick Start

### 1. Configure environment

Set up configuration file with required parameters.

### 2. Activate virtual environment

```bash
source venv/bin/activate
```

### 3. Install dependencies

```bash
pip install -r requirements.txt
```

### 4. Start services

```bash
docker-compose up --build -d
```

### 5. Run application

```bash
python services/run.py
```

## Wayline pipeline API

REST API for processing geospatial data and waypoints:

- POST endpoint for initiating waypoint synchronization
- GET endpoint for retrieving cached waypoint data
- Background processing with Redis caching
- Authentication and authorization mechanisms

## RTMP server

Pure Python RTMP server for video stream handling:

- Multi-channel support
- Automatic startup with main application
- Logging and monitoring capabilities
- Reconnection handling

## MQTT integration

Client for device telemetry via MQTT:

- Pub/sub pattern implementation
- Device discovery and caching
- Message parsing and storage
- Coordinate matching with waypoints

## Drone camera integration

Theoretical investigation of video stream sources:

- Multiple camera types (dock vs drone payload)
- Different API mechanisms for stream initiation
- Elevation-based triggers for stream switching
- Fallback strategies for different flight phases

## External integrations

Communication with external services:

- Webhook callbacks for event notifications
- REST API calls for data synchronization
- Cloud storage operations
- External endpoint notifications