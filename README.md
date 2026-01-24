# CDM Config File
This repository contains the configuration and settings files for the **CDM plugin**, and supports updating data without updating the sector.

---

## How CDM Works
CDM calculates the runway load using the pilot-submitted **Target Off-block Time (TOBT)** (or **EOBT**) and their estimated taxi time. It then issues a slot (**TSAT** or **Target Start-up Approval Time**) to the pilot based on the runway's defined capacity to avoid waiting or a long queue on the taxiway. CDM can also take flow control measures and pilot **CTOT** into account during large events.

---

### Abbreviations
* **EOBT - Estimated Off-Block Time**: The expected off-block time the pilot files in their flight plan (or Simbrief departure time).
* **TOBT - Target Off-Block Time**: The target off-block time submitted by the pilot in our **ADGS system**, indicating when they will be ready for pushback (or **EOBT** when **TOBT** is not available).
* **ASRT - Actual Start-up Request Time**: The time that the pilot requests pushback. **Activating ASRT** will lock the slot (**TSAT**) for the pilot.
* **TSAT - Target Start-up Approval Time**: The calculated time for the traffic to push back. The valid pushback window will be **TSAT -5** to **TSAT +5 minutes**.
* **SLOT - Time until offblock slot**: Time remaining until the **TSAT** window. **-8** means 8 minutes until **TSAT**.
* **TTOT - Target Take Off Time**: The calculated take-off slot for the pilot.
* **CTOT - Calculated Take Off Time**: The assigned take-off slot for the pilot, usually during large events (e.g., **CTL** or **CTP**) or when nearby airspace or an airport is overloaded and detected by the system to reduce load.

---

### Set Up:
1.  Go to the **Departure list** and click "**F**."
2.  Select the required columns:
    * For **CDC**, select **TSAT, SLOT, ASRT**.
3.  The controller covering **CDC** uses the command `.cdm master VHHH` to activate the system or left-click VHHH on the CDM panel on Euroscope (Click once only and allow it to load for a while).

---

### Procedure:
### CDC
1.  Issue clearance as normal.
2.  Keep the aircraft on your frequency and ask them to **report ready for push and start**.
3.  When the traffic has reported ready, check the "**SLOT**" column on the departure list.
    * If the **green colour** ($-5$ to $+5$) is shown, **left-click ASRT**, then send the traffic to **GMC**. (If you see $-6$ on **SLOT**, it is okay to just left-click **ASRT** and send the pilot to **GMC** as it is only 1 minute until their slot.)
    * If the **yellow colour** ($<-5$) is shown, **right-click ASRT**, then send the traffic to **GMC** when it becomes green (In TSAT window), you may use the phraseology `(Callsign) expect push at time (minutes, e.g. 45 for XX:45z), call you back`
    * If no value is shown or "**~**" is displayed, **right-click ASRT**, then send the traffic to **GMC** when it becomes green (In TSAT window), you may use the phraseology `(Callsign) expect push at time (minutes, e.g. 45 for XX:45z), call you back`
-   You are strongly advised to issue clearance via PDC to maintain radio capacity, use the phraseology `(Callsign) Hong Kong Delivery ATC Clearance will be sent via data-link`
-   You may use STUP Ground status to indicate traffic that has been sent to the GMC freq.
--- 

### For Controller Change of the CDM Master Position
1.  The previous "**master**" controller uses the command `.cdm slave VHHH`.
2.  Then, the relief controller uses the command `.cdm master VHHH`.

---

### Runway Direction Change
In case of a runway direction change:
1.  **AMC** should decide the last departure aircraft by checking the **TTOT**, which is available in the **Departure list** (can be selected by clicking "**F**" on the departure list).
2.  **AMC** can then process and pass the first available departure time for the new runway to **CDC**.
3.  **CDC** uses the command `.cdm departuredelay VHHH/(new departure runway) (first departure time in 4-digit UTC)`. E.g., `.cdm departuredelay VHHH/07C 0700`. (You have to do it separately if there are 2 departure runways.)
    > Use time "**9999**" to cancel the previous input command in case of any mistakes.

---

### Ground Stop
In case a ground stop is required:
- **CDC** uses the command `.cdm startupdelay VHHH/(affected runway) (4-digit time in UTC)`. (Use the same format as the runway direction change delay command.)
OR
- **FMP** use Ground Stop flow measure in website dashboard
