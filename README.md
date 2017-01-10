# it394_template
A project template for a Python/Django project ready to deploy into the Heroku production environment.

#Creating a new project using Django, Heroku and this Template.
Create a new folder and virtual environment named DAD.
```
mkdir IT384
cd IT384
virtualenv DAD
```

## Start your DAD virtual environment.
```
cd DAD
source bin/activate
```

## Test to see if it is working properly
```
pip –V
```
You should see
```
pip 1.5.6 from /home/cdt/<xnumber>/it394/DAD/lib/python2.7/site-packages (python 2.7)
```

##Install Django – This takes a couple of minutes
```
pip install django
```

## Use Django to create named Project

To use this template start a new project using django-admin and the --template parameter
```
django-admin startproject --template=https://github.com/usma-eecs/IT394_template/archive/master.zip <project name>
```

You should use an all lower case name (with no special characters) for your project name.

## Update Select Files

django-admin startproject will correctly rename the project_name folder and also replace the {{ }} patterns with the correct values for the contents of the files in the project_name folder.

Unfortunately, it does not apply changes to the contents of files in the root directory of the project. Use grep to find the file that requires modifications.

```
cd <project name>
grep –rnw ./ -e “{{ project_name }}”
```

Change the {{ project_name }} to <project name> in the approriate file.


For example, if I wanted to start a new project named "nifty" I would enter the following:
```
django-admin startproject --template=https://github.com/usma-eecs/IT394_template/archive/master.zip nifty
cd nifty
grep –rnw ./ -e “{{ project_name }}”
```

## Install packages required by Heroku
Install the packages from requirements.txt (you probably already have these installed in your DAD environment):
```
pip install -r requirements.txt
```

## Local data management commands

Create the initial database structure and create the initial security principal with:
```
python manage.py migrate
```

See: https://docs.djangoproject.com/en/1.10/topics/migrations/ 


# Create Local git Repository

Make sure you are in root directory for the project not in the subdirectory with the same name. Then create a local git repository, add all the files and commit them. 

```
git init
git add --all
git status
```

At this point you should see all of the files on branch master that you added. It should be all of the files from your new project. Now commit the files to your repository.

```
git commit –m “Initial Project Commit”
```

#Deploy into production
After creating a local git repository, deploy it to heroku. First login into your heroku account with:
```
heroku login
```
Then tell heroku to create a new application with:
```
heroku create
```
The "heroku create" command will add a new remote to your local git repo.  You can see it with:
```
git remote -v
```
Now you can push the local repo to heroku.  This is how you "deploy your project into the production environment on Heroku".
```
git push heroku master
```
You can observe the status of the application on heroku with
```
heroku logs
heroku ps
```

## Remote data management commands
We are separated from Heroku by a firewall that blocks interactive access to Heroku one-off dynos (See: https://devcenter.heroku.com/articles/one-off-dynos).  Because of this you'll need to interact with Heroku using the run:detached option.


Create the database structure with
```
heroku run:detached python manage.py migrate
```

The firewall configuration also complicates the creation of the projects initial security principal (the "superuser"). Check the file named create_admin.sh.  It provides a method to create the superuser on heroku.

```
heroku run:detached bash ./create_admin.sh admin admin@example.com password
```

# Misc

## Reuse a Heroku Application Instance
Instead of creating a new Heroku application with "heroku create", You can associate a new clone of a project with an existing Heroku application instance.  I could clone this repository then associate it with the heroku application "immense-scrubland-9864" (previosly made with "heroku create" on another computer) with the following command:
```
heroku git:remote –a immense-scrubland-9864
```

## Procfile
Heroku uses the commands in the Procfile to run your application.  See https://devcenter.heroku.com/articles/procfile for additional information.

# References
The standard django-admin templates for projects and apps are at https://github.com/django/django/tree/master/django/conf


