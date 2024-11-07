# NBA Playoff Predictor: Machine Learning Model for NBA Season Outcomes

## Table of Contents
- [Overview](#overview)
- [Features](#features)
- [Requirements](#requirements)
- [Installation](#installation)
- [Usage](#usage)
- [Data Sources](#data-sources)
- [Model Explanation](#model-explanation)
- [Deployment](#deployment)
- [API Usage](#api-usage)
- [Future Improvements](#future-improvements)
- [License](#license)

## Overview

This project implements a machine learning model to predict NBA playoff teams, conference winners, and the NBA champion for a given season. It also includes functionality to create and evaluate a fictional NBA team.

## Features

- Predicts playoff teams for both Eastern and Western Conferences
- Forecasts conference winners and the NBA champion
- Creates a fictional team and estimates its playoff chances
- Provides a simple API for making predictions

## Requirements

- Python 3.7+
- pandas
- numpy
- scikit-learn
- Flask (for API)

## Installation

1. Clone this repository:
   ```
   git clone https://github.com/yourusername/nba-playoff-predictor.git
   cd nba-playoff-predictor
   ```

2. Install the required packages:
   ```
   pip install -r requirements.txt
   ```

## Usage

To run the main prediction script:

```
python nba_predictions.py
```

This will output the predicted playoff teams, conference winners, NBA champion, and a fictional team's playoff probability.

## Data Sources

This project uses the following data sources from FiveThirtyEight:

- NBA Elo ratings: https://projects.fivethirtyeight.com/nba-model/nba_elo_latest.csv
- RAPTOR team ratings: https://projects.fivethirtyeight.com/nba-model/2021/latest_RAPTOR_by_team.csv
- RAPTOR player ratings: https://projects.fivethirtyeight.com/nba-model/2021/latest_RAPTOR_by_player.csv

## Model Explanation

The prediction model uses a combination of Elo ratings and RAPTOR scores:

1. We combine the latest Elo rating for each team with their total RAPTOR score.
2. A logistic regression model is trained on this combined feature to predict playoff probabilities.
3. The top 8 teams from each conference (based on these probabilities) are selected as playoff teams.
4. Conference winners and the NBA champion are predicted based on the highest probability among playoff teams.

For the fictional team:
1. 15 players are randomly selected from the player pool.
2. The team's RAPTOR score is calculated by summing the selected players' scores.
3. This score is combined with an average Elo rating to predict the team's playoff probability.

## Deployment

### Apache Kafka and Spark Integration

To scale this model for real-time predictions:

1. Use Apache Kafka to stream real-time game data and player statistics.
2. Implement Spark Streaming to process this data in real-time, updating Elo ratings and RAPTOR scores.
3. Use Spark MLlib to train and update the logistic regression model across a cluster.
4. Store results in a distributed database like Cassandra for quick access.

### Web Deployment

To deploy the model as a web service:

1. Use the provided Flask app (`app.py`) as an API wrapper for the model.
2. Deploy the Flask app to a cloud service like Heroku or AWS Elastic Beanstalk.
3. Create a frontend using a framework like React or Vue.js to interact with the API.

## API Usage

After deploying the Flask app, you can use the API as follows:

```python
import requests
import json

url = "http://your-api-url/predict"
data = {
    "elo": 1500,
    "raptor_total": 10
}
headers = {'Content-type': 'application/json'}

response = requests.post(url, data=json.dumps(data), headers=headers)
result = response.json()
print(f"Playoff Probability: {result['playoff_probability']}")
```

## Future Improvements

- Incorporate more features like player injuries, schedule difficulty, and historical performance.
- Implement time series analysis to account for team momentum and trends.
- Create a more sophisticated model for simulating playoff series outcomes.

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

