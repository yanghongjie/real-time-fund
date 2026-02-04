## ADDED Requirements

### Requirement: Position Amount Setting

The system SHALL allow users to set a position amount (in CNY) for each fund to track their investment.

#### Scenario: User sets position amount for a fund

- **WHEN** user clicks the position setting button on a fund card
- **THEN** a modal or inline input SHALL appear allowing the user to enter the position amount
- **AND** the amount SHALL be saved to localStorage under the key `positions`

#### Scenario: Position amount persists across sessions

- **WHEN** user refreshes the page or returns later
- **THEN** the previously set position amounts SHALL be restored from localStorage

#### Scenario: User clears position amount

- **WHEN** user sets the position amount to 0 or empty
- **THEN** the position for that fund SHALL be cleared
- **AND** daily profit SHALL no longer be displayed for that fund

---

### Requirement: Daily Profit Calculation

The system SHALL calculate and display the daily profit for funds with position amounts set.

#### Scenario: Calculate daily profit based on position and change rate

- **WHEN** a fund has a position amount set
- **AND** the fund has a valid change rate (gszzl or estGszzl)
- **THEN** the system SHALL calculate daily profit as: `position × (changeRate / 100)`
- **AND** display the profit amount with appropriate styling (red for profit, green for loss)

#### Scenario: Display profit in fund card

- **WHEN** viewing a fund card with position set
- **THEN** the daily profit amount SHALL be displayed alongside the change percentage
- **AND** the format SHALL be: `+¥123.45` or `-¥123.45`

#### Scenario: Display profit in list view

- **WHEN** viewing funds in list mode with position set
- **THEN** a new column SHALL display the daily profit amount

---

### Requirement: Portfolio Summary Display

The system SHALL display a summary section showing aggregated portfolio information.

#### Scenario: Display total assets

- **WHEN** user has set position amounts for one or more funds
- **THEN** the system SHALL display total assets calculated as: sum of all position amounts
- **AND** the total assets SHALL be displayed prominently in a summary section

#### Scenario: Display total daily profit

- **WHEN** user has funds with position amounts and valid change rates
- **THEN** the system SHALL calculate and display the total daily profit
- **AND** the total daily profit SHALL be the sum of individual fund daily profits
- **AND** appropriate styling SHALL indicate profit (red) or loss (green)

#### Scenario: Display total daily profit percentage

- **WHEN** total assets is greater than 0
- **THEN** the system SHALL display the total daily profit percentage
- **AND** the percentage SHALL be calculated as: `(totalDailyProfit / totalAssets) × 100`

#### Scenario: Summary hidden when no positions

- **WHEN** no funds have position amounts set
- **THEN** the summary section SHALL NOT be displayed
- **OR** SHALL display a prompt to set positions

---

### Requirement: Position Data Storage

The system SHALL persist position data using localStorage.

#### Scenario: Store positions in localStorage

- **WHEN** user sets or updates a position amount
- **THEN** the system SHALL save to localStorage with key `positions`
- **AND** the format SHALL be: `{ "fundCode1": amount1, "fundCode2": amount2, ... }`

#### Scenario: Include positions in data export

- **WHEN** user exports configuration data
- **THEN** the exported JSON SHALL include the `positions` object

#### Scenario: Import positions from data file

- **WHEN** user imports configuration data containing `positions`
- **THEN** the positions SHALL be merged with existing positions
- **AND** imported values SHALL override existing values for the same fund code

#### Scenario: Clean up positions when fund is removed

- **WHEN** user removes a fund from the list
- **THEN** the position for that fund SHALL also be removed from localStorage
