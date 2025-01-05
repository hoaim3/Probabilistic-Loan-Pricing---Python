# Commercial Loan (Startup Lending) Analysis Model

This project simulates the financial feasibility of extending a loan to a small manufacturing business by evaluating the expected internal rate of return (IRR) under different scenarios. The model accounts for uncertainties in business maturity, default probabilities, recovery rates, and loan repayment dynamics. It allows for varying loan terms, interest rates, and default probabilities to help assess the risks and returns associated with different loan scenarios.

## Features

- **Simulates Year-by-Year Cash Flows:** Calculates the loan repayment, default probabilities, and recovery amounts over time.
- **Internal Randomness:** Uses stochastic elements to simulate default events and recovery processes.
- **Scenario Analysis:** Assesses different interest rates, initial default probabilities, and loan durations to calculate IRR.
- **Sensitivity Analysis:** Performs multiple iterations to ensure the robustness of the IRR estimates.
- **Visual Output:** Generates detailed outputs and data visualizations to assist with decision-making.

## Structure
- **Setup:** Imports necessary libraries and sets up the environment.
- **Inputs:** Defines the input parameters for the model, including machine price, loan life, default probabilities, interest rates, etc.
- **Model:** Core functions that simulate the loan repayment and default risk based on the inputs.
- **Output:** Generates the loan assessment results, including the IRR for each scenario.

## Model Workflow

### 1. **Default Risk Calculation**
   - The model calculates the default probability at each year, with higher default risk towards the final years. The default probability decays over time based on the business maturity.
### 2. **Internal Randomness**
   - The model incorporates randomness to simulate the possibility of default each year. The `get_the_payment_case` function uses random selection based on the calculated default probability to decide whether the loan is fulfilled or defaulted.
### 3. **Loan Repayment Calculation**
   - The model computes the loan repayment based on the machine price and the interest rate. It simulates the cash flows for each year of the loan term, taking into account both interest and principal repayment.
### 4. **IRR Calculation**
   - The Internal Rate of Return (IRR) is calculated based on the simulated cash flows over the life of the loan, factoring in both loan repayments and any recovery in case of default.
### 5. **Scenario & Sensitivity Analysis**
   - The model evaluates the IRR under different scenarios:
        - Loan interest rates (30%, 35%, 40%)
        - Initial default probabilities (0.1, 0.2, 0.3)
        - Loan durations (5, 10, 20 years)
- The model runs 1,000 iterations for each scenario to ensure reliable results.

## Functions Overview

- **`default_risk_at_year(data, year)`**: Calculates the default probability at a given year.
- **`get_the_payment_case(data, year)`**: Determines the payment case (Fulfillment or Default) based on the default probability.
- **`loan_repayment_at_year(data, year)`**: Calculates the loan repayment at a given year.
- **`loan_assessment(data, print_output=True)`**: Main function to simulate loan repayments and calculate the IRR.
- **`simulate_loan_iteration_df(data)`**: Runs multiple iterations and stores the results in a DataFrame.
- **`simulate_scenario_results(data)`**: Generates a summary of results for all scenario combinations (interest rate, loan life, initial default).

## Example Usage

```python
from dataclasses import dataclass
import numpy_financial as npf
import pandas as pd
import random
import itertools
import matplotlib.pyplot as plt

# Define the inputs
@dataclass
class ModelInputs:
    price_machine: float = 1_000_000
    loan_life: tuple = (5, 10, 20)
    initial_default: tuple = (0.1, 0.2, 0.3)
    default_decay: float = 0.9
    final_default: float = 0.4
    recovery_rate: float = 0.4
    interest: tuple = (0.3, 0.35, 0.4)
    num_iterations: int = 1000
    case_names: tuple = ('Fulfillment', 'Default')

# Initialize the model data
model_data = ModelInputs()

# Run the loan assessment and display results
irr_df = simulate_scenario_results(model_data)
print(irr_df)
```
## Output

The model will output a DataFrame containing the following columns:

- **Interest Rate**: The interest rate applied to the loan.
- **Loan Life**: The loan duration in years.
- **Initial Default Probability**: The initial probability of default.
- **IRR**: The calculated Internal Rate of Return for the loan scenario.

### Example Output:

| Interest Rate | Loan Life | Initial Default Probability | IRR   |
| -------------- | --------- | --------------------------- | ----- |
| 30%            | 5         | 10%                         | 14.75%|
| 35%            | 10        | 20%                         | 12.50%|
| 40%            | 20        | 30%                         | 10.00%|

## Conclusion

This model helps assess the financial feasibility of a loan by simulating cash flows and accounting for the dynamic risks associated with default and recovery. It provides a valuable tool for lenders to determine the expected return on investment based on varying scenarios.


