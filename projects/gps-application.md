---
layout: default
title: GPS Application
permalink: /projects/gps-application/
---

# GPS Tracking Application
#### A functional dog tracking application utilizing GPS and LTE (LoRa coming soon. . .)

## Overview

A comprehensive GPS tracking system designed for real-time location monitoring and visualization. This project includes both a production application built for embedded Linux devices and a demo version with mock GPS API for development and demonstration purposes.

<div class="gps-app-container">
  <iframe 
    src="https://portable-dev.taila1da91.ts.net/" 
    frameborder="0">
    Your browser does not support iframes. <a href="https://portable-dev.taila1da91.ts.net/" target="_blank">Click here to view the GPS Application</a>
  </iframe>
</div>

*<a href="https://portable-dev.taila1da91.ts.net/" target="_blank">Open in new tab →</a>*

## Production Application

The main application (`tracker.py`) is designed to run on embedded Linux devices, specifically targeting Raspberry Pi with cellular modem and external GPS hardware. Built for handheld touchscreen devices in field environments.

### Hardware Integration
- **GPS Hardware**: GPS receiver integration via `gpsd` daemon
- **Cellular Connectivity**: LTE modem communication with AT command interface
- **Signal Quality Monitoring**: Real-time cellular signal strength indicators
- **Embedded Display**: PyQt5 GUI optimized for touchscreen Linux devices

### Core Features
- Real-time GPS coordinate capture and validation
- Cellular signal quality monitoring with visual indicators
- Location data transmission to remote servers
- GPS fix status indicators (2D/3D fix detection)
- Graceful error handling and connection recovery

## Demo Application & Mock API

The demo version utilizes a custom-built mock GPS API server to simulate tracking functionality without requiring local or remote GPS hardware: the GPS API parses a .GPX file to generate mock location and tracking data, which is consumed by the tracking application. In the demo, the cellular signal quality and GPS fix data is hardcoded

### Mock GPS Server Architecture
- **Flask/SocketIO Backend**: Real-time bidirectional communication
- **GPX Route Simulation**: Provides realistic location data sequences
- **RESTful API**: Endpoints for user and tracker location data
- **API Key Authentication**: Secure access control

### Demo Features
- **Dual Location Sources**: Separate user and tracker (dog) location streams
- **Real-time Tracking**: Start/stop tracking functionality with state management
- **Distance Calculation**: Haversine formula implementation for accurate distance measurement
- **WebSocket Communication**: Live updates between client and server
- **Interactive Web Interface**: Real-time map visualization with location markers

## Technical Implementation

### Core Technologies
- **Backend**: Python, Flask, SocketIO
- **Frontend**: HTML5, CSS3, JavaScript, Leaflet.js
- **GUI Framework**: PyQt5 with WebEngine integration
- **GPS Integration**: gpsd daemon, real GPS hardware
- **Networking**: Requests library, cellular modem AT commands
- **Data Processing**: Real-time coordinate processing and distance calculations

### Advanced Features
- **Haversine Distance Calculation**: Precise distance measurement between GPS coordinates
- **Signal Quality Analysis**: RSSI/RSRQ parsing and visualization
- **Multi-threaded Architecture**: Separate threads for GPS updates, signal monitoring, and UI
- **Error Recovery**: Automatic reconnection and graceful failure handling
- **Logging System**: Comprehensive logging with rotating file handlers

### Communication Protocols
- **AT Commands**: Direct cellular modem communication for signal monitoring
- **Socket.IO**: Real-time bidirectional event-based communication
- **HTTP/REST**: Location data transmission and API interactions
- **WebSocket**: Live map updates and tracking state synchronization

## Development Architecture

### V2 Evolution
Currently developing version 2 with enhanced hardware integration and expanded API functionality:
- Custom tracking hardware development
- Enhanced communication protocols (exploring LoRa integration)
- Improved API architecture for scalability
- Alternative deployment options (tablet-based solutions)

### Mock API Benefits
- **Development Acceleration**: No physical GPS hardware required for testing
- **Reproducible Testing**: Consistent location data for feature development
- **Demo Capabilities**: Showcase functionality without field deployment
- **API Design Validation**: Test communication protocols and data structures

---

*This project demonstrates expertise in embedded systems development, real-time data processing, GPS/cellular integration, and full-stack application architecture. The combination of production-ready hardware integration and sophisticated development tooling showcases both practical implementation skills and thoughtful development methodology.*