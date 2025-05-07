<!DOCTYPE html>
<html>
<head>
    <title>Attendance Sheet</title>
    <style>
        table, th, td {
            border: 1px solid black;
            border-collapse: collapse;
            padding: 10px;
        }
        input {
            margin: 5px;
        }
        #printBtn {
            display: block;
            margin: 20px auto;
            padding: 10px 20px;
        }

        /* Hide everything except the table during printing */
        @media print {
            body * {
                visibility: hidden;
            }
            #print-area, #print-area * {
                visibility: visible;
            }
            #print-area {
                position: absolute;
                top: 0;
                left: 0;
            }
        }
    </style>
    <script>
        let count = 1;

        function add_row() {
            const date = document.getElementById("date").value;
            const rollNo = document.getElementById("rollNo").value;
            const name = document.getElementById("name").value;
            const status = document.getElementById("status").value;

            if (!date || !rollNo || !name || !status) {
                alert("Please fill in all fields.");
                return;
            }

            const table = document.getElementById("data");
            const row = table.insertRow(-1);

            row.insertCell(0).innerText = count;
            row.insertCell(1).innerText = date;
            row.insertCell(2).innerText = rollNo;
            row.insertCell(3).innerText = name;
            row.insertCell(4).innerText = status;

            const deleteCell = row.insertCell(5);
            const delBtn = document.createElement("button");
            delBtn.innerText = "Delete";
            delBtn.onclick = function () {
                table.deleteRow(row.rowIndex);
                updateSerialNumbers();
            };
            deleteCell.appendChild(delBtn);

            count++;

            document.getElementById("date").value = "";
            document.getElementById("rollNo").value = "";
            document.getElementById("name").value = "";
            document.getElementById("status").value = "";
        }

        function updateSerialNumbers() {
            const table = document.getElementById("data");
            count = 1;
            for (let i = 1; i < table.rows.length; i++) {
                table.rows[i].cells[0].innerText = count++;
            }
        }

        function printTable() {
            window.print();
        }
    </script>
</head>
<body>

    <div style="text-align:center; margin-bottom:20px;">
        <input type="date" id="date" placeholder="Date" />
        <input type="number" id="rollNo" placeholder="Roll Number" />
        <input type="text" id="name" placeholder="Name" />
        <input type="text" id="status" placeholder="Present (P) or Absent (A)" />
        <button onclick="add_row()">Add Row</button>
    </div>

    <div id="print-area">
        <table id="data" align="center">
            <tr>
                <th>Sr No.</th>
                <th>Date</th>
                <th>Roll Number</th>
                <th>Name</th>
                <th>Present (P) or Absent (A)</th>
                <th>Action</th>
            </tr>
        </table>
    </div>

    <button id="printBtn" onclick="printTable()">Print Attendance Sheet</button>

</body>
</html>
