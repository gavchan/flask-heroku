# Simple Python Flask Heroku Deployment

Goal: To create a barebones Python web "app" using Flask and deploy to [Heroku](https://heroku.com)

*Assumptions:*

- Python 3.5 or higher installed
- Heroku installed and setup with login credentials

## Steps

### 1. Create directory and virtual environment

```
mkdir testapp
cd testapp
python3 -m venv venv      # Recommended method since Python version 3.5
source venv/bin/activate  # Activate virtual environment
```

### 2. Install flask and gunicorn using `pip`

```
pip install flask gunicorn
pip freeze > requirements.txt
```

- Heroku will look for `requirements.txt` for creation of web app.

### 3. Configure `Procfile` for Heroku

```
echo "web: gunicorn app:app" > Procfile
```

### 4. Initialize Heroku to use Python

```
heroku create --buildpack heroku/python
```

- Note: Heroku app would be generated, e.g. called `funky-ocean-12345`

### 5. Create simple Flask app called `app.py` in main directory

```
from flask import Flask
app = Flask(__name__)
@app.route('/')
def index():
    return "It's working!"
if __name__ == "__main__":
    app.run()
```

- Note that `app.py` must be in the main directory or else an application error would occur.
- Can verify that application works by running `python app.py` then go to `localhost:5000` in the web browser to view the app.

### 6. Setup Git

#### Create a new Git repository

```
git init
heroku git:remote -a funky-ocean-12345    # Setup git remote with heroku
```

#### Deploy application on Heroku

```
git add .
git commit -m "initial commit"
git push heroku master
```

*To ensure at least one instance of app is running:*

```
heroku ps:scale web=1
```

*To visit app at the URL:*

```
heroku open
```

#### To add another remote repository to store the local repository (e.g. Github)

- Assumes creation of a new repository on Github with `repo-name`

```
git remote add origin https://github.com/<username>/repo-name.git
git push -u origin master
```

## Resources and Further Reading:

- Adapted from [How to deploy a Flask App to Heroku](https://progblog.io/How-to-deploy-a-Flask-App-to-Heroku/)
- [Getting started on Heroku with Python](https://devcenter.heroku.com/articles/getting-started-with-python#introduction)
- [Creation of virtual environments using python3 -m venv](https://docs.python.org/3/library/venv.html#module-venv) instead of using `pipenv` as described in Heroku documentation.
