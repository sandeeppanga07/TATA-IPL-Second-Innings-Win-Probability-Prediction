# TATA-IPL-Second-Innings-Win-Probability-Prediction
# TATA IPL – Second-Innings Win Probability Prediction

> **Machine Learning + Streamlit application to predict the winning probability of a team during the second innings of a TATA IPL match.**


## Project Overview

In a TATA IPL match, the situation of the chasing team changes after every ball.

For example:

* How many runs are still required?
* How many balls are remaining?
* How many wickets are available?
* What is the current run rate?
* What run rate is required?
* Which team is batting?
* Which team is bowling?
* Where is the match being played?

Instead of simply predicting **Win/Loss**, this project predicts the **probability of winning at the current match situation**.

### Example

If the current match situation is:

Target          : 190
Current Score   : 145
Balls Remaining : 32
Wickets Left    : 6
Required RR     : 8.44

The application may produce:

Chasing Team Win Probability: 78%


This makes the model suitable for a **real-time-style IPL match analysis system**.


# Business Problem

The objective is to build a **binary classification machine-learning model** that predicts whether the team batting in the second innings will eventually win the match.

The model takes the current match state as input and generates:

*  Probability of chasing team winning
*  Probability of chasing team losing

The prediction can be refreshed as the match situation changes.

# Business Objective

The major objectives of this project are:

1. Predict second-innings winning probability.
2. Convert the current match state into meaningful ML features.
3. Provide real-time-style predictions.
4. Create an easy-to-use Streamlit interface.
5. Help cricket fans and analysts understand the impact of match situations.
6. Demonstrate an end-to-end machine-learning deployment workflow.

#  Dataset

The project uses a cleaned IPL dataset containing second-innings match-state information.

| Property       |                 Value |
| -------------- | --------------------: |
| Rows           |               102,975 |
| Columns        |                    11 |
| Target         |              `result` |
| Target Type    | Binary Classification |
| Loss Class     |                   `0` |
| Win Class      |                   `1` |
| Teams          |                    12 |
| Cities         |                    32 |
| Missing Values |                     0 |

### Dataset

The original dataset is available here:

**Google Drive Dataset:**
https://drive.google.com/file/d/156QSkPOzJl2fTwlWG7zniXcFBbk8ge-1/view?usp=sharing

#  Features Used

| Feature        | Type        | Description                                  |
| -------------- | ----------- | -------------------------------------------- |
| `batting_team` | Categorical | Team currently batting in the second innings |
| `bowling_team` | Categorical | Team currently bowling                       |
| `city`         | Categorical | Match location                               |
| `runs_left`    | Numerical   | Runs required to win                         |
| `balls_left`   | Numerical   | Deliveries remaining                         |
| `wickets`      | Numerical   | Wickets available                            |
| `total_runs_x` | Numerical   | Target score                                 |
| `crr`          | Numerical   | Current Run Rate                             |
| `rrr`          | Numerical   | Required Run Rate                            |
| `result`       | Target      | `1 = Win`, `0 = Loss`                        |

#  Machine Learning Problem

This project is formulated as a:

> **Binary Classification Problem**

### Target Variable

result

Where:
0 → Loss
1 → Win

The trained model generates class probabilities using:

python
model.predict_proba(input_df)


The application then extracts:

python
loss = result[0][0]
win = result[0][1]

#  Match-State Feature Engineering

The Streamlit application does not directly ask the user for every ML feature.

Instead, it derives important features from the user's match inputs.

## 1. Balls Bowled

python
balls_bowled = overs * 6 + balls


## 2. Balls Left

python
balls_left = 120 - balls_bowled
Since an IPL innings contains 20 overs:
20 × 6 = 120 balls

## 3. Runs Left

runs_left = target - score

## 4. Wickets Available

wickets = 10 - wickets_out

## 5. Current Run Rate

crr = score / (balls_bowled / 6) if balls_bowled > 0 else 0

## 6. Required Run Rate
rrr = (runs_left * 6) / balls_left

# Match-State Validation

The application checks whether the match has already finished.

### Match Over

python
if balls_left <= 0:
    st.error("Match Over")
    st.stop()

### Chasing Team Won

python
if runs_left <= 0:
    st.success(f"{batting_team} won the match!")
    st.balloons()
    st.stop()
The application also prevents the user from selecting the same team for batting and bowling.
# Streamlit Application

The application provides an interactive interface where the user can enter:

*  Batting Team
*  Bowling Team
*  Match City
*  Target Score
*  Current Score
*  Overs Completed
*  Balls Completed
*  Wickets Lost

After entering the match state, the user can click:
Predict Probability
The trained ML model then generates the probability of winning and losing.

# Prediction Output

The application displays two probability metrics:

