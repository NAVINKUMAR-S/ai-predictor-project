# ai-predictor-project

# PROJECT TITLE: AI-BASED WEBSITE LOAD PREDICTOR & SMART AUTO-SCALING SYSTEM

## THEORY:

### 1️⃣ Introduction:

Good morning everyone,

In our college, all students use a single website for:

* Study materials

* Assignment submissions

* Skill assessments

* Online exams

During peak times, especially exams, many students log in simultaneously.
This causes the server to slow down or even crash.

So the problem we identified is:

Server overload during peak academic usage.

### 🚨 2️⃣ Problem Statement:

Let us understand the technical issue.

Suppose:

* Server capacity = 500 users

* During exam time = 800 students login

Since 800 > 500,

The server gets overloaded.

This results in:

* Website crashes

* Students unable to submit exams

* Increased stress

* Possible data loss

The main issue is:

The system reacts only after overload happens.
It does not predict peak traffic beforehand.

### 🧠 3️⃣ Our Solution (AI-Based Approach)

To solve this, we built:

#### AI-Based Website Load Predictor

Our system:

1.Takes inputs like:

   * Exam time

   * AM/PM

   * Is exam time or not

   * Day of the week

2.Uses a Machine Learning model (Regression Model)

3.Predicts expected number of users

4.Compares predicted users with server capacity

If:

Predicted users > Server capacity

Then:

⚠ Overload Risk
🚀 Auto Scaling Activated

If:

Predicted users ≤ Server capacity

Then:

✅ Server Safe

## 🖥 4️⃣ Demo Explanation :

#### Case 1: Exam Time

Here, we selected:

* Time: 10 AM

* Is Exam Time: Yes

* Day: Monday

The AI predicted:

Predicted Users: 790
Server Capacity: 500

Since 790 > 500

System shows:

⚠ Overload Risk – Auto Scaling Activated

This means in real-world cloud systems, additional servers would be launched automatically to handle traffic.

#### Case 2: Normal Time

Now we selected:

* Time: 10 AM

* Is Exam Time: No

* Day: Monday

AI predicted:

Predicted Users: 86
Server Capacity: 500

Since 86 < 500

System shows:

✅ Server Safe

No scaling required.

This demonstrates intelligent decision-making.

## ⚙ 5️⃣ How Auto-Scaling Works in Real Time (40–50 seconds)

In real-world cloud platforms like:

* Amazon Web Services

* Google Cloud

* Microsoft Azure

When overload risk is predicted:

1.AI sends scaling signal

2.Cloud launches new server instances

3.Load balancer distributes traffic

4.System handles peak load smoothly

This prevents downtime during exams.

💰 6️⃣ Why Not Just Increase Capacity Permanently?

We considered that option.

Example:

Total students = 100
Set capacity = 200

Yes, it reduces crash risk.

But:

* It increases cost

* Resources remain unused most of the time

So instead of fixed high capacity,
We use:

AI + Dynamic Auto-Scaling

Which is:

* Cost-efficient

* Scalable

* Intelligent

## 🔐 7️⃣ Additional Improvements (Advanced Thinking)

We also identified other real-world issues:

* Database bottleneck

* DDoS attacks

* Load balancer failure

* Sudden unexpected spikes

Future improvements include:

* Caching

* Queue system

* Security layers

* Redundant load balancers

This makes the system production-ready.

## 🎯 8️⃣ Conclusion (30 seconds)

To conclude,

* Our project solves a real-world college problem using:

* Machine Learning prediction

* Intelligent load comparison

* Simulated auto-scaling logic

This ensures:

* Zero downtime during exams

* Better user experience

* Cost-efficient infrastructure


