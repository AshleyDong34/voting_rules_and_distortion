# Library for Distortion in Utilitarian Social Choice Theory

The purpose of this project is to provide a way to easily access and experiment with distortion in social choice theory. This library contains ways to parse and generate ranking data, experiment with deterministic and randomized voting rules, and to calculate distortion from those rules.

## Description

This library provides tools for:
- Parsing and generating ranking data
- Experimenting with deterministic and randomized voting rules
- Calculating distortion from those voting rules

## Installation

### Cloning the Repository

```sh
# Clone the repository
git clone https://github.com/AshleyDong34/Masters-Project.git
```


## Setting up a virtual environment

# Navigate to the project directory
```sh
cd Masters-Project
```
# Create a virtual environment
```sh
python -m venv venv
```
# Activate the virtual environment
# On Windows
```sh
.\venv\Scripts\activate
```
# On macOS/Linux
```sh
source venv/bin/activate
```

## Installing dependencies

# run the following code on to install the dependencies on the virtual environment
```sh
pip install -r requirements.txt
```

## Data Setup

You need to create folders for all your data that you download. The data is found on the Preflib website(https://preflib.simonrey.fr/format#soc). There are 4 files you need to download to use the library offline. Each of them needs to be in their own folder named soc_data, soi_data, toc_data, and toi_data.

# Example structure
```sh
implementation/
├── soc_data/
│   ├── 00004-00000001.soc
│   └── ...
├── soi_data/
│   ├── ...
├── toc_data/
│   ├── ...
├── toi_data/
│   └── ...
```

## Usage

This library provides several ways to get ranking data with generated utilities

# 1. use the downloaded preflib data and parse them into a usable data type.

```
import numpy as np
from preflibtools import DataParser
from voting_rules import vr  

def custom_distribution():
    return np.random.uniform(0, 1)

parser = DataParser('soc_data/00004-00000001.soc', custom_distribution)
metadata, ranking_data, utilities_data = parser.parse()
num_alternatives = int(metadata['number_alternatives'])
winner, _ = voting_rule_function(ranking_data, num_alternatives)
print(f"The winner is: {winner}")
```
# 2. Using URL parsing that Preflib provides, therefore you do not have to download the data if you don't want to
```
import numpy as np
from preflibtools.instances import OrdinalInstance
from preflibtools import DataParser
import voting_rules as vr  # Replace with your actual import

# Custom distribution function for utility generation
def custom_distribution():
    return np.random.uniform(0, 1)

instance = OrdinalInstance()
instance.parse_url("https://www.preflib.org/static/data/irish/00001-00000001.soi")
parser = DataParser()
metadata, ranking_data, utilities_data = parser.parse_data(instance, custom_distribution)

# Use the ranking data and metadata to get the winner of a voting rule
num_alternatives = int(metadata['number_alternatives'])
winner, _ = vr.harmonic_scoring_rule(ranking_data, num_alternatives)
print(f"The winner is: {winner}")
```
# 3. Generate Ranking Data using Markov's model or single peaked preferences.

# Single Peaked Preferences:
```
candidates = [1, 2, 3, 4, 5]  # List of candidates
num_voters = 10
parser = generate_single_peaked_preferences(candidates, num_voters)
print(parser.ranking_data)
```
# Markov's Model(RIM)
```
num_voters = 10
num_candidates = 4
phi = 0.8  # A dispersion parameter; smaller values indicate closer to the reference ranking

parser = generate_rim_rankings(num_voters, num_candidates, phi)
parser.generate_utilities()
print(parser.utilities_data)
num_alternatives = int(parser.metadata['number_alternatives'])
winner, _ = vr.harmonic_scoring_rule(parser.ranking_data, num_alternatives)
print(f"The winner is: {winner}")
```
# Calculating Deterministic Distortion

# To use the deterministic distortion, there are two options: calculating either the average distortion over a number of iterations or the worst-case distortion over a number of iterations. It is set to average by default.
```
distortion = vr.det_distortion(winner, parser, num_iteres=10)
print(f"Distortion: {distortion}")
```

## Features

- Parsing ranking data from local files or URLs
- Generating utility data
- Implementing and experimenting with various voting rules
- Calculating distortion metrics

## License
This project is licensed under the MIT License - see the LICENSE file for details.

## Authors and Acknowledgements

- Ashley Dong - initial work
- Aris Filos-Ratsikas - supervisor

## Project Status

This project is completed and no longer maintained. Future development is not planned. 


