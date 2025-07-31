#  Data Cleaning Report: Hotel Booking Demand Dataset

## 1. Original Dataset Statistics
- **Total rows**: 119,390  
- **Total columns**: 32  
- **Dataset period**: July 2015 – August 2017  
- **Missing values detected** in: `children`, `country`, `agent`, `company`

---

## 2. Issues Identified and Their Impact

| Issue Type                 | Description                                                                  | Affected Rows |
|---------------------------|-------------------------------------------------------------------------------|---------------|
| Missing values            | NaNs in `children`, `country`, `agent`, `company`                            | Thousands     |
| Duplicates                | Exact duplicate rows found                                                   | 31,994        |
| Outliers                  | Extreme values in `adr`, `lead_time`, `guests`                               | Several       |
| Inconsistencies           | Case/spacing issues in `country`, `meal`, `market_segment`, etc.             | Multiple cols |
| Illogical values          | Bookings with 0 guests, ADR = 0 while not canceled, 0-night bookings          | Hundreds      |

---

## 3. Cleaning Methodology

###  Missing Values
- `children`: Filled with 0 (assume no children)
- `agent`, `company`: Filled with 0 (no agent/company involved)
- `country`: Imputed using mode (most frequent country)

###  Duplicates
- Removed all exact duplicate rows using `.drop_duplicates()`

###  Outliers
- Capped or removed extreme `adr`, `lead_time` values
- Removed bookings with 0 nights and not canceled

###  Inconsistencies
- Standardized string formatting: `.str.strip()` and `.str.upper()` or `.str.title()` applied to:
  - `country`, `meal`, `assigned_room_type`, `reserved_room_type`
  - `market_segment`, `deposit_type`, `customer_type`, `reservation_status`

###  Illogical Values
- Removed rows where:
  - `adults + children + babies == 0`
  - `adr < 0`
  - `stays_in_weekend_nights + stays_in_week_nights == 0` and not canceled
  - `adr == 0` and not canceled
  - `company == 0` but `market_segment == 'Corporate'`

---

## 4. Final Dataset Statistics
- **Final row count**: *(update this with `df.shape[0]` after cleaning)*  
- **Total columns**: 33 (added `arrival_date`, `total_guests`)
- **Missing values**: 0  
- **Duplicates**: 0  
- **Cleaned and ready for analysis**

---

## 5. Assumptions Made During Cleaning

- If `children` is missing, assume 0
- If `agent` or `company` is missing, assume no intermediary involved
- If `country` is missing, assume it follows the most common country
- Rows with no guests or nonsensical values are invalid and removed
- Categorical values were standardized to improve consistency and modeling

---