## PROGRAM:
app.py
```
from flask import Flask, render_template, request, jsonify
import pandas as pd
from sklearn.linear_model import LinearRegression
import numpy as np

app = Flask(__name__)

# -------------------------------
# STEP 1: Create Training Data
# -------------------------------

data = {
    "hour": [9,10,11,2,3,10,11,9,2,3,10,11],
    "is_exam": [0,1,1,0,0,1,1,0,0,0,1,1],
    "day": [1,1,1,2,2,3,3,4,4,4,5,5],
    "users": [120,800,750,200,180,820,780,110,210,190,850,790]
}

df = pd.DataFrame(data)

X = df[["hour", "is_exam", "day"]]
y = df["users"]

# -------------------------------
# STEP 2: Train Model
# -------------------------------

model = LinearRegression()
model.fit(X, y)

SERVER_CAPACITY = 500

# -------------------------------
# Routes
# -------------------------------

@app.route("/")
def home():
    return render_template("index.html")

@app.route("/predict", methods=["POST"])
def predict():
    hour = int(request.form["hour"])
    is_exam = int(request.form["is_exam"])
    day = int(request.form["day"])

    new_data = np.array([[hour, is_exam, day]])
    prediction = int(model.predict(new_data)[0])

    if prediction > SERVER_CAPACITY:
        status = "⚠ Overload Risk - Auto Scaling Activated"
    else:
        status = "✅ Server Safe"

    return jsonify({
        "predicted_users": prediction,
        "capacity": SERVER_CAPACITY,
        "status": status
    })

if __name__ == "__main__":
    app.run(debug=True)
```

