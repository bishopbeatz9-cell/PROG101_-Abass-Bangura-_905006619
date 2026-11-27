            OVERVIEW
## Project Title: 

Freetown Urban Flood Warning & Response Network (FUFWARN)  an automated software logic model designed to monitor flash flood risks in high-risk Freetown communities (such as Kroo Bay and Susan's Bay).
Core Technical Content: Features a complete step-by-step algorithm, professional pseudocode, a visual flowchart diagram, variable and constant tables, data type applications, decision structures (using IF, ELSE, and Boolean operators like AND, OR, NOT), repetition structures (WHILE, FOR, REPEAT...UNTIL), modular functions, and test scenarios.
Community & Academic Framework: Examines the rationale behind Freetown's seasonal flooding, Open Source Software (OSS) advantages, Digital Public Goods (DPGs) alignment, and UN Sustainable Development Goal 11 (Sustainable Cities and Communities).
Formatting: Fully compiled into a polished academic Word Document complete with an official cover page, table of contents, custom tables, and APA 7th edition references.

 ##Official Goal Name: SDG 11   
 
 3Sustainable Cities and Communities (making cities inclusive, safe, resilient, and sustainable).
Direct Target Alignment (Target 11.5): By providing automated, real-time early warnings before floodwaters reach dangerous levels, the system helps significantly reduce the number of deaths and economic losses caused by natural disasters, specifically protecting vulnerable populations living in high-risk informal settlements like Kroo Bay and Susan's Bay.
Secondary SDG Alignment (SDG 13 - Climate Action): The system also supports Target 13.1 by strengthening local resilience and adaptive capacity to climate-related hazards and extreme tropical monsoon rainfall events in Sierra Leone.


##  Problem Description: Urban Flash Flooding in Freetown
Geographical & Climatic Context: Freetown, the capital city of Sierra Leone, is tightly sandwiched between the Atlantic Ocean and the steep, mountainous terrain of the Western Area Peninsula. The region experiences an intense tropical monsoon climate, characterized by heavy seasonal rainfall exceeding 3,000 millimeters annually, concentrated primarily between the months of May and November.
The Human & Social Crisis: Uncontrolled urban migration, deforestation of the Western Area Peninsula, and a severe shortage of formal affordable housing have forced thousands of urban residents to settle in high-risk, low-lying coastal or flood-prone informal settlements such as Kroo Bay, Susan's Bay, Culvert, and Dwarzark. During heavy downpours, water cascades rapidly down Freetown's steep slopes, meeting clogged drainage canals (like the Samba Gutter) choked with uncollected municipal solid waste. This creates sudden flash floods that inundate homes within 15 to 30 minutes, resulting in tragic loss of life, displacement, and destruction of personal property.
The Failure of Traditional Warning Methods: Historically, early warnings relied entirely on manual observation, informal word-of-mouth reports, local radio broadcasts, or scattered WhatsApp messages. Because these manual communication chains are slow and unreliable especially during heavy overnight downpours when residents are asleep communities receive warnings far too late, leaving families trapped by rising waters before they can react.



## Logical Solution:
The Freetown Urban Flood Warning & Response Network (FUFWARN)
System Concept & Architecture: FUFWARN is a modular, algorithmic software logic system designed to bridge the gap between real-time environmental data acquisition and rapid community emergency response. It operates continuously without human intervention to mitigate the dangers of flash flooding.
Core Computational & Logical Workflow:
Continuous Telemetry Ingestion (WHILE Loop): The system operates within an infinite execution loop, continuously reading live environmental data feeds specifically the sensor ID (sensorID), current water level height (currentWaterLevel), and rainfall intensity (rainRate) every 60 seconds.
Data Sanitization & Error Handling: Before processing, a single-alternative decision check (IF currentWaterLevel < 0.0 OR currentWaterLevel > 10.0) filters out corrupted data or physical sensor malfunctions, suppressing false alarms and logging a SENSOR_FAULT event.
Rate-of-Change & Compound Boolean Evaluation: The system calculates the rate of water level rise (waterLevelDelta = currentWaterLevel - previousWaterLevel) and evaluates multi-variable compound Boolean conditions (AND, OR). For example, a RED emergency threat level is triggered if the water level breaches the critical height threshold (2.50" meters" ), OR if a moderate water level (1.50" meters" ) is combined with a rapid rise rate (0.15" m/min" ) or heavy rainfall (50.0" mm/hr" ).
Tiered Emergency Action Execution:
	If a RED Threat is Verified: The system automatically sets the hardware relay flag (sirenActive = TRUE) to trigger loud physical community alarm sirens, activates a count-controlled FOR loop to broadcast automated emergency evacuation SMS texts to all registered household mobile numbers (subscriberList), and transmits an automated alert API payload to the National Disaster Management Agency (NDMA) command center.
	If a YELLOW Warning is Verified: It suppresses sirens but dispatches cautionary SMS advisories warning residents to stay alert and secure valuable possessions.
	If GREEN Status is Maintained: It logs normal operations and quietly continues its monitoring cycle.


Pseudocode
// ============================================================================
// MODULE: FUFWARN Main Emergency Processing Engine
// AUTHOR: [INSERT MY NAME] (ID: [INSERT MY ID])
// COURSE: PROG101 - Principles of Programming Logic and Design
// INSTITUTION: Limkokwing University of Creative Technology, Sierra Leone
// ============================================================================

START

    // Declare Global System Constants
    CONSTANT REAL MAX_SAFE_WATER_LEVEL = 1.50
    CONSTANT REAL CRITICAL_WATER_LEVEL = 2.50
    CONSTANT REAL CRITICAL_RAIN_RATE = 50.0
    CONSTANT REAL RAPID_RISE_RATE = 0.15
    CONSTANT STRING SYSTEM_NAME = "FUFWARN v1.0"

    // Declare System Variables
    STRING sensorID
    REAL currentWaterLevel
    REAL previousWaterLevel
    REAL waterLevelDelta
    REAL rainRate
    STRING threatLevel
    BOOLEAN sirenActive
    BOOLEAN systemActive
    INTEGER contactIndex
    ARRAY STRING subscriberList[500]

    // Initial Setup
    previousWaterLevel = 0.0
    threatLevel = "GREEN"
    sirenActive = FALSE
    systemActive = TRUE

    // Main Operational Processing Loop
    WHILE systemActive == TRUE DO
        
        // Data Acquisition Section
        INPUT sensorID
        INPUT currentWaterLevel
        INPUT rainRate

        // Data Sanitization check
        IF currentWaterLevel < 0.0 OR currentWaterLevel > 10.0 THEN
            OUTPUT "ERROR: Invalid sensor telemetry out of range."
            threatLevel = "ERROR"
            CALL Module_LogSystemEvent(sensorID, currentWaterLevel, rainRate, "SENSOR_FAULT")
        ELSE
            // Calculate rate of change
            waterLevelDelta = currentWaterLevel - previousWaterLevel

            // Decision Logic for Threat Level Determination
            IF (currentWaterLevel >= CRITICAL_WATER_LEVEL) OR 
               (currentWaterLevel >= MAX_SAFE_WATER_LEVEL AND waterLevelDelta >= RAPID_RISE_RATE) OR 
               (currentWaterLevel >= MAX_SAFE_WATER_LEVEL AND rainRate >= CRITICAL_RAIN_RATE) THEN
                
                threatLevel = "RED"
                sirenActive = TRUE

            ELSE IF (currentWaterLevel >= MAX_SAFE_WATER_LEVEL) OR (rainRate >= 30.0) THEN
                
                threatLevel = "YELLOW"
                sirenActive = FALSE

            ELSE
                
                threatLevel = "GREEN"
                sirenActive = FALSE

            END IF

            // Execute Actions Based on Determined Threat Level
            IF threatLevel == "RED" THEN
                OUTPUT "ALERT: Executing Red Level Evacuation Protocols."
                CALL Module_ActivateSiren(TRUE)
                
                // Iterative SMS Alert Dispatch Loop
                FOR contactIndex = 0 TO LENGTH(subscriberList) - 1 STEP 1
                    CALL Module_SendSMS(subscriberList[contactIndex], "RED ALERT: Severe flooding imminent in Kroo Bay. Move to higher ground immediately!")
                END FOR

                CALL Module_NotifyNDMA(sensorID, currentWaterLevel)

            ELSE IF threatLevel == "YELLOW" THEN
                OUTPUT "WARNING: Executing Yellow Level Precaution Protocols."
                CALL Module_ActivateSiren(FALSE)

                FOR contactIndex = 0 TO LENGTH(subscriberList) - 1 STEP 1
                    CALL Module_SendSMS(subscriberList[contactIndex], "YELLOW WARNING: Water level rising near Samba Gutter. Stay alert and secure belongings.")
                END FOR

            ELSE
                OUTPUT "SYSTEM STATUS: Normal operating conditions."
                CALL Module_ActivateSiren(FALSE)
            END IF

            // Record system audit history
            CALL Module_LogSystemEvent(sensorID, currentWaterLevel, rainRate, threatLevel)

            // Update state variable for next execution iteration
            previousWaterLevel = currentWaterLevel

        END IF

        // Wait before next sensor reading cycle
        CALL Module_Sleep(60)

    


<img width="1219" height="1350" alt="FLOWCHART_ABASS_BANGURA" src="https://github.com/user-attachments/assets/538f52dc-eff9-41a9-93e8-abaf99d4f2eb" />






