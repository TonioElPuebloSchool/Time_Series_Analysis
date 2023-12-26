<img style="float: left; padding-right: 10px; width: 200px" src="https://upload.wikimedia.org/wikipedia/fr/b/b1/Logo_EPF.png?raw=true"> 

## Time Series Analysis
**P2024** Antoine Courbi | Thibault Gillard

# Yearly electrical production analysis

-----
### [*Moodle course*](https://moodle.epf.fr/course/view.php?id=9512)
### [*Github depository*](https://github.com/EPF-MDE/MMMDE4IN19-22-antoine-courbi)
-----

This **README** is meant to explain **how to run** the application and what **features** are implemented.

# **Requirements**
The following requirements are needed to run the application:
```bash
npm install
npm install express-basic-auth
npm install method-override
npm install method-override body-parser
```
Now `npm run dev` can be used **in the epfbook folder** to run the application properly, after which the application can be accessed at http://localhost:3000/.
The auhtentification is done with the following credentials:
```bash
username,password
eric-burel,1234admin
acourbi,newpassword
admin,admin
```
There is no other kind of auhtentification, so the user can access the application with any of the credentials above.

# **Overview**

The application is a simple `web application` that allows the user to see a **list of students** and their details. The user can also `update` the details of a student, `add` a student or `delete` a student. The user can also access the `data` of the students by `downloading` a csv file. There is also a `search bar` that allows the user to search for a student by name, and a `chart` page that displays a chart on the *covid-19-students-delhi.csv* file.

# **Architecture**

The application is composed of the following files in `epfbook` folder:
- app.js
- package.json
- package-lock.json
- students.csv
- users.csv
- users_uncrypted.csv
- **views folder**
    - home.html
    - create-student.ejs
    - search_results.ejs
    - student_details.ejs
    - student_unknown.ejs
    - student_unknown.ejs
    - students-data.ejs
    - students.ejs
- **public folder**
    - covid-19-students-delhi.csv
    - style.css
    - style_chart.css
    - students.js

# **Features**
# Exercise 0: Consume an existing API

If we want to retrieve the character whose id is 5, we can follow the documentation where we can see the following :
```bash
You can get a single character by adding the id as a parameter: /character/
GET https://rickandmortyapi.com/api/character/2 
```
Hence when following the url for character with id=5 we get :
```bash
{"id":5,"name":"Jerry Smith","status":"Alive","species":"Human", ...
```

# Exercise 1: A page per student

In order to have a different page for **each student**, we need to add a route for each student. We can do this by adding the following code in the app.js file:
```bash
app.get('/students/:id', (req, res)
```
We also need to create a `student_details.ejs` file in the `views` folder. This file will be used to display the details of a student.

If a student is not found, we can redirect the user to a `student_unknown.ejs` file. This file will display a message saying that the student is not found as follows:
```html
<h1>Student Not Found</h1>
      <p>The requested student does not exist.</p>
```


# Exercise 2: Update a student

Still in the `student_details.ejs` file, we can add a form to update the student. This form will be sent to the server with a **POST request**. The server will then update the student in the `students.csv` file. The following code is used to update the student:
```bash
fs.createReadStream('students.csv')
    .pipe(csv.parse({ headers: true }))
    .on('data', (data) => {
      students.push(data);
    })
    .on('end', () => {
      students[studentId] = updatedStudent;

      // Write the updated students array back to the CSV file
      const ws = fs.createWriteStream('students.csv');
      csv.write(students, { headers: true })
        .pipe(ws)
        .on('finish', () => {
          console.log(`Updated student with ID: ${studentId}`);
          res.redirect(`/students/${studentId}`);
        });
    });
```
As you can see after the update is done, we redirect the user to the `student_details` page, which will refresh the page and display the updated student.

# Exercice 3: Delete a student

In order to **delete** a student, we need to add a button in the `student_details.ejs` file. This button will send a **POST request** to the server. The server will then **delete** the student from the `students.csv` file. After the deletion is completed, the user is redirected to the students page. 

To make sure no student is deleted by mistake, we can add a **confirmation message** before the deletion is done. This is done by adding the following code in the `student_details.ejs` file:
```bash
function confirmDelete() {
          if (confirm("Are you sure you want to delete this student?")) {
            window.location.href = "/students/<%= studentId %>/delete";
          }
        }
```
This function is called when the user clicks on the delete button. If the user confirms the deletion, the function redirects the user to the delete route. 

# Exercice 4: Search for a student

In the `students.ejs` file, we can add a **search bar**. If the user types a name in the **search bar**, the students page will be refreshed and only the students whose name contains the string typed by the user will be displayed. It also works for the student's school. 

To do so we need to create a new page for the results of the search, displaying the students found as **clickable links**, redirecting towards the `student_details` page. 

# Exercice 5: Download the list of students

We can also add a route to **download** the list of students in a csv file. This is done by adding the following code in the app.js file, in the `app.get('/students/:id', (req, res)` route:
```bash
if (req.params.id === 'download') {
    // adding the route to download the data
    console.log("downloading file");
    res.download('./students.csv');
    return;
  }
```

# Exercice 6: Adding a chart page

We can use the **chart** page created in the previous TP. We just need to add a route to access this page. This is done by adding the following code in the `app.js` file:
```bash
app.get("/students/data", (req, res) => {
  res.render("students-data");
});
```

# Exercice 7: Adding a menu
To finalize the application, we can add a **menu** to navigate between the different pages. We use the same menu for all of the pages, added at the top of the page as follows:
```html
<nav>
    <ul>
        <li><a href="/">Home</a></li>
        <li><a href="/students/data">Chart</a></li>
        <li><a href="/students">Students</a></li>
        <li><a href="/students/create">Create a Student</a></li>
        <li><a href="/students/download">Download CSV</a></li>
    </ul>
</nav>
```
# Exercice 8: Work on the design

We can work on the design of the application by modifying the `style.css` file. We can add a background color, change the color of the links, change the font, etc. We use a differente css file for the chart page, `style_chart.css`, to change the design of the chart, because we don't want the same design for the chart and the rest of the application.

Thank you for reading this README. I hope you enjoyed it. If you have any remarks or questions, feel free to contact me on [LinkedIn](https://www.linkedin.com/in/antoine-courbi/).

<p align="center">&mdash; ⭐️ &mdash;</p>
<p align="center"><i>This README was created during the Web Programming course</i></p>
<p align="center"><i>Created by Antoine Courbi</i></p>