# OpenClaw Native Chat Scroll Proof

Generated June 30, 2026 for PR 98258.

## What changed in this proof pass

- iOS proof uses the SwiftUI `OpenClawChatUI` preview host for the actual shared chat view and view model.
- Android proof uses the debug-only `chat-proof` screenshot scene, mounted on the real `ChatMessageListCard`.
- Both proof scenes use long restored history plus delayed stream/tool rows so reviewers can see that new content can arrive without stealing the reader's current position.

## Files

- `ios-chat-scroll-proof.mp4` - iPhone simulator recording of the OpenClaw chat preview.
- `ios-pre-stream.png` - iOS loaded pre-stream state, no stale previous-app label.
- `ios-jump-visible.png` - iOS delayed content arrived out of view; Jump to latest is visible while the visible transcript remains unchanged.
- `ios-after-jump.png` - iOS after Jump; tool and live reply rows are visible and Jump is gone.
- `android-chat-scroll-proof.mp4` - Android emulator recording of the OpenClaw `chat-proof` scene.
- `android-pre-stream.png` - Android loaded pre-stream state.
- `android-jump-visible.png` - Android delayed content arrived out of view; Jump to latest is visible while the visible hierarchy remains on restored context.
- `android-after-jump.png` - Android after Jump tap.

## Runtime evidence notes

The proof recordings are backed by native runtime snapshots/dumps captured during the run:

- iOS before Jump: `snapshot_ui` exposed `Jump to latest reply` as tappable while the visible text list still contained the restored context and did not include the live/tool rows.
- iOS after Jump: `snapshot_ui` no longer exposed Jump; visible text included `Running tools...`, `scroll.proof`, and the live assistant reply.
- Android before Jump: `uiautomator dump` exposed `Jump to latest` as clickable while the visible hierarchy did not include `This live reply` or `scroll.proof`.

## Commands rerun

- `swift build --package-path apps/shared/OpenClawKit --target OpenClawChatUI`
- `ANDROID_HOME=/Users/chrishogg/Library/Android/sdk ANDROID_SDK_ROOT=/Users/chrishogg/Library/Android/sdk ./gradlew :app:installPlayDebug`
- Earlier unit pass in this proof cycle: `ANDROID_HOME=/Users/chrishogg/Library/Android/sdk ANDROID_SDK_ROOT=/Users/chrishogg/Library/Android/sdk ./gradlew :app:testPlayDebugUnitTest --tests ai.openclaw.app.ui.chat.ChatTimelineTest`
