# Phase 5: Development

**Objective**:
Use AI tools to assist in code generation for key components required for implementing real-time notifications in the app.

**Approach**:
AI can aid in generating code snippets, boilerplate code, and templates for various components. This phase will focus on using the user story and acceptance criteria to generate the necessary code for the notification system.

**Expectations**:
- **Code Quality**: The generated code should be clean, efficient, and follow best practices.
- **Completeness**: Ensure that all key components of the notification system are covered, including backend services, database schemas, and frontend integration.
- **Customization**: Be prepared to modify and enhance the generated code to fit the specific project requirements.

## Steps

1. **Select a User Story and Acceptance Criteria**:
   - Example: "As a casino player, I want to receive notifications about the Slot Showdown promotion so that I can participate in the competition and have a chance to win exciting prizes while playing my favorite slots."
   - Acceptance Criteria:
     1. The notification about the Slot Showdown promotion should be sent to registered casino players every hour during the duration of the competition.
     2. The notification should include details about the promotion, such as the name "Slot Showdown," the opportunity to win exciting prizes, and the requirement to play favorite slots to participate.
     3. The notification should provide a clear call-to-action, encouraging players to join the competition and start playing their favorite slots.
     4. Players should be able to opt-out of receiving notifications about the Slot Showdown promotion.

2. **Generate Backend Service Code**:
   - Use AI tools like GitHub Copilot or Tabnine to generate backend code for the notification service.
   - Example: Python code for sending notifications using a scheduler.

3. **Generate Database Schema**:
   - Use AI tools to generate the database schema for storing notification data.
   - Example: SQL schema for a table to store notification details.

4. **Generate Frontend Integration Code**:
   - Use AI tools to generate frontend code for displaying notifications in the app.
   - Example: JavaScript/React code for showing push notifications.

## Example Code Generation

**User Story**:
"As a casino player, I want to receive notifications about the Slot Showdown promotion so that I can participate in the competition and have a chance to win exciting prizes while playing my favorite slots."

**Acceptance Criteria**:
1. The notification about the Slot Showdown promotion should be sent to registered casino players every hour during the duration of the competition.
2. The notification should include details about the promotion, such as the name "Slot Showdown," the opportunity to win exciting prizes, and the requirement to play favorite slots to participate.
3. The notification should provide a clear call-to-action, encouraging players to join the competition and start playing their favorite slots.
4. Players should be able to opt-out of receiving notifications about the Slot Showdown promotion.

