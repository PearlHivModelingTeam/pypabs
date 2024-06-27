# pyPABS: Population Agent Based Simulations in python

### Why another agent based simulation library?
There are a number of great projects that already exist that cover agent based simulation quite well, like [Mesa](https://github.com/projectmesa/mesa?tab=readme-ov-file), however they do not handle the case of a population agent based simulation in which the agents act independentily, but are acted upon by external events. An example of this type of model is [PEARL](https://github.com/PearlHivModelingTeam/comorbidityPaper).

### Goals
This project aims to make creating Population Agent Based Simulations (PABS) easy, as easy as creating agent based simulations in Mesa. Much of the insipiration for this project, comes from the ease of using Mesa, and extends its design.

# pyPABS early design documentation
The lifecycle of creating a population based simulation follows these steps:
- 1) Define the population generating functions. These are functions that produce `n` agents with the desired characteristics, where `n` is the population size.
- 2) Define event functions that runs during each step in the simulation. An example would be agents aging in a longitudinal study, or contracting a disease in the case of pearl.
- 3) Define an engine that runs `k` steps feeding the population into the event functions in a fixed or random order.
- 4) Run the simulation generating output artifacts.
- 5) Analyze the resulting artifacts to create reports or plots.

These steps contain the following key components:
- Population generating functions
- Population with persistent state
- Simulation engine
- Event functions for processing simulation states
- Artifacts from simulation runs
- Analysis of artifacts to generate reports and plots

## Population generating functions
- Return `n` agents that form the population. This generation could be through sampling, random generation, static values, parametric or non-parametric methods, but it must return a value for a given feature or feature in the population.
- The returned features may depend on the current state of the population based on other generating functions, so the order of feature generation matters.
## Population State
- The population must keep state of what the expected features are and communicate when a feature is referenced outside of scope.
- The population should also maintain it's expected representation, for instance in the case of long tailed distributions, if an instance of an agent does not exist in the given simulation, it's abscence should be referencible in later analysis.
## Event Function
- Event functions take the population as input and return a mutated population.
- Event functions must not reference features of a population that do not exist, and this should be checked before running to save time in development.
## Simulation Engine
- The engine takes a given seed population, and a list of event functions and run them over a number of steps.
## Artifacts
- The resulting population, as well as residual artifacts from the event functions must be stored in a manner that allows for efficient storage and retrieval for analysis.
- Large pandas dataframes should have correct types for feature columns to avoid wasted space
- Ability to write results to sql during/after run
## Analysis
- The scope of analysis is quite large, and user specific, however this library will support some basic utility functions.
