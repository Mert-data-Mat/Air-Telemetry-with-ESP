What I building

I want a little weather / air-quality sentinel on the balcony. This node quietly monitors the outside air (VOC, eCO₂, general air-quality) plus environmental data. Then, inside the house, another ESP shows the latest readings on a screen — so my girlfirend stop asking questions about the weather outside.

Why this setup

The balcony unit runs off battery, doesn’t need to be wired to the house mains.

Wireless link via ESP-NOW means no WiFi setup headaches, less overhead.

The display inside is just there to show the data — low-interaction.

We’re aiming for low-power behaviour, so the balcony node only wakes up when needed, sends data, then goes back to sleep — extending battery life. Also I found out from the datasheets ENS also has a Deep Sleep mode like ESP.

How I got here (our thinking)
- Sensor choice: Using the ENS160 because it gives VOC, eCO₂ equivalent, air-quality index — richer than just “temperature/humidity”. But it’s not trivial: it has warm-up and baseline behaviour. 
- Power strategy: I looked at two options:

    1. Power everything only when measuring (sensor + ESP awake for a few minutes) → big battery drain during the “awake” window. Also ENS needs the "warm-up" according the datasheet for "some" time :)

    2. Keep the sensor powered continuously (so baseline/warm-up is handled), and only the ESP wakes at intervals → lower overall average current.

- I found the second option gives better longevity for a periodic measurement scenario.	
- Synchronization & wireless: The indoor display ESP must receive the data when the balcony unit transmits. So we need a schedule — both nodes know “now we wake/send” so links don’t get lost. 
- Display behaviour: The indoor node should show the last valid reading. Even while sleeping, the screen should remain showing that reading (assuming its power/back-light is managed).


What’s next in my mind ?

Decide interval: how often do we want readings? Every 5 mins? 10 mins?

Pin/board selection: which ESP board, battery size, regulator, sensor mounting on balcony (weather-proofing?).

Indoor display: choose screen type, placement, ensure wireless link coverage from balcony.

Power budget: estimate battery life given sensor always-on + ESP wake interval + display node behaviour.

Define data packet: what values we send (TVOC, eCO₂, AQI, timestamp, status).

Sketch schedule: Sensor node wakes → transmits → goes to sleep; Display node either wakes synchronously or stays ready for some time after expected send.

Why this matters ?

By planning the architecture before writing code, I try to avoid chasing after “why isn’t the battery lasting” or “why is the reading garbage after waking”. I already did this project halfly at the ENS160 repository I have. I wanted to divide it to different repo with more advanced and complete finished project.
