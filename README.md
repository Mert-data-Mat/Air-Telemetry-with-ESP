A low-power, wireless sensor system designed to monitor outdoor air quality and environmental data, displaying the readings on an indoor unit.

🎯 Project Overview

I aim to build a discreet "sentinel" for my balcony that quietly monitors the outside air for VOC (Volatile Organic Compounds), eCO₂, and a general Air Quality Index (AQI). The data is wirelessly transmitted to a display unit inside the house, providing at-a-glance information to answer the eternal question of my girlfriend: "What's the weather like outside?"

This project focuses on a robust, battery-friendly architecture from the ground up.


Why This Setup Works ?

Optimized Power Strategy: The key to long battery life. The ESP microcontroller on the balcony node sleeps deeply between readings. Crucially, the ENS160 sensor's sleep mode is also leveraged to minimize average current draw.

![ENS160 Sleep Mode Diagram](images/sleep_sheet.png)
*Diagram from the ENS160 datasheet illustrating the low-power sleep mode*

🤔 Design Philosophy & Trade-Offs

A significant part of this project is planning the power management strategy before writing a single line of code to avoid common pitfalls.

The ENS160 was chosen for its rich data (VOC, eCO₂, AQI), but it introduces complexity with its required warm-up time and baseline tracking.

Power Strategy Analysis

Two primary power management options were considered:

**Option A**: Full Power-Cycling: Turning the sensor and ESP completely off between readings.

Con: High inrush current during wake-up and the ENS160 requires a significant warm-up time before accurate readings, leading to high peak power drain.

-----------------------
**Option B**: Strategic Sleep (Chosen Approach): Keeping the ENS160 sensor powered continuously to maintain its baseline and warm state, while putting the ESP into deep sleep.

Pro: The ENS160 can be placed into a low-power mode itself, resulting in a much lower average current and more stable readings immediately upon wake-up.


📋 Immediate Next Steps:

**Finalize Operational Parameters**: Decide on the measurement interval (e.g., every 5 or 10 minutes).

**Component Selection** : Finalize ESP board models, battery size, voltage regulator, and weather-proofing for the balcony enclosure.

**Synchronization**: I am still not sure about how to make this best for the battery life. Trying not to lost even 10 μA without losing my mind.

**Power Budget Analysis**: Calculate estimated battery life based on the chosen sleep intervals and measured current consumption.

**Protocol Definition**: Define the structure of the ESP-NOW data packet (to include TVOC, eCO₂, AQI, timestamp, sensor status flags).

🚀 Why This Architecture Matters

By carefully planning the sleep/wake cycles and wireless communication strategy from the start, this project aims to be:

Power-Efficient: Maximizing battery life on the balcony unit.
Reliable: Ensuring data is successfully transmitted and displayed.
Maintainable: A clean separation of concerns between the sensor and display nodes.
