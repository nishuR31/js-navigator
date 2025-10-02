

# Browser Navigator Guide

[![Made with JavaScript](https://img.shields.io/badge/Made%20with%20JavaScript-yellow?logo=javascript&logoColor=black)](https://developer.mozilla.org/en-US/docs/Web/JavaScript)
[![Browser Support](https://img.shields.io/badge/Browser%20APIs-blue?logo=googlechrome&logoColor=black)](https://developer.mozilla.org/en-US/docs/Web/API)
[![MDN Docs](https://img.shields.io/badge/MDN-orange?logo=mozilla&logoColor=black)](https://developer.mozilla.org/)
[![Contributions Welcome](https://img.shields.io/badge/Contributions%20Welcome-brightgreen&logoColor=black)](https://github.com/)

A **complete reference of `navigator` APIs in JavaScript** for modern browsers.
This guide demonstrates **Geolocation, Clipboard, Online/Offline, Device Info, Notifications, Sharing, Media, Sensors**, and more — with **full example code, configuration options, error handling, and best practices**.

---

## Table of Contents

1. [Online / Offline Detection](#online--offline-detection)
2. [Geolocation API](#geolocation-api)
3. [Notifications API](#notifications-api)
4. [Clipboard API](#clipboard-api)
5. [Share API](#share-api)
6. [Device & OS Info](#device--os-info)
7. [Battery API](#battery-api)
8. [Media Devices (Camera/Mic)](#media-devices)
9. [Vibration API](#vibration-api)
10. [Device Memory & Hardware Concurrency](#device-memory--cpu)
11. [Orientation & Motion](#device-orientation--motion)
12. [Service Worker / PWA](#service-worker--pwa)
13. [References](#references)

---

### Online / Offline Detection

Detect network status changes with `navigator.onLine`. Listen for events and handle gracefully.

```js
function updateNetworkStatus() {
    console.log(`Network status: ${navigator.onLine ? "Online" : "Offline"}`);
}

window.addEventListener("online", () => {
    console.info("Connection restored.");
    updateNetworkStatus();
});

window.addEventListener("offline", () => {
    console.warn("Connection lost.");
    updateNetworkStatus();
});

updateNetworkStatus();
```

**Best Practices:**

* Inform the user visually when connectivity changes.
* Implement retry logic for failed network requests.

[MDN: Navigator.onLine](https://developer.mozilla.org/en-US/docs/Web/API/NavigatorOnLine/onLine)

---

### Geolocation API

Get user location with precision settings and watch position for real-time updates.

```js
const geoOptions = {
    enableHighAccuracy: true,
    timeout: 10000, // Maximum wait time (ms)
    maximumAge: 30000 // Use cached position if not older than this (ms)
};

function success(position) {
    const { latitude, longitude, accuracy } = position.coords;
    console.log(`Latitude: ${latitude}, Longitude: ${longitude}, Accuracy: ${accuracy} meters`);
}

function error(err) {
    console.error(`Geolocation error (${err.code}): ${err.message}`);
}

if ("geolocation" in navigator) {
    navigator.geolocation.getCurrentPosition(success, error, geoOptions);

    const watchId = navigator.geolocation.watchPosition(success, error, geoOptions);
    // Stop watching after some time
    setTimeout(() => navigator.geolocation.clearWatch(watchId), 60000);
} else {
    console.error("Geolocation is not supported by this browser.");
}
```

[MDN: Geolocation API](https://developer.mozilla.org/en-US/docs/Web/API/Geolocation_API)

---

### Notifications API

Request permission and display advanced notifications.

```js
async function showNotification() {
    if (!("Notification" in window)) {
        console.error("Notifications are not supported.");
        return;
    }

    const permission = await Notification.requestPermission();
    if (permission !== "granted") {
        console.warn("Notification permission denied.");
        return;
    }

    const options = {
        body: "This is a test notification with advanced options.",
        icon: "/assets/icons/notification-icon.png",
        image: "/assets/images/notification-image.png",
        badge: "/assets/icons/badge.png",
        vibrate: [200, 100, 200],
        actions: [
            { action: "open", title: "Open App" },
            { action: "dismiss", title: "Dismiss" }
        ],
        data: { timestamp: Date.now(), customData: "example" }
    };

    new Notification("Navigator API Guide", options);
}

showNotification();
```

[MDN: Notifications API](https://developer.mozilla.org/en-US/docs/Web/API/Notifications_API)

---

### Clipboard API

Read and write clipboard data securely.

```js
async function writeClipboard(text) {
    try {
        await navigator.clipboard.writeText(text);
        console.log("Text copied successfully.");
    } catch (err) {
        console.error("Failed to write to clipboard:", err);
    }
}

async function readClipboard() {
    try {
        const text = await navigator.clipboard.readText();
        console.log("Clipboard content:", text);
    } catch (err) {
        console.error("Failed to read clipboard:", err);
    }
}

writeClipboard("Example text for clipboard");
readClipboard();
```

[MDN: Clipboard API](https://developer.mozilla.org/en-US/docs/Web/API/Clipboard_API)

---

### Share API

Enable native sharing with full metadata.

```js
async function shareContent() {
    if (!navigator.share) {
        console.error("Share API not supported.");
        return;
    }

    try {
        await navigator.share({
            title: "Navigator API Guide",
            text: "Explore all navigator APIs in one guide.",
            url: window.location.href
        });
        console.log("Content shared successfully.");
    } catch (err) {
        console.error("Error sharing content:", err);
    }
}

shareContent();
```

[MDN: Web Share API](https://developer.mozilla.org/en-US/docs/Web/API/Navigator/share)

---

### Device & OS Info

Access detailed environment data.

```js
console.log("User Agent:", navigator.userAgent);
console.log("Platform:", navigator.platform);
console.log("Language:", navigator.language);
console.log("Languages:", navigator.languages);
console.log("Cookies Enabled:", navigator.cookieEnabled);
console.log("Do Not Track:", navigator.doNotTrack);
```

[MDN: Navigator](https://developer.mozilla.org/en-US/docs/Web/API/Navigator)

---

### Battery API

Advanced battery info with event listeners.

```js
async function getBatteryInfo() {
    if (!("getBattery" in navigator)) {
        console.error("Battery API not supported.");
        return;
    }

    const battery = await navigator.getBattery();

    function logBattery() {
        console.log(`Battery Level: ${(battery.level * 100).toFixed(0)}%`);
        console.log(`Charging: ${battery.charging}`);
    }

    battery.addEventListener("levelchange", logBattery);
    battery.addEventListener("chargingchange", logBattery);

    logBattery();
}

getBatteryInfo();
```

[MDN: Battery Status API](https://developer.mozilla.org/en-US/docs/Web/API/Battery_Status_API)

---

### Media Devices

Access camera and microphone with constraints.

```js
async function startMedia() {
    if (!navigator.mediaDevices || !navigator.mediaDevices.getUserMedia) {
        console.error("Media Devices API not supported.");
        return;
    }

    const constraints = {
        audio: { echoCancellation: true, noiseSuppression: true },
        video: {
            width: { ideal: 1280 },
            height: { ideal: 720 },
            facingMode: "user"
        }
    };

    try {
        const stream = await navigator.mediaDevices.getUserMedia(constraints);
        const videoElement = document.querySelector("#cameraFeed");
        if (videoElement) {
            videoElement.srcObject = stream;
            videoElement.play();
        }
    } catch (err) {
        console.error("Error accessing media devices:", err);
    }
}

startMedia();
```

[MDN: MediaDevices.getUserMedia()](https://developer.mozilla.org/en-US/docs/Web/API/MediaDevices/getUserMedia)

---

### Vibration API

Control vibration patterns.

```js
if ("vibrate" in navigator) {
    navigator.vibrate([300, 100, 300, 100, 300]);
    console.log("Vibration triggered.");
} else {
    console.warn("Vibration API not supported.");
}
```

[MDN: Vibration API](https://developer.mozilla.org/en-US/docs/Web/API/Vibration_API)

---

### Device Memory & CPU

Retrieve device specs.

```js
console.log("Device Memory:", navigator.deviceMemory || "Not supported", "GB");
console.log("Hardware Concurrency:", navigator.hardwareConcurrency || "Not supported", "cores");
```

[MDN: Navigator.deviceMemory](https://developer.mozilla.org/en-US/docs/Web/API/Navigator/deviceMemory)

---

### Device Orientation & Motion

Handle motion events with accuracy.

```js
window.addEventListener("deviceorientation", e => {
    console.log(`Alpha: ${e.alpha}, Beta: ${e.beta}, Gamma: ${e.gamma}`);
});

window.addEventListener("devicemotion", e => {
    console.log("Acceleration:", e.acceleration, "Rotation Rate:", e.rotationRate);
});
```

[MDN: DeviceOrientation API](https://developer.mozilla.org/en-US/docs/Web/API/Device_orientation_events)

---

### Service Worker / PWA

Register service worker with fallback handling.

```js
if ("serviceWorker" in navigator) {
    navigator.serviceWorker.register("/sw.js")
    .then(registration => {
        console.log("Service Worker registered:", registration.scope);
    })
    .catch(err => {
        console.error("Service Worker registration failed:", err);
    });
} else {
    console.warn("Service Workers not supported.");
}
```

[MDN: Service Workers](https://developer.mozilla.org/en-US/docs/Web/API/Service_Worker_API)

---

### References

* [MDN Web API Reference](https://developer.mozilla.org/en-US/docs/Web/API)
* [Navigator API](https://developer.mozilla.org/en-US/docs/Web/API/Navigator)
* [Web.dev Capabilities](https://web.dev/fugu-status/)
