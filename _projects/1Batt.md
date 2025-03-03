---
layout: page
title: Batt
description: Standardize battery data
img: assets/img/12.jpg
importance: 1
category: work
related_publications: false
---

Batt is a Python project designed to clean, standardize, and efficiently store battery data in a database. It includes a user-friendly web interface for easy access and management. The project ensures high data integrity through thorough testing.

# Key Features
Supports .mat, .csv, and .json file formats
Cleans and processes raw battery data
Standardizes data formats for consistency
Efficiently loads data into a database
Web interface for seamless user interaction

# Challenges & Solutions
1. Handling Charging/Discharging Transitions
Used `shift()` to compare the current and previous SoC values, but cannot recognize idle states.
Addressed momentary idle states by ignoring short idle durations and tracking state transitions efficiently.
	> Updating several previous rows marked as idle when a transition occurs, like idle to charging/discharging and idle state are short. (Checking current state is charging/discharging)
	> Resetting `idle_count` when battery starts charging or discharging
	> Increasing `idle_count`when the battery remains idle
	
2. Performance Optimization
Initially used `iterrows()` but found it slow for large datasets.
Switched to df.apply(lambda row: function(), axis=1), significantly improving speed.
**Key limitation of lambda**:
    Unlike `iterrows()`, lambda cannot modify previously processed rows in the DataFrame.
    To work around this, stored necessary updates in a dictionary and applied them in a separate step.

3. Cycle Counting
description: Increased the cycle number with every charging/discharging transition.
Initially planned to increase the cycle number during state recognition. However, due to idle states, previous cycle numbers need adjustment. Since `lambda` function cannot update past rows, I handled it after setting all state. Using `shift()` to find the transitions and define cycles.