### Backend and Front Service Code Example
```python
import os
import subprocess

# Define the user story and acceptance criteria
user_story = """
**User Story**:
As a casino player, I want to receive notifications about the Slot Showdown promotion so that I can participate in the competition and have a chance to win exciting prizes while playing my favorite slots.

**Acceptance Criteria**:
1. The notification about the Slot Showdown promotion should be sent to registered casino players every hour during the duration of the competition.
2. The notification should include details about the promotion, such as the name "Slot Showdown," the opportunity to win exciting prizes, and the requirement to play favorite slots to participate.
3. The notification should provide a clear call-to-action, encouraging players to join the competition and start playing their favorite slots.
4. Players should be able to opt-out of receiving notifications about the Slot Showdown promotion.
"""

# Extract details from user_story for generating code
promotion_name = "Slot Showdown"
promotion_details = "the opportunity to win exciting prizes while playing your favorite slots"
call_to_action = "Join the competition and start playing your favorite slots now!"
opt_out_message = "If you do not wish to receive these notifications, you can opt-out."

# Define project structure
project_name = "ai-sdlc-phase5"
backend_name = "backend-ai-sdlc-phase5"
frontend_name = "frontend-ai-sdlc-phase5"

dirs = [
    project_name,
    f"{project_name}/{backend_name}",
    f"{project_name}/{frontend_name}",
    f"{project_name}/{backend_name}/templates",
    f"{project_name}/{backend_name}/static",
    f"{project_name}/{frontend_name}/src",
    f"{project_name}/{frontend_name}/public"
]

# Create directories
for dir in dirs:
    os.makedirs(dir, exist_ok=True)

# Backend Code
backend_code = f"""
from flask import Flask, request, jsonify
from flask_sqlalchemy import SQLAlchemy
from apscheduler.schedulers.background import BackgroundScheduler
import time

app = Flask(__name__)
app.config['SQLALCHEMY_DATABASE_URI'] = 'sqlite:///notifications.db'
db = SQLAlchemy(app)

class Notification(db.Model):
    id = db.Column(db.Integer, primary_key=True)
    user_id = db.Column(db.Integer, nullable=False)
    title = db.Column(db.String(255), nullable=False)
    message = db.Column(db.Text, nullable=False)
    sent_at = db.Column(db.DateTime, default=db.func.current_timestamp())
    opt_out = db.Column(db.Boolean, default=False)

@app.route('/notify', methods=['POST'])
def notify():
    user_id = request.json['user_id']
    title = "{promotion_name} Promotion"
    message = request.json['message'] + "\\n\\n{promotion_details}\\n{call_to_action}\\n{opt_out_message}"
    notification = Notification(user_id=user_id, title=title, message=message)
    db.session.add(notification)
    db.session.commit()
    return jsonify({{'message': 'Notification sent'}}), 201

@app.route('/opt-out', methods=['POST'])
def opt_out():
    user_id = request.json['user_id']
    notifications = Notification.query.filter_by(user_id=user_id).all()
    for notification in notifications:
        notification.opt_out = True
    db.session.commit()
    return jsonify({{'message': 'You have successfully opted out of notifications.'}}), 200

def send_notification():
    notifications = Notification.query.filter_by(opt_out=False).all()
    for notification in notifications:
        print(f"Sending {{notification.title}} to user {{notification.user_id}}")

scheduler = BackgroundScheduler()
scheduler.add_job(send_notification, 'interval', hours=1)
scheduler.start()

if __name__ == '__main__':
    db.create_all()
    app.run(host='0.0.0.0', port=5000)
"""

with open(f"{project_name}/{backend_name}/app.py", "w") as f:
    f.write(backend_code)

# Frontend Code
frontend_code = """
import React from 'react';
import { ToastContainer, toast } from 'react-toastify';
import 'react-toastify/dist/ReactToastify.css';

const notify = () => {
    toast.info("Slot Showdown: Participate now to win exciting prizes!");
};

const App = () => (
    <div>
        <button onClick={notify}>Show Notification</button>
        <ToastContainer />
    </div>
);

export default App;
"""

index_js = """
import React from 'react';
import ReactDOM from 'react-dom';
import App from './App';
import './index.css';

ReactDOM.render(
  <React.StrictMode>
    <App />
  </React.StrictMode>,
  document.getElementById('root')
);
"""

package_json = """
{
  "name": "frontend",
  "version": "1.0.0",
  "main": "index.js",
  "dependencies": {
    "react": "^17.0.2",
    "react-dom": "^17.0.2",
    "react-scripts": "4.0.3",
    "react-toastify": "^8.0.3"
  },
  "scripts": {
    "start": "react-scripts start",
    "build": "react-scripts build",
    "test": "react-scripts test",
    "eject": "react-scripts eject"
  }
}
"""

index_html = """
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>AI SDLC Phase 5 Frontend</title>
</head>
<body>
  <div id="root"></div>
</body>
</html>
"""

with open(f"{project_name}/{frontend_name}/src/App.js", "w") as f:
    f.write(frontend_code)

with open(f"{project_name}/{frontend_name}/src/index.js", "w") as f:
    f.write(index_js)

with open(f"{project_name}/{frontend_name}/public/index.html", "w") as f:
    f.write(index_html)

with open(f"{project_name}/{frontend_name}/package.json", "w") as f):
    f.write(package_json)

print("Project structure and code created.")

# Function to initialize, pull, add, commit, and push the repository
def git_operations(repo_path, repo_url):
    if not os.path.exists(os.path.join(repo_path, ".git")):
        subprocess.run(["git", "init"], cwd=repo_path)
        subprocess.run(["git", "remote", "add", "origin", repo_url], cwd=repo_path)
    
    subprocess.run(["git", "pull", "origin", "main"], cwd=repo_path)
    subprocess.run(["git", "add", "."], cwd=repo_path)
    subprocess.run(["git", "commit", "-m", "Update project files"], cwd=repo_path)
    subprocess.run(["git", "push", "-u", "origin", "main"], cwd=repo_path)

# Initialize and push the backend repository
git_operations(f"{project_name}/{backend_name}", "https://github.com/jeovahfialho/backend-ai-sdlc-phase5.git")

# Initialize and push the frontend repository
git_operations(f"{project_name}/{frontend_name}", "https://github.com/jeovahfialho/frontend-ai-sdlc-phase5.git")

print("Project pushed to GitHub.")
```

### Database Schema Example
```sql
CREATE TABLE notifications (
    id SERIAL PRIMARY KEY,
    user_id INT NOT NULL,
    title VARCHAR(255) NOT NULL,
    message TEXT NOT NULL,
    sent_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    FOREIGN KEY (user_id) REFERENCES users(id)
);
```
project's requirements.
