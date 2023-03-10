# Techtorial-Docker-Project
This project demostrates our students mastery with Docker Products.

## Instructions:

Now it's time to build a project that brings together the skils you've learned in docker.

I have 2 sets of instructions for you. These instructions are built so that you can check your mastery

The 1st has minimal tips and should be what you refer to for most of the project.

The 2nd set is inside a folder named Take-a-break-and-think-5 and contains more explicit instructions with some code examples. DO NOT open this one unless you've been stuck on the current step for at least 5 minutes or more. 

### things to do during the 5 minutes
    - take a look back at class slides for docker.
    - look at the hands-on practice we did. 
    - google the command you are trying to use and see if you can find the Docker documentation.

*This will help build vital troubleshooting skills that will make you more self sufficient and desirable to employers*

If you feel you still need help after referring to the 2nd set please let me know I will still be here assisting students during our Saturday session when they have questions.

### :fire: The challenge: :fire:



### Set up a Docker Compose project:

Create a new directory for the project.

Create a new file named docker-compose.yml in the directory.

*Your docker-compose file will need the following in order to run:*
make sure to include the entire directory in your build context.
you should have 2 services defined in the docker-compose file
call the first service "flask"
The flask container's port 5000 needs to be forwarded to the host machine's port 5000
flask shouldn't be able to run unless the second service, mysql, is working
call the 2nd service "mysql"
This service will pull in the "mysql:5.7" image
There should be an enviroment variable called "MYSQL_ROOT_PASSWORD:" make sure to remember this password for line !!!!



### Create a Dockerfile for the flask service:

Create a new file named Dockerfile in the directory.
*Your Dockerfile needs the following:*
the "python:3.9-slim" as an OS.
Set your work directory to "/app"
copy the contents of the directory the dockerfile is in into the current working directory of the container that will be built
run pip install with no cacheing and make sure to point it at the requirements.txt file
your last command on startup should python app.py hint: use CMD, python is an executable


### Create a requirements.txt file:
Create a new file named requirements.txt in the directory and out in the following dependencies
Flask==2.1.2
pymysql==1.0.2
### Create the Flask app:
Create a new file named app.py in the directory.
paste the following code:

from flask import Flask, render_template
import pymysql

app = Flask(__name__)

class Database:
    def __init__(self):
        host = "mysql"
        user = "root"
        password = "example"
        db = "employees"
        self.con = pymysql.connect(host=host, user=user, password=password, db=db, cursorclass=pymysql.cursors.
                                   DictCursor)
        self.cur = self.con.cursor()
    def list_employees(self):
        self.cur.execute("SELECT first_name, last_name, gender FROM employees LIMIT 50")
        result = self.cur.fetchall()
        return result

@app.route('/')
def employees():
    def db_query():
        db = Database()
        emps = db.list_employees()
        return emps
    res = db_query()
    return render_template('employees.html', result=res, content_type='application/json')

if __name__ == '__main__':
    app.run(debug=True, host='0.0.0.0')

### WAIT:exclamation::exclamation::exclamation:

Remember that password we put in as an enviroment variable on line 41? See if you can find where in the database class of this python file it needs to go. Your app won't work without it.

### Create the HTML template:
Create a new folder named templates in the directory.
Create a new file named employees.html in the templates folder.

paste this into the employees.html file:
<table>
  <tr>
    <th>First Name</th>
    <th>Last Name</th>
    <th>Gender</th>
  </tr>
  {% if result %}
    {% for row in result %}
      <tr>
        <td>{{ row.first_name }}</td>
        <td>{{ row.last_name }}</td>
        <td>{{ row.gender }}</td>
      </tr>
    {% endfor %}
  {% endif %}
</table>

### Start the Docker Compose project:

Run the necessary command to start the Docker Compose project, From the appropriate directory.
###  :partying_face: Access the application: :partying_face:
Open a web browser.
Go to the URL to access the application.
Celebrate, you're done!!! Now go get curious about what else you could do with this.
