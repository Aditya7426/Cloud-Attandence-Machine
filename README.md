# Cloud-Attandance insert.htm
<!DOCTYPE html>

<html lang="en">
<head>

  <title>Attendance System</title>
  <meta charset="utf-8">
  <meta name="viewport" content="width=device-width, initial-scale=1">

  <link rel="stylesheet" href="https://maxcdn.bootstrapcdn.com/bootstrap/3.4.1/css/bootstrap.min.css">
  <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/4.7.0/css/font-awesome.min.css">
  <script src="https://ajax.googleapis.com/ajax/libs/jquery/3.5.1/jquery.min.js"></script>
  <script src="https://maxcdn.bootstrapcdn.com/bootstrap/3.4.1/js/bootstrap.min.js"></script>

  <meta name='viewport' content='width=device-width, initial-scale=1'>
  <script src='https://kit.fontawesome.com/a076d05399.js'></script>

</head>


<body>

  <nav class="navbar navbar-inverse" style="padding-bottom: 0px;">
    <div class="container-fluid" >
      <div class="navbar-header">
        <a class="navbar-brand" href="#">Attendance System</a>
      </div>
  
      <ul class="nav navbar-nav">
        <li><a href="home">Home</a></li>
        <li><a href="student_insert">Student</a></li>
        <li class="active"><a href="attendance_insert">Attendance</a></li>
        <li><a href="subjects_viewall">Subjects</a></li>
      </ul>
  
      <ul class="nav navbar-nav navbar-right">
        <li><a href="{% url 'logout' %}">
          Hi {{ user.first_name }}      
          
          <span class="glyphicon glyphicon-log-in"></span> Log Out</a></li>
      </ul>
  
    </div>
  </nav>
  
    <div class="row">
    <div class="container-fluid" style="padding-top: 0%;background-color: lightblue;">
      <ul class="nav navbar-nav">
        <div class="navbar-header">
          <i class='fas fa-user-graduate' style='font-size:50px;color: black;'></i>
        </div>
        <li ><a href="attendance_viewall">View All</a></li>
        <li ><a href="attendance_viewsingle">View Single</a></li>
        <li class="active" style="background-color: lightgrey;"><a href="attendance_insert">Insert</a></li>
        <li><a href="attendance_update">Update</a></li>
        <li><a href="attendance_delete">Delete</a></li>
      </ul>
    </div>


    <div class="row" style="padding-top: 10px;">

      <div class="col-sm-1"></div>
      <div class="col-sm-4" style="background-color: lightgrey">

        <form>

          {% csrf_token %}
          <label for="validationTooltip01">Student ID</label>
          <input type="text" style="background-color: lightyellow;width: 85%;" class="form-control" id="studentId" placeholder="Student ID" required>
          
          <label for="validationTooltip01" style="padding-top: 15px;">Subject</label>
          <div class="form-group">
            <select class="form-control" id="subjectCode" style="background-color: lightyellow;width: 85%;">
              <option>Advanced Mathematics</option>
              <option>Computer Networks</option>
              <option>Algorithms</option>
              <option>UI Design</option>
              <option>Database</option>
            </select>
          </div>
          
          <label for="validationTooltip01" style="padding-top: 5px;">Lecture Date</label>
          <input type="datetime-local" id="lectureDate" style="background-color: lightyellow;width: 85%;">
          <p></p>
          <label for="validationTooltip01" style="padding-top: 5px;">Status</label>
          <div class="form-group">
          <select class="form-control" id="status" style="background-color: lightyellow;width: 85%;">
            <option>Present</option>
            <option>Absent</option>
          </select>
        </div>
          <div style="padding-bottom: 10px;"></div>

          <button class="btn btn-primary" style="padding-top: 10px;" type="button" onclick="insert_record()">Submit</button>
        </form>
      </div>

      <div class="col-sm-2"></div>

      <div class="col-sm-4" style="background-color: lightgrey;">
        <div class="form-group">
          <label for="exampleFormControlTextarea1">Output</label>
          <textarea id="output" class="form-control rounded-0" id="exampleFormControlTextarea1" style="height: 295px;font-family: Verdana, Geneva, Tahoma, sans-serif;font-weight: 200;" readonly></textarea>
        </div>
      </div>

      <div class="col-sm-1"></div>
    </div>
 

</body>

<div class="footer" style="left: 0;bottom: 0;width:100%;background-color: grey;color: white;text-align: center;">
  <p>All Rights Reserved</p>

</html>

<script>
  function insert_record()
  {
      var data = new FormData();
      data.append('studentId', document.getElementById("studentId").value);
      data.append('subjectCode', document.getElementById("subjectCode").value);
      data.append('lectureDate', document.getElementById("lectureDate").value);
      data.append('status', document.getElementById("status").value);

      var request = new XMLHttpRequest()

      request.open('POST', 'http://cloud-attendance-system.eba-rqvb9aen.us-west-2.elasticbeanstalk.com/v1/attendance', true)

      request.onload = function() {
      // Begin accessing JSON data here
      var response_data = (this.response)

      if (request.status >= 200 && request.status < 400) {
         
         var message="Successfully Created new record\  ------ \n"
         var convert=JSON.parse(response_data)

         var array=['id','studentId','subjectCode','status','lectureDate']
         var count=0
         for(var i in convert) 
          {
            var key = array[count];
            var reply = convert[i]
              
            message+=key + " : " + reply + "\n\n"
            count+=1
          }

          document.getElementById("output").style.color="green";
          document.getElementById("output").value=message
        
      } 
      else {
          var message="Error : \n\n"
          var convert=JSON.parse(response_data)
          
          for(var i in convert) 
          {
            var key = i;
            var reply = convert[key]
              
            message+=key + " : " + reply + "\n\n"
          }

          document.getElementById("output").style.color="red";
          document.getElementById("output").value=message     
      }
      }

      request.send(data)            
  }

  
</script>
