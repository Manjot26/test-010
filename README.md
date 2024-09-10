<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Activity Manager</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            display: flex;
            justify-content: center;
            align-items: center;
            height: 100vh;
            background-color: #f0f0f0;
            margin: 0;
        }
        .wrapper {
            background-color: white;
            padding: 18px;
            border-radius: 10px;
            box-shadow: 0 3px 6px rgba(0, 0, 0, 0.16);
            width: 370px;
        }
        h2 {
            text-align: center;
        }
        input {
            padding: 8px;
            margin: 4px 0;
            width: 100%;
            border: 1px solid #aaa;
            border-radius: 5px;
        }
        button {
            padding: 9px 18px;
            margin: 8px 0;
            width: 100%;
            background-color: #2e7d32;
            color: #fff;
            border: none;
            border-radius: 5px;
            cursor: pointer;
        }
        button:hover {
            background-color: #256728;
        }
        table {
            width: 100%;
            margin-top: 15px;
            border-collapse: collapse;
        }
        th, td {
            padding: 10px;
            text-align: center;
            border: 1px solid #ddd;
        }
        th {
            background-color: #f7f7f7;
        }
        .msg {
            color: red;
            text-align: center;
        }
    </style>
</head>
<body>

<div class="wrapper">
    <h2>Activity Manager</h2>

    <!-- Fields for adding activity -->
    <div>
        <input type="text" id="taskInput" placeholder="Activity">
        <input type="number" id="taskWeight" placeholder="Weight (1-100)">
        <button onclick="createTask()">Add Activity</button>
    </div>

    <!-- Fields for adding grade -->
    <div>
        <input type="number" id="taskGrade" placeholder="Grade (0-100)">
        <input type="number" id="taskNo" placeholder="Activity no">
        <button onclick="inputGrade()">Add Grade</button>
    </div>

    <!-- Button to remove activity -->
    <div>
        <button onclick="removeLast()" style="background-color: #dc3545;">Delete Activity</button>
    </div>

    <!-- Table displaying the activities -->
    <table id="taskTable">
        <tr>
            <th>No</th>
            <th>Activity</th>
            <th>Percentage</th>
            <th>Grade</th>
        </tr>
    </table>

    <!-- Placeholder for error messages -->
    <div class="msg" id="msgPlaceholder"></div>
</div>

<script>
    // Initialize an empty task list
    let taskList = [];
    // Element references
    let errMsg = document.getElementById('msgPlaceholder');
    let taskTbl = document.getElementById('taskTable');

    // Add an activity
    function createTask() {
        let taskInput = document.getElementById('taskInput').value;
        let taskWeight = document.getElementById('taskWeight').value;

        // Validate input
        if (taskInput === "") {
            displayMsg("Activity name rejected.");
            return;
        }

        if (taskWeight === "" || taskWeight < 1 || taskWeight > 100) {
            displayMsg("Weight rejected.");
            return;
        }

        // Add new task
        taskList.push({ taskInput, taskWeight, grade: null });
        clearInputs(['taskInput', 'taskWeight']);
        refreshTable();
    }

    // Add a grade for an activity
    function inputGrade() {
        let taskGrade = document.getElementById('taskGrade').value;
        let taskNo = document.getElementById('taskNo').value;

        // Grade validation
        if (taskGrade === "" || taskGrade < 0 || taskGrade > 100) {
            displayMsg("Grade rejected.");
            return;
        }

        if (taskNo === "" || taskNo < 1 || taskNo > taskList.length) {
            displayMsg("Activity No rejected.");
            return;
        }

        // Update task with grade
        taskList[taskNo - 1].grade = taskGrade;
        clearInputs(['taskGrade', 'taskNo']);
        refreshTable();
    }

    // Remove the last activity
    function removeLast() {
        if (taskList.length === 0) {
            displayMsg("No more tasks to remove.");
            return;
        }

        taskList.pop();
        refreshTable();
    }

    // Update the activity table
    function refreshTable() {
        taskTbl.innerHTML = `<tr>
            <th>No</th>
            <th>Activity</th>
            <th>Percentage</th>
            <th>Grade</th>
        </tr>`;

        taskList.forEach((item, idx) => {
            let newRow = taskTbl.insertRow();
            newRow.insertCell(0).innerText = idx + 1;
            newRow.insertCell(1).innerText = item.taskInput;
            newRow.insertCell(2).innerText = item.taskWeight;
            newRow.insertCell(3).innerText = item.grade !== null ? item.grade : "-";
        });

        clearMsg();
    }

    // Display an error message
    function displayMsg(msg) {
        errMsg.innerText = "Error: " + msg;
    }

    // Clear the error message
    function clearMsg() {
        errMsg.innerText = "";
    }

    // Clear input fields
    function clearInputs(fields) {
        fields.forEach(f => document.getElementById(f).value = "");
    }
</script>

</body>
</html>
