# CDM config file
This repository contains the config and settings file for the CDM plugin, and supports updating data without updating the sector.

## How to CDM
### How do CDM works
CDM do calculation on the pilot submitted Target Off-block Time TOBT (or EOBT), their estimated taxi time to calculate runway load, and gives slot (TSAT or Target Start-up Approval Time) to pilot based on runway defined capacity to avoid waiting or a long queue on the taxiway. CDM can also take flow control measures and pilot CTOT into account during large event.

### Abbreviation
EOBT - Estimated off-block time, the expected off-block time pilot files in their flight plan (or Simbrief departure time)
TOBT - Target off-block time, the target off-block time submitted by the pilot in our ADGS system, which they will ready for pushback (or EOBT when not available)
ASRT - Actual start-up request time, the time that the pilot requests for pushback. Activating will lock the slot (TSAT) for the pilot.
TSAT - Target start-up approval time, the time calculated for the traffic to push. Valid pushback window will be TSAT -5 to TSAT +5 minutes.
TTG - Time to gain, time to go for the TSAT window. -8 means 8 minutes to go for TSAT -5 minutes.
TTOT - Target Take Off Time, the calculated take off slot for the pilot.
CTOT - Calculated Take Off Time, the given take off slot for the pilot, usually during large event, e.g. CTL CTP or when near airspace or airport is overloaded and detected by the system to reduce load.

### Set up:
1. Go to the Departure list, click "F"
2. For CDC, Select TSAT, TTG, ASRT
   For GMC, Select TTG (or TSAT if preferred)
3. For the controller covering CDC (or Hong Kong Departure flow manager VHHH_P_GND when approved by vACC directors), use command ".cdm master VHHH" to activate the system

### Procedure:
### CDC
1. Issue clearance as normal
2. Keep the aircraft on your frequency and ask them to report ready for push and start / Or, when VHHH_P_GND is available, ask the pilot to report ready on VHHH_P_GND frequency
3. When traffic has reported ready, check "TTG" column on the departure list. 
   -If green colour (-5 to +5) is shown, left-click ASRT, then send the traffic to GMC (if you see -6 on TTG, it is ok to just left click ASRT and send the pilot to GMC as it is just 1 minutes to go for his slot)
   -If yellow colour (<-5) is shown, right-click ASRT, then send the traffic to GMC
   -If no value is shown or "~", right-click ASRT, then send the traffic to GMC

### GMC
1. When the pilot requests push and start, check if "TTG" or "TSAT" is green in colour before issuing pushback clearance; If not, ask the pilot to stand by (or provide expected clearance time, TSAT)
   If you see traffic with TTG >+5, depending on the situation, you may still issue pushback clearance if it is not > +10)
   If TTG is >+10, ask the pilot to refile a valid TOBT | [Callsign], your TOBT is invalid, update it and report ready again. or .tobt alias

Link to the ADGS system (where they can file their TOBT and check their slot is included in all CDC and GMC controller info

### For controller change of the CDM master position
1. The previous "master" controller use command ".cdm slave VHHH"
2. Then, the relief controller use command ".cdm master VHHH"

### Runway Direction Change
In case of runway direction change
1. AMC should decide the last departure aircraft by checking the TTOT, available in Departure list (can be selected by clicking "F" on departure list)
2. AMC can then process and pass the first available departure time for the new runway to CDC
3. CDC use command ".cdm departuredelay VHHH/(new departure runway) (first departure time in 4-digit UTC)" e.g. .cdm departuredelay VHHH/07C 0700 (you have to do it separately if there is 2 departure runways)
Use time "9999" to cancel the previous input command in case of any mistakes

### Ground Stop
In case of ground stop is required
1. CDC use command ".cdm startupdelay VHHH/(affected runway) (4-digit time in UTC) (same format as runway director change delay command)
