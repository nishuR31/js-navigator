

# Browser Navigator API Guide

[![Made with JavaScript](https://img.shields.io/badge/Made%20with%20JavaScript-yellow?logo=javascript)](https://developer.mozilla.org/en-US/docs/Web/JavaScript)
[![Browser Support](https://img.shields.io/badge/Browser%20APIs-blue?logo=googlechrome)](https://developer.mozilla.org/en-US/docs/Web/API)
[![MDN Docs](https://img.shields.io/badge/MDN-orange?logo=mozilla)](https://developer.mozilla.org/)
[![Contributions Welcome](https://img.shields.io/badge/Contributions%20Welcome-brightgreen)](https://github.com/)

A **complete reference of `navigator` APIs in JavaScript** across modern browsers.
This repo demonstrates how to use **Geolocation, Clipboard, Online/Offline, Device Info, Notifications, Sharing, Media, Sensors**, and more — with examples and docs.

---

##  Table of Contents

1. [Online / Offline Detection](#-online--offline-detection)
2. [Geolocation API](#-geolocation-api)
3. [Notifications API](#-notifications-api)
4. [Clipboard API](#-clipboard-api)
5. [Share API](#-share-api)
6. [Device & OS Info](#-device--os-info)
7. [Battery API](#-battery-api)
8. [Media Devices (Camera/Mic)](#-media-devices)
9. [Vibration API](#-vibration-api)
10. [Device Memory & Hardware Concurrency](#-device-memory--cpu)
11. [Orientation & Motion](#-device-orientation--motion)
12. [Service Worker / PWA](#-service-worker--pwa)
13. [References](#-references)

---

## Online / Offline Detection

Detect network status changes using `navigator.onLine`.

```js
window.addEventListener("online", () => console.log("Online"));
window.addEventListener("offline", () => console.log("Offline"));

console.log("Current Status:", navigator.onLine ? "Online" : "Offline");
```

[MDN: Navigator.onLine](https://developer.mozilla.org/en-US/docs/Web/API/NavigatorOnLine/onLine)

---

## Geolocation API

Get the user’s location (requires HTTPS + permission).

```js
navigator.geolocation.getCurrentPosition(
  pos => console.log(pos.coords.latitude, pos.coords.longitude),
  err => console.error(err),
  { enableHighAccuracy: true }
);
```

 [MDN: Geolocation API](https://developer.mozilla.org/en-US/docs/Web/API/Geolocation_API)

---

##  Notifications API

Request permission and send browser notifications.

```js
if ("Notification" in window) {
  Notification.requestPermission().then(permission => {
    if (permission === "granted") {
      new Notification("Hello!", { body: "This is a test notification " });
    }
  });
}
```

 [MDN: Notifications API](https://developer.mozilla.org/en-US/docs/Web/API/Notifications_API)

---

##  Clipboard API

Read/write text to clipboard.

```js
// Write
navigator.clipboard.writeText("Hello World").then(() => {
  console.log("Copied to clipboard!");
});

// Read
navigator.clipboard.readText().then(text => {
  console.log("Clipboard content:", text);
});
```

 [MDN: Clipboard API](https://developer.mozilla.org/en-US/docs/Web/API/Clipboard_API)

---

## Share API

Use native device sharing (mobile only).

```js
if (navigator.share) {
  navigator.share({
    title: "Navigator API Guide",
    text: "Check out this amazing repo!",
    url: window.location.href
  })
  .then(() => console.log("Shared successfully!"))
  .catch(err => console.error(err));
}
```

 [MDN: Web Share API](https://developer.mozilla.org/en-US/docs/Web/API/Navigator/share)

---

##  Device & OS Info

Check browser, platform, and user agent.

```js
console.log("User Agent:", navigator.userAgent);
console.log("Platform:", navigator.platform);
console.log("Language:", navigator.language);
```

 [MDN: Navigator](https://developer.mozilla.org/en-US/docs/Web/API/Navigator)

---

##  Battery API

Battery level and charging status.

```js
navigator.getBattery().then(battery => {
  console.log("Battery Level:", battery.level * 100 + "%");
  console.log("Charging:", battery.charging);
});
```

 [MDN: Battery Status API](https://developer.mozilla.org/en-US/docs/Web/API/Battery_Status_API)

---

##  Media Devices

Access camera and microphone.

```js
navigator.mediaDevices.getUserMedia({ video: true, audio: true })
  .then(stream => {
    document.querySelector("video").srcObject = stream;
  })
  .catch(err => console.error("Error:", err));
```

 [MDN: MediaDevices.getUserMedia()](https://developer.mozilla.org/en-US/docs/Web/API/MediaDevices/getUserMedia)

---

##  Vibration API

Trigger device vibrations (mobile only).

```js
navigator.vibrate([200, 100, 200]); // Vibrate -> pause -> vibrate
```

 [MDN: Vibration API](https://developer.mozilla.org/en-US/docs/Web/API/Vibration_API)

---

##  Device Memory & CPU

Estimate device memory and logical CPU cores.

```js
console.log("Memory:", navigator.deviceMemory, "GB");
console.log("Cores:", navigator.hardwareConcurrency);
```

 [MDN: Navigator.deviceMemory](https://developer.mozilla.org/en-US/docs/Web/API/Navigator/deviceMemory)

---

##  Device Orientation & Motion

Listen to accelerometer & gyroscope events.

```js
window.addEventListener("deviceorientation", e => {
  console.log("Alpha:", e.alpha, "Beta:", e.beta, "Gamma:", e.gamma);
});

window.addEventListener("devicemotion", e => {
  console.log("Acceleration:", e.acceleration);
});
```

 [MDN: DeviceOrientation API](https://developer.mozilla.org/en-US/docs/Web/API/Device_orientation_events)

---

##  Service Worker / PWA

Check service worker availability.

```js
if ("serviceWorker" in navigator) {
  navigator.serviceWorker.register("/sw.js").then(reg => {
    console.log("Service Worker registered:", reg.scope);
  });
}
```

 [MDN: Service Workers](https://developer.mozilla.org/en-US/docs/Web/API/Service_Worker_API)

---

##  References

* [MDN Web API Reference](https://developer.mozilla.org/en-US/docs/Web/API)
* [Navigator API](https://developer.mozilla.org/en-US/docs/Web/API/Navigator)
* [Web.dev - Capabilities](https://web.dev/fugu-status/)

