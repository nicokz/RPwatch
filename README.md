# ⌚ RPwatch

### **The MicroPython Wearable OS.**
**High-performance firmware for the RP2040 hardware ecosystem.** Optimized for the GC9A01 circular display and QMI8658 inertial sensors, RPwatch is a framework for building responsive, low-power wearable interfaces.

![RP2040 Watch Display](./docs/img/rp2040-w-color_bg.jpg)

---

## 🚀 Core Capabilities

| Feature | Description |
| :--- | :--- |
| **Viper-Engine Rendering** | Uses `@micropython.viper` and direct `ptr8` memory pointers for near-instant sprite blitting. |
| **No-FrameBuffer Alpha** | Custom `setPixel` logic allows for opacity and blending without the memory overhead of the standard FrameBuffer. |
| **Hybrid Tasking** | Supports IRQ-based button handling and `uasyncio` for non-blocking sensor/UI updates. |
| **Precision Typography** | Specialized `Fonts.py` library with safe-zone calculations for circular displays. |

---

## ⚙️ Hardware Integration

* **Display**: GC9A01 1.28-inch TFT (240x240 Circular).
* **IMU**: QMI8658 6-Axis Gyro/Accelerometer for gesture and motion-based UI.
* **Power Management**: Integrated battery voltage monitoring and deep-sleep RTC synchronization.

---

## 🛠️ The "Viper" Performance Edge
RPwatch bypasses the standard Python bottleneck for graphics. By using direct memory manipulation, the **Sprite System** handles background restoration and transparency at machine-code speeds:

1. **`backup()`**: Snapshots the screen area behind the sprite.
2. **`draw()`**: Injects RGBA565 data directly into the display buffer via pointers.
3. **`restore()`**: Reinstates the original pixels to prevent "ghosting" during motion.

```python
@micropython.viper
def draw(self):
    # Direct ptr8 access to hardware registers
    # Machine-speed pixel injection
```

## 📂 Project Structure
* **`/watch`**: Core OS libraries (`RP2040watch.py`, `fonts.py`).
* **`/examples`**: Real-world implementations (Gyro-sprites, Sleep modes).
* **`/utils`**: Toolchain for converting standard assets to `RGB565` and `RGBA565`.
* **`/fonts`**: Binary font data optimized for fast lookup.

---

## 🛡️ Programming Strategy

> **Real-Time vs. Efficiency**
> RPwatch supports three distinct input models: **Polling** (for low-latency gaming), **Asyncio** (for multitasking UI), and **IRQ** (for ultra-low power consumption).

---

---

## 🚀 Quick Start

### 1. Requirements
* **MicroPython Firmware**: [V1.22+](https://micropython.org/download/RPI_PICO_W/) (Standard RP2040/Pico W build).
* **Hardware**: RP2040 with GC9A01 Circular Display & QMI8658 IMU.
* **Toolchain**: Install `mpremote` for fast deployment:
  ```bash
  pip install mpremote
  ```

### 2. Installation & Deployment
Connect your watch via USB and run the following commands to sync the local project folders to your device:

```bash
# Clone the repository
git clone [https://github.com/nicokz/RPwatch.git](https://github.com/nicokz/RPwatch.git)
cd RPwatch

# Deploy core folders and main entry point
mpremote fs cp -r watch/ :
mpremote fs cp -r fonts/ :
mpremote fs cp -r images/ :
mpremote fs cp main.py :

# Soft-reset to start the app
mpremote reset
```

## 🏁 The Bottom Line
RPwatch isn't just a collection of scripts; it's a specialized rendering engine for the RP2040. It proves that MicroPython can be used for high-speed, visually rich wearable tech when you work close to the silicon.

**OPTIMIZE THE WRIST. CONTROL THE DATA.**