index.html
```
<!DOCTYPE html>
<html lang="en" >
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1" />
  <title>AI-Based College Website Load Predictor</title>
  <style>
    /* Reset */
    *, *::before, *::after {
      box-sizing: border-box;
    }
    body {
      margin: 0;
      font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
      background: linear-gradient(135deg, #66cbea, #4b95a2);
      color: #2c2c2c;
      min-height: 100vh;
      display: flex;
      justify-content: center;
      align-items: center;
      padding: 20px;
    }
    .container {
      background: #fff;
      max-width: 480px;
      width: 100%;
      padding: 40px 50px;
      border-radius: 16px;
      box-shadow: 0 15px 30px rgba(0,0,0,0.12);
      text-align: center;
      position: relative;
      overflow: hidden;
    }
    h1 {
      font-weight: 900;
      font-size: 1.9rem;
      margin-bottom: 30px;
      color: #4b0082;
      user-select: none;
    }
    label {
      display: block;
      font-weight: 700;
      margin-bottom: 8px;
      text-align: left;
      font-size: 0.95rem;
      color: #4a4a4a;
    }
    .input-group {
      position: relative;
      margin-bottom: 20px;
    }
    input[type="number"], select {
      width: 100%;
      padding: 12px 45px 12px 15px;
      font-size: 1rem;
      border-radius: 8px;
      border: 2px solid #ddd;
      transition: border-color 0.3s ease;
      box-shadow: inset 0 2px 5px rgba(0,0,0,0.05);
      font-weight: 600;
      color: #333;
    }
    input[type="number"]:focus, select:focus {
      outline: none;
      border-color: #7b5cff;
      box-shadow: 0 0 10px rgba(123,92,255,0.4);
    }
    /* Icons inside inputs */
    .input-group svg {
      position: absolute;
      right: 15px;
      top: 50%;
      transform: translateY(-50%);
      width: 20px;
      height: 20px;
      fill: #aaa;
      pointer-events: none;
    }

    button {
      width: 100%;
      padding: 16px 0;
      font-weight: 800;
      font-size: 1.15rem;
      background: linear-gradient(135deg, #7b5cff, #4c30ff);
      color: white;
      border: none;
      border-radius: 12px;
      cursor: pointer;
      box-shadow: 0 8px 20px rgba(123,92,255,0.4);
      transition: background 0.4s ease, box-shadow 0.4s ease;
      user-select: none;
    }
    button:hover {
      background: linear-gradient(135deg, #624de8, #3a1fff);
      box-shadow: 0 12px 30px rgba(97,77,232,0.6);
    }
    button:active {
      transform: scale(0.97);
    }

    #result {
      margin-top: 35px;
      font-weight: 700;
      font-size: 1.3rem;
      user-select: text;
      min-height: 90px;
      color: #444;
      line-height: 1.4;
    }
    #result .safe {
      color: #16a34a;
    }
    #result .overload {
      color: #dc2626;
    }

    /* Responsive */
    @media (max-width: 500px) {
      .container {
        padding: 30px 25px;
      }
    }
  </style>
</head>
<body>
  <main class="container" role="main" aria-label="Website Load Predictor">
    <h1>🎓 Website Load Predictor</h1>

    <div class="input-group">
      <label for="hour12">Exam Time (12-hour format):</label>
      <input
        id="hour12"
        type="number"
        min="1"
        max="12"
        value="3"
        aria-describedby="hour12-desc"
        required
      />
      <svg aria-hidden="true" viewBox="0 0 24 24"><path d="M12 4v8l5 5"/></svg>
    </div>
    <div class="input-group">
      <label for="ampm">AM/PM:</label>
      <select id="ampm" aria-describedby="ampm-desc" required>
        <option value="AM">AM</option>
        <option value="PM" selected>PM</option>
      </select>
      <svg aria-hidden="true" viewBox="0 0 24 24"><path d="M19 9l-7 7-7-7"/></svg>
    </div>
    <div class="input-group">
      <label for="exam">Is Exam Time?</label>
      <select id="exam" aria-describedby="exam-desc" required>
        <option value="0">No</option>
        <option value="1" selected>Yes</option>
      </select>
      <svg aria-hidden="true" viewBox="0 0 24 24"><path d="M19 9l-7 7-7-7"/></svg>
    </div>
    <div class="input-group">
      <label for="day">Day of the Week:</label>
      <select id="day" aria-describedby="day-desc" required>
        <option value="Mon" selected>Monday</option>
        <option value="Tue">Tuesday</option>
        <option value="Wed">Wednesday</option>
        <option value="Thu">Thursday</option>
        <option value="Fri">Friday</option>
        <option value="Sat">Saturday</option>
        <option value="Sun">Sunday</option>
      </select>
      <svg aria-hidden="true" viewBox="0 0 24 24"><path d="M19 9l-7 7-7-7"/></svg>
    </div>

    <button onclick="predictLoad()" aria-label="Predict website traffic">
      Predict Traffic
    </button>

    <section id="result" aria-live="polite"></section>
  </main>

  <script>
    // Convert 12h to 24h
    function convertTo24Hour(hour, ampm) {
      hour = parseInt(hour);
      if (ampm === 'PM' && hour !== 12) {
        hour += 12;
      }
      if (ampm === 'AM' && hour === 12) {
        hour = 0;
      }
      return hour;
    }

    async function predictLoad() {
      const hour12 = document.getElementById("hour12").value;
      const ampm = document.getElementById("ampm").value;
      const dayStr = document.getElementById("day").value;
      const is_exam = document.getElementById("exam").value;

      if (!hour12 || hour12 < 1 || hour12 > 12) {
        alert("Please enter a valid hour between 1 and 12.");
        return;
      }

      const hour = convertTo24Hour(hour12, ampm);

      // Map day string to number (Mon=1,... Sun=7)
      const dayMap = {
        'Mon': 1,
        'Tue': 2,
        'Wed': 3,
        'Thu': 4,
        'Fri': 5,
        'Sat': 6,
        'Sun': 7
      };
      const day = dayMap[dayStr];

      const formData = new FormData();
      formData.append("hour", hour);
      formData.append("is_exam", is_exam);
      formData.append("day", day);

      try {
        const response = await fetch("/predict", {
          method: "POST",
          body: formData
        });

        if (!response.ok) {
          throw new Error("Network response was not ok");
        }

        const data = await response.json();
        const resultDiv = document.getElementById("result");

        resultDiv.innerHTML = `
          Predicted Users: <strong>${data.predicted_users}</strong><br/>
          Server Capacity: <strong>${data.capacity}</strong><br/>
          <span class="${data.status.includes('Overload') ? 'overload' : 'safe'}" style="font-weight: 900; font-size: 1.3rem;">
            ${data.status.includes('Overload') ? '⚠ ' : '✔ '} ${data.status}
          </span>
        `;
      } catch (error) {
        alert("Error fetching prediction: " + error.message);
      }
    }
  </script>
</body>
</html>
```

## OUTPUT:

<img width="1919" height="1093" alt="image" src="https://github.com/user-attachments/assets/72b5c02d-3add-4527-8976-1a4f3c229c5c" />


<img width="1919" height="1087" alt="image" src="https://github.com/user-attachments/assets/27245ad6-327f-4935-a3b6-f7958fa3d9ed" />

