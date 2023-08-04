# eta-prediction-privacy-guardian-oauth2
This is the backend server component of ETA Prediction tool writtten in Python Flask.

The server component has single code base that forks multiple instances. This is Flask based app backed by Gunicorn to achieve processing multiple requests at the same time.

At a minimum 2 server instances need to be deployed - one instannce receives a pre-configured random number R sent by the clients and other one receives difference of X and R where X is the numbered entered by the user. This is Secured Multi-Party Computation (MPC) using additive secret sharing. Let's call these servers R_SERVER and X_R_SERVER respectively.

The Privacy Guardian has the following functions:

1. Creating dynamic endpoints - this is for the developers who can request endpoints to be created
2. Post ETA data - this POST endpoint allows end users to post the ETA data
3. Get ETA data - this GET endpoint allows Data Scientists to query the ETA data posted by end users
4. These end points are protected by user roles. For example, a user with "user" role can post ETA data whereas only users with "scientist" role can get the data and "developer" role can create endpoints
5. The serevr code is independent of any OAuth2 provider (e.g. Auth0) implementation. This means that if the provider is changed in future, the server code remains unimpacted
6. ***Over all idea is to develop a frameowkr that enables anyone to bring in algorithms in andhave developer create end points and user-facing apps***

## Prerequisites

- Python 3.x

## Installation

1. Clone the repository:
   ```
   git clone https://github.com/your-username/eta-prediction-privacy-guardian-oauth2.git
   ```

3. Navigate to project directory:
   ```
   cd eta-prediction-server
   ```

4. Create a virtual env:
   ```
   python3 -m venv .venv
   ```

5. Activate the virtual environment
   ```
   source .venv/bin/activate
   ```

6. Install the required packages:
   ```
   python -m pip install -r requirements.txt
   ```

7. Cretae a ```.env``` file with below contents:
   ```
   FLASK_APP = wsgi.py
   FLASK_ENV = production
   
   NUM_CLIENTS_TO_SUM = 5 #

   LOG_FILE_DIR = .
   ```

8. Start R_SERVER:
   ```
   export LOG_FILE_NAME=r_server.log
   export SERVER_ID=R_SERVER
   gunicorn --workers=1 --threads=1 -b :8181 wsgi:app &
   ```

9. Start X_R_SERVER
   ```
   export LOG_FILE_NAME=x_r_server.log
   export SERVER_ID=X_R_SERVER
   gunicorn --workers=1 --threads=1 -b :8281 wsgi:app &
   ```
