# iOS 27: pausing media during microphone capture silences the call (test case)

Test case for https://bugs.webkit.org/show_bug.cgi?id=325272.

On iOS 27.2.1 (tested on iPhone 16), pausing the only playing media element during microphone capture stops the microphone. The track stays `live`, then becomes `ended` 20–40 seconds later. Does not reproduce on iOS 26 or on WebKit trunk (317782@main).

Test case: https://suke.github.io/ios27-pause-capture-repro/

## Steps to reproduce

1. Open the test case in Safari on an iPhone and tap **Start**.
2. Keep speaking and tap **Toggle mute** three times. Each toggle calls `video.pause()`, flips `video.muted`, then calls `play()`. The first unmute can recover.
3. Wait 45 seconds.

- Expected: **Microphone** stays `capturing and sending`.
- Actual: **Microphone** shows `STALLED`, then **Microphone track** shows `readyState=ended`.

The received audio is a clone of the microphone, so this page shows only the microphone side. In a real call, the remote audio stops too.

## Workarounds

- Toggle only `video.muted`, without calling `pause()` or `play()`.
- Keep another media element playing.
