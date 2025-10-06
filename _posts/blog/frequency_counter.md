---
layout: post
title: Locked or Lost? Trust, But Verify with a Frequency Counter
modified:
categories: blog
excerpt:
tags: [Introduction]
date: 2025-10-06T20:41:50-04:00
---

## Introduction

When it comes to board bring-up and verification, the frequency counter isn’t just helpful—it’s essential. Whether you're validating clock trees, debugging timing issues, or confirming signal integrity, this tool is your go-to for reliable frequency measurements.
One common scenario is working with PLLs (Phase-Locked Loops). You might assume that once the PLL lock signal is asserted, everything is good to go. After all, a locked PLL should mean the output frequency is correct, right? Well—usually. But in high-stakes designs, “usually” isn’t good enough.
How do you know the reference clock (refclk) feeding the PLL is correct? And how do you confirm the PLL is actually generating the expected output frequency? That’s where the frequency counter comes in. It gives you confidence that your system is behaving exactly as intended, even when everything looks fine.
Because in hardware validation, it’s never too much to double-check. Trust the lock signal—but verify it.

## Simulation waveform
Found the frequency checker from the F-tile DPHY Example Design to be extremely helpful. Extracted the codes and simulated it. Here are the results: 

Overview
--------
`frequency_checker.v` is a Verilog module for measuring the frequency of an input clock using a reference clock. It outputs the measured frequency in MHz.

Simulation Instructions
----------------------
1. Make sure you have the following files in your simulation directory:
   - frequency_checker.v
   - frequency_checker_tb.v (testbench)
   - dcfifo_alt_xcvr_resync_std.sv (synchronizer module)
   - altera_std_synchronizer_nocut.v (if required)
   - dcfifo_altera_std_synchronizer_nocut.v (if required)
   - run_frequency_checker.do (ModelSim/QuestaSim script)

2. Open ModelSim/QuestaSim and change to the directory containing these files.

3. Run the simulation script:
   do run_frequency_checker.do

4. The script will:
   - Compile all required files
   - Run the testbench
   - Open the waveform viewer and add all signals
   - Display the measured frequency in the transcript
   
Notes: WLF provided for simulated waveform   

How to Use frequency_checker.v in Your Design
---------------------------------------------
- Instantiate the module in your design:

  frequency_checker #(
    .COUNT(<ref_clk_freq_in_MHz>) 	//if you connect to 100Mhz then write as .COUNT(16'd100)
  ) u_freq_checker (
    .ref_clock(ref_clk), 			//this should match with .COUNT frequency e.g. connect to .ref_clock(clk100m), 
    .in_clock(clk_to_measure), 		// connect your clock DUT
    .reset_freq_count(reset), 		// optional reset
    .clkout_freq(measured_freq), 	// clock DUT results 
    .master_freq(ref_count), 		//should show as decimal 100 if using 100Mhz
    .freq_measured(done) 			// optional not rea
  );

- Set the COUNT parameter to the reference clock frequency in MHz (e.g., 156 for 156 MHz).
- Apply the reference clock and the clock to be measured.
- Assert `reset_freq_count` to start a new measurement.
- When `freq_measured` goes high, read `clkout_freq` for the measured frequency.

Notes
-----
- The module uses synchronizer blocks for safe clock domain crossing.
- For hardware use, connect the outputs to registers or status ports as needed.
