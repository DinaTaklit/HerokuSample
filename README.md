# Deploy and application on Heroku 

## What is Heroku?
Heroku is a cloud platforme to host applications, databases and other services in several languages. It is easy and flexible to use. It is free, with paid specialized memberships.

## Getting Started on Heroku 

1. Create an account with [Heroku](https://signup.heroku.com/). We need also to install Heroku CLI(Command Line Interface) to run commands from the terminal to create a Heroku app.
2.  Install Heroku with Homebrew 
    ```
    brew tap heroku/brew && brew install heroku
    ``` 
After installing heroku you can run it using `hroku login` and provide auth info. After that you can run commands to create your heroku app.

## Heroku Deployment 
To configure your app such that it runs properly on Heroku, you should follow those steps:

- Creating and updating `requirements.txt` file to install depndecies using this command every time you add new dpendency 
  ```
  pip freeze > requirements.txt
  ```
- Setting up your environement varibales.
- Using a `Procfile`, using gunicorn to run the application

## Deplyment Configuration

### Environment Configuration

- Set up the envirement variables on `setup.sh`. We can also setup them using the web interface by going to settings then `Reveal Config Vars` you can define here a varibales in the table that appear just like you did in `setup.sh`. 

### Gunicorn 
Gunicorn is a pure-Python HTTP server for WSGI applications. We will use it to deploy our application.

- Install by: 
    ```
    pip install gunicorn
    ```
- Create `Procfile`. include one line to instruct Heroku correctly for us: `web: gunicorn app:app`. app is the name of your app or whatever is your application name and make sure your app is housed in that `app.py` or whatever you name your app file.
  
  ## Databse Migration
  
- To allow heroku run all your migrations to the database you have hosted on the platforme, your application need to include `manage.py` file.

- First istall those package: 
    ```   
    pip install flask_script
    pip install flask_migrate
    pip install psycopg2-binary
    ``` 
- `mange.py` should contain the following code 
    ``` 
    from flask_script import Manager
    from flask_migrate import Migrate, MigrateCommand

    from app import app
    from models import db

    migrate = Migrate(app, db)
    manager = Manager(app)

    manager.add_command('db', MigrateCommand)


    if __name__ == '__main__':
        manager.run()
    ```  
- We can run our local migrations using our `manage.py` file, to mirror how Heroku will run behind the scenes for us when we deploy our app:
    ```  
    python manage.py db init
    python manage.py db migrate
    python manage.py db upgrade
    ```
- Those last commands are the essential process that Heroku will run to ensure your database is architected properly. We, however, won't need to run them again unless we're testing the app locally.

Ps: Wenever you install new package remeber to freeze your `requirements.txt`

- Your file structure should be like this 
    ```
    > __pycache__
    > migrations
    .gitignore
    app.py
    manage.py 
    models.py 
    Procfile
    requirements.text
    Setup.sh
    ```