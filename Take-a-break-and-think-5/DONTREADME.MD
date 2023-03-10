

Create a new directory for the project

> mkdir docker-project

Create a new file named docker-compose.yml in the directory

> touch docker-compose.yml

Open the file with nano

> nano docker-compose.yml

Add the following code to the docker-compose.yml file:

version: "3"
services:
  flask:
    build: .
    ports:
      - "5000:5000"
    depends_on:
      - mysql
  mysql:
    image: mysql:5.7
    environment:
      MYSQL_ROOT_PASSWORD: example
## Create a Dockerfile:
Create a new file named Dockerfile in the directory

> touch Dockerfile
> nano Dockerfile

Add the following code to the Dockerfile:

FROM python:3.9-slim
WORKDIR /app
COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt
COPY . .
CMD ["python", "app.py"]


Create a requirements.txt file:

Create a new file named requirements.txt in the directory
> touch requirements.txt
> nano requirements.txt
Add the following code to the requirements.txt file:

Flask==2.1.2
pymysql==1.0.2


## Create the Flask app:

Create a new file named app.py in the directory
> touch app.py
> nano app.py

Add the following code to the app.py file:

START HERE 
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
END HERE

Create the HTML template:
Create a new folder named templates in the directory
> mkdir templates

Create a new file named employees.html in the templates folder
> cd templates
> mkdir employees.html
> nano employees.html
Add the following code to the employees.html file:

START HERE
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
END HERE


Start the Docker Compose project:

Run the following command in the correct directory

> docker-compose up
