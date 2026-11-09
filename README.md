# SimBridge — Bluetooth Phone Companion for Android

SimBridge connects two Android smartphones over Bluetooth Classic (RFCOMM) allowing a secondary device without a cellular SIM card to remotely view alerts and control supported call and messaging operations of a SIM-equipped host phone.

## Key Highlights
- **Single Universal APK**: One build runs on both Phone A (Host with SIM) and Phone B (Remote Controller without SIM).
- **Zero-Cloud & 100% Local**: Works strictly peer-to-peer over local Bluetooth RFCOMM sockets. Does not require Wi-Fi, mobile data, internet access, or cloud servers.
- **Cellular Audio Stays on Host**: In compliance with Android platform standards, cellular call voice audio remains strictly on Phone A (earpiece, speakerphone, or connected Bluetooth headset).
- **Robust Versioned Protocol**: Custom framed transport with magic header (`SMBR`), 4-byte length prefix, and CRC32 payload checksum.
- **Mutual Security Pairing**: Challenge-response handshake utilizing 6-digit confirmation PIN and HMAC-SHA256 authenticated verification.
- **Bilingual & RTL-Ready**: Full English (LTR) and Persian / فارسی (RTL) localization with instant in-app switching.
- **Android 8.0 to 15+ Compatibility**: Complies with modern Android foreground service restrictions (`connectedDevice` type) and least-privilege permission models.

---

## Architecture Overview

```
 ┌─────────────────────────────────────────────────────────────┐
 │                     SimBridge Architecture                  │
 └─────────────────────────────────────────────────────────────┘

       Phone A (Host with SIM)                  Phone B (Remote Controller)
  ┌─────────────────────────────────┐      ┌─────────────────────────────────┐
  │ Telephony & Telecom Integration │      │   Jetpack Compose UI (M3)       │
  │ • TelephonyManager / Calls      │      │   • Calls Tab (History/Callback)│
  │ • SmsReceiver / SmsSender       │      │   • Messages Tab (Conversations)│
  │ • CallLog & Contacts Resolver   │      │   • Dialer (Remote Dispatch)    │
  │ • InCallService (Telecom)       │      │   • Real-Time Incoming Overlay  │
  └────────────────┬────────────────┘      └────────────────┬────────────────┘
                   │                                        │
  ┌────────────────┴────────────────┐      ┌────────────────┴────────────────┐
  │  CompanionService (Host Server) │      │ CompanionService (Client daemon)│
  │  • Bluetooth RFCOMM Server      │◄────►│ • RFCOMM Client Socket          │
  │  • Protocol Router & Security   │      │ • NotificationHelper (Heads-up) │
  │  • Foreground Service           │      │ • Auto-Reconnect Backoff Engine │
  └─────────────────────────────────┘      └─────────────────────────────────┘
```

---

## First-Run Setup on Both Phones

1. **Install the same APK** on both devices.
2. **On Phone A (Host - With SIM)**:
   - Launch SimBridge.
   - Select **Phone A — Host (SIM Card)**.
   - Tap **Grant Permissions** (Phone State, Calls, SMS, Call Log, Contacts, Bluetooth, Notifications).
   - SimBridge starts its background RFCOMM server and displays "Waiting for Controller...".
3. **On Phone B (Remote Controller - No SIM)**:
   - Launch SimBridge.
   - Select **Phone B — Remote Controller**.
   - Tap **Grant Permissions** (Bluetooth, Notifications).
   - In the Controller dashboard, tap **Connect** or the Bluetooth icon to open the device picker.
   - Tap **Scan for Devices** or select Phone A from paired devices.
4. **Security Pairing**:
   - Both devices will display a mutual 6-digit confirmation code.
   - Verify that the numbers match, then tap **Confirm & Authorize**.
   - The connection is established! Phone B immediately syncs call logs and SMS conversations.
  

---
https://myket.ir/app/com.aistudio.simbridge.vxcp

https://cafebazaar.ir/app/com.aistudio.simbridge.vxcp?l=en
