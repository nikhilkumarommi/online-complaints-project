the above are all the project files used in this project
├── backend/
│   └── server.js
├── frontend/
│   ├── index.html
│   └── style.css
├── database/
│   └── schema.sql
├── data/
│   └── sample_complaints.json
├── report/
│   └── Final_Project_Report.md
├── README.md
├── .gitignore
├── LICENSE

 backend/server.js
js
const express = require('express');
const app = express();
app.use(express.json());

let complaints = [];

app.post('/complaints', (req, res) => {
  const complaint = { id: complaints.length + 1, ...req.body, status: 'Pending' };
  complaints.push(complaint);
  res.status(201).json(complaint);
});

app.get('/complaints', (req, res) => {
  res.json(complaints);
});

app.listen(3000, () => {
  console.log('ResolveNow backend running on http://localhost:3000');
});

 frontend/index.html
html
<!DOCTYPE html>
<html>
<head>
  <title>ResolveNow</title>
  <link rel="stylesheet" href="style.css" />
</head>
<body>
  <h1>ResolveNow: File a Complaint</h1>
  <form id="complaintForm">
    <input type="text" id="name" placeholder="Your Name" required />
    <textarea id="issue" placeholder="Describe your complaint" required></textarea>
    <button type="submit">Submit</button>
  </form>

  <script>
    document.getElementById('complaintForm').addEventListener('submit', async (e) => {
      e.preventDefault();
      const response = await fetch('http://localhost:3000/complaints', {
        method: 'POST',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify({
          name: document.getElementById('name').value,
          issue: document.getElementById('issue').value
        })
      });
      const data = await response.json();
      alert('Complaint submitted! ID: ' + data.id);
    });
  </script>
</body>
</html>

 frontend/style.css
css
body {
  font-family: Arial, sans-serif;
  margin: 40px;
  padding: 0;
  background-color: #f4f4f4;
}
form {
  background: white;
  padding: 20px;
  border-radius: 8px;
  max-width: 400px;
}
input, textarea {
  width: 100%;
  padding: 10px;
  margin: 10px 0;
}
button {
  background-color: #0a84ff;
  color: white;
  border: none;
  padding: 10px 20px;
}

 database/schema.sql
sql
CREATE TABLE complaints (
  id SERIAL PRIMARY KEY,
  name VARCHAR(100),
  issue TEXT NOT NULL,
  status VARCHAR(50) DEFAULT 'Pending',
  created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

 data/sample_complaints.json
json
[
  {
    "id": 1,
    "name": "Alice",
    "issue": "Streetlight not working in Block C",
    "status": "Pending"
  },
  {
    "id": 2,
    "name": "Bob",
    "issue": "Water leakage in apartment 302",
    "status": "Resolved"
  }
]
