# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Overview

This is the Top Coder Challenge repository for reverse-engineering a 60-year-old travel reimbursement system. The goal is to recreate the legacy system's exact behavior (including bugs) by analyzing 1,000 historical input/output examples and employee interviews.

## Final Solution Summary

**Score Achieved**: 14,324 (71% improvement from baseline of 49,727)
**Exact matches**: 1/1000 (0.1%)
**Close matches**: 12/1000 (1.2%)

### Key Discoveries
1. **Pattern-based lookup tables** work better than simple formulas - the system has 84 different per diem rates
2. **Receipt endings .49/.99** follow completely different calculation rules
3. **High receipts (>$1500)** use declining percentage model based on amount and trip duration
4. **Penalty mode** exists for extremely inefficient trips (efficiency < 20 mi/day)
5. **Efficiency sweet spots** around 180-220 miles/day provide bonuses

### Final Architecture
- Pattern-based per diem lookup + $0.655/mile for normal cases
- Percentage model for high receipts with declining rates by amount
- Special handling for .49/.99 receipt endings
- Penalty mode for extreme inefficiency cases
- Efficiency bonuses for 180-220 mi/day range

## Key Commands

- **Test solution**: `./eval.sh` - Tests against 1,000 public cases
- **Generate results**: `./generate_results.sh` - Creates private_results.txt for submission  
- **Run single test**: `./run.sh <trip_duration_days> <miles_traveled> <total_receipts_amount>`

## Files

### Essential Files
- `run.sh` - Main entry point that calls refined_final_model.py
- `refined_final_model.py` - Final optimized model implementation
- `private_results.txt` - Generated results for 5,000 private test cases
- `public_cases.json` - 1,000 public test cases for development
- `private_cases.json` - 5,000 private test cases for final submission

### Reference Files  
- `PRD.md` - Original problem description
- `INTERVIEWS.md` - Employee interviews about the legacy system
- `README.md` - Challenge instructions
- `eval.sh` - Evaluation script
- `generate_results.sh` - Results generation script
- `run.sh.template` - Template for implementation