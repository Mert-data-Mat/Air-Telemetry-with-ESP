# 🌳 Air Telemetry: Giving My Balcony a Mind

### *A low-power, wireless sensor system to monitor outdoor air quality, humidity and temperature*

## 🎯 The Big Idea: Answering the Eternal Question

We've all been there: "What's the weather like outside?" But I wanted to answer a much deeper question: **"What's the *air* like outside?"**

This project, "Air Telemetry," is my mission to build a discreet, battery-powered spy for my balcony—a "Sentinel"—that quietly monitors the outside air and beams the data to a friendly display inside the house.

I'm aiming for something that doesn't just work, but keeps working for **months on a single charge.** No one wants to change batteries every week!

### What We're Measuring:
* **The Invisible Stuff:** VOCs (Volatile Organic Compounds) & eCO₂
* **The Big Picture:** General Air Quality Index (AQI)
* **The Comfort Check:** Temperature and Humidity 

---

## 🤯 My Obsession: The Power Battle Plan

The real challenge here isn't just *getting* the data; it's making the battery last forever. I decided to tackle power management **before** writing a single line of code, because, frankly, power pitfalls ruin projects.

**My goal:** Beat the vampire drain. I'm fighting to keep the average current draw as low as possible—we're talking microamperes!

### 🛌 Why We Don't Just Turn Everything Off

I had two choices for the power strategy, but one was a clear winner.

| Option | Strategy | Why I Rejected It (or Chose It!) |
| :--- | :--- | :--- |
| **A: Full Power-Cycling** | Turn the sensor AND the ESP completely OFF. | The specialized ENS160 sensor *hates* this. It needs a long time to warm up and re-establish its baseline, wasting a ton of power in a high-current "peak" phase every time. |
| **B: Strategic Sleep (The Winner!)** | Keep the sensor powered (but in its *own* sleep mode), and put the ESP into Deep Sleep. | **It's genius!** The sensor maintains its internal warm-up and calibration state. When the ESP wakes, the data is instantly ready, and we go back to sleep fast. Maximum battery life achieved! |


![ENS160 Sleep Mode Diagram](images/sleep_sheet.png)
*Diagram from the ENS160 datasheet illustrating the low-power sleep mode*
---

## 🚀 The Tech Stack for Speed & Efficiency

* **The Sender (Sentinel):** A stripped-down ESP (TBD model) running super-efficient **ESP-NOW**. We send the data packet lightning-fast—no slow, power-hungry Wi-Fi connection attempts!
* **The Sensor:** The **ENS160** for its rich data set (VOC/eCO₂).
* **The Receiver (Indoor):** An ESP-based display unit. (The strategy is still TBD: always listening, or a synchronized-wakeup?)

---

## ✅ What's Next on the Workbench

I'm moving from theory to hardware, but a few key decisions need locking down.

* [x] **Pick the "Time Gate":** How often is enough? Decided to request the data every hour.
* [x] **Finalize the Shopping List:** ✅ DONE
* [ ] **Do The Math:** Crunch the **Power Budget Analysis** with real-world measurements to confidently say: "This thing will last $X$ months."
* [ ] **Define the Data Blueprint:** Finalize the exact, tiny data packet we'll send over ESP-NOW (e.g., `AQI`, `TVOC_val`, `eCO2_val`, `Sensor_Status`, `Time`.
