# RFID_NFC-airtag
## Automatic Gate Opening using RFID & NFC (ESP32)

An ESP32-based automatic gate/parking access control system using RFID (RC522) card detection for member and visitor vehicle management — inspired by real mall/parking entry-exit workflows.

# Overview

This project automates gate entry and exit for a parking/access system by reading RFID/NFC cards, checking membership status against a database, and triggering a relay-controlled gate. It supports both registered members and walk-in visitors, with separate logic paths for each.

# System Architecture
                    ESP32-S3
                       |
        ---------------------------------
        |                               |
      RC522                           WiFi
        |                               |
    RFID UID                    Database / API
                                        |
                                Membership check
                                        |
                    ---------------------------------
                    |                               |
                  MEMBER                         VISITOR
                    |                               |
                Access OK                  Temporary record
                    |                               |
                    ---------------------------------
                                    |
                                  RELAY
                                    |
                                  GATE


## How It Works
1. Entry Flow


VEHICLE ARRIVES
      |
      
------------------------
| Entry Terminal         |
| RFID Reader            |
------------------------+
      |
  Card detected
      |
      v
+------------------------+
| Check database         |
+------------------------+
      |
      -------------------------------
      |                             |
  MEMBER CARD                  NEW VISITOR
      |                             |
Check membership              Create ticket /
validity/status                temporary ID
      |                             |
   -------------                    |
   |           |                    v
 VALID     EXPIRED            OPEN + RECORD
   |           |                ENTRY TIME
   v           v
OPEN GATE   DENY


 # 2.Exit-flow

Vehicle arrives at EXIT
        |
        v
    Read RFID
        |
        v
Find parking session
        |
        v
Calculate status/fee
        |
   ---------------
   |             |
 MEMBER       VISITOR
   |             |
   v             v
FREE/        PAY
DISCOUNT     REQUIRED
   |             |
   ---------------
        |
        v
    OPEN GATE
        |
        v
Close parking session

# Production-Grade Architecture (Target Design)
MEMBER CARD
    |
    v
+--------------------------+
| Secure RFID Reader       |
| 13.56 MHz                |
+--------------------------+
    |
  SPI/UART
    |
    v
+--------------------------+
|        ESP32-S3          |
|  Authenticate card       |
|  Check membership        |
|  Check expiry            |
|  Anti-passback           |
|  Log access              |
+--------------------------+
    |
 VALID / INVALID
    |
    -------------------
    |                 |
Gate controller   Alarm/display
    |
    v
 OPEN/CLOSE

## Hardware
ESP32-S3 microcontroller
RC532 RFID/NFC reader module (13.56 MHz)
Relay module (gate/actuator control)
Gate controller / actuator hardware
WiFi connectivity for database/API communication

# Contributing

Contributions, issues, and feature requests are welcome. Feel free to check the issues page if you want to contribute.
