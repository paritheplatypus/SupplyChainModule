# Supply Chain Module ([MizzouCloudDevOps](https://www.mizzouclouddevops.net/MizzouCloudDevOps/#!/home_page))

This module contains code authored by @parisa (Fatemah Pourdehghan Golneshini).

## Architecture Summary

main_SA.py
│  
├── generate_data.py           --> Reads Excel data → creates 'data' dictionary  
├── initial_solution.py        --> Creates an initial feasible solution  
├── calculate_objective.py     --> Evaluates the cost of a solution  
├── generate_neighbor.py       --> Generates neighbor solutions using 14 operators  
│                                (also runs all feasibility checks!)  
├── feasibility.py              └ All feasibility functions  
├── export_SA_results.py       --> Writes results to Excel  