### Chasing Team
Batting Team
Win Probability: 78%
### Defending Team
Bowling Team
Win Probability: 22%
The application also displays progress bars corresponding to the predicted probabilities.

#  Project Architecture

                IPL Dataset
                     │
                     ▼
             Data Preprocessing
                     │
                     ▼
           Feature Engineering
                     │
                     ▼
             ML Model Training
                     │
                     ▼
               iplmodel.pkl
                     │
                     ▼
              Streamlit App
                     │
          ┌──────────┴──────────┐
          ▼                     ▼
    User Match Input       Match-State
                              Logic
                                  │
                                  ▼
                         Feature Calculation
                                  │
                                  ▼
                         ML Model Prediction
                                  │
                                  ▼
                       Win/Loss Probability
                                  │
                                  ▼
                     Interactive Dashboard

#  Project Structure
TATA-IPL-Win-Probability-Prediction/
│
├── app.py
│
├── iplmodel.pkl
│
├── TataIplcleaned.csv
│
├── requirements.txt
│
└── README.md
### File Description

| File                 | Purpose                        |
| -------------------- | ------------------------------ |
| `app.py`             | Streamlit application          |
| `iplmodel.pkl`       | Trained machine-learning model |
| `TataIplcleaned.csv` | Cleaned IPL dataset            |
| `requirements.txt`   | Python dependencies            |
| `README.md`          | Project documentation          |

#  Technologies Used

### Programming Language

* Python

### Libraries

* Pandas
* NumPy
* Scikit-learn
* Streamlit

### Machine Learning

* Binary Classification
* Probability Prediction
* Feature Engineering
* Categorical Feature Handling

### Deployment

* Streamlit

#  Installation

## 1. Clone the Repository

git clone https://github.com/your-username/TATA-IPL-Win-Probability-Prediction.git
Navigate into the project:
cd TATA-IPL-Win-Probability-Prediction
## 2. Create a Virtual Environment
python -m venv venv
### Windows
venv\Scripts\activate
### Linux / macOS
source venv/bin/activate
#  Install Dependencies
pip install -r requirements.txt
#  Run the Application

Execute:
streamlit run app.py
The application will open in your browser.
#  Example Match Scenario

Suppose:
Batting Team       : Chennai Super Kings
Bowling Team       : Mumbai Indians
Target             : 185
Current Score      : 130
Overs Completed    : 15
Balls Completed    : 3
Wickets Lost       : 4
The application internally calculates:
Balls Bowled = 15 × 6 + 3
             = 93
Balls Left   = 120 - 93
             = 27
Runs Left    = 185 - 130
             = 55
Wickets Left = 10 - 4
             = 6
Then:
CRR = Current Score / Overs Completed
RRR = Runs Left × 6 / Balls Left
These derived features are passed to the trained model.
The final output could look like:
CSK
Win Probability
78%
and
MI
Win Probability
22%
> **Note:** The displayed probability depends on the trained model and the supplied match state.

#  Key Features

###  Interactive Team Selection

Select:
Batting Team
Bowling Team
from the available dataset teams.

###  City Selection

Select the match city from the available dataset locations.

###  Dynamic Match-State Calculation

The application automatically calculates:

* Runs Left
* Balls Left
* Wickets Available
* Current Run Rate
* Required Run Rate

###  Probability Prediction
The trained model predicts:
P(Loss)
P(Win)
using `predict_proba()`.

###  Match-State Feedback

The application handles:
* Match completed
* Chasing team already won
* High winning probability
# Important Design Decision
The user enters:
Target
Score
Overs
Balls
Wickets Lost
while the ML model receives derived features such as:
runs_left
balls_left
wickets
total_runs_x
crr
rrr
This separation makes the application easier to use while preserving the feature representation expected by the trained model.

#  Future Improvements

Possible improvements include:

*  Live IPL score integration
*  Ball-by-ball probability graph
*  Win probability over time
*  Venue-specific analysis
*  Player-level impact features
*  Batsman and bowler information
*  Recent-over performance
*  Model performance dashboard
*  Explainable AI using SHAP
*  Cloud deployment
*  Mobile-friendly UI

#  Disclaimer

This project provides a **machine-learning-based probability estimate**.

The predicted probability should not be interpreted as a guaranteed match result. Cricket outcomes depend on many factors that may not be represented in the model.

This project is intended for:

* Educational purposes
* Cricket analytics
* Machine-learning demonstration
* Fan engagement
* Data science portfolio development

#  Author

**P.sandeep yadav**

Data Science | Machine Learning | Python | Streamlit
#  If You Like This Project

If you find this project useful, consider giving the repository a  on GitHub.
 Predict the probability.
 Understand the match.
 Let Machine Learning analyze the chase.

## License

This project is intended for educational and demonstration purposes.
