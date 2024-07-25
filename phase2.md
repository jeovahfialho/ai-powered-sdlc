# Phase 2: Requirements Gathering

## Objective:
Generate user stories and acceptance criteria based on the ideas generated in the Concept Ideation phase.

## Demo:
Utilizing an AI tool to automatically generate requirement documents from the brainstorming ideas, including user stories and acceptance criteria.

## Implementation Steps

1. **AI Requirement Generation Tool**:
   - Use an NLP model to convert the ideas generated in Phase 1 into detailed user stories and acceptance criteria.
   - Parse the text ideas and transform them into structured requirements.

2. **Practical Example**:
   - Develop a script that uses GPT-4 to generate user stories and acceptance criteria based on the notification ideas from Phase 1.

### Example Implementation

Let's create a script in Python that uses GPT-4 to generate user stories and acceptance criteria based on the previously generated notification ideas.

#### Example Script:

```python
import requests
from requests.auth import HTTPBasicAuth
import json
import openai

# Configure the OpenAI API key
openai.api_key = ""

# JIRA configuration
JIRA_DOMAIN = "jeovahfialho.atlassian.net"
JIRA_EMAIL = "jeovahfialho@gmail.com"
JIRA_API_TOKEN = ""
JIRA_PROJECT_KEY = "KAN"
ISSUE_TYPE_ID = "10001"  # Substitua pelo ID correto do tipo de issue "Story"

# Headers for JIRA API
headers = {
    "Accept": "application/json",
    "Content-Type": "application/json"
}

def create_jira_issue(summary, description):
    url = f"https://{JIRA_DOMAIN}/rest/api/3/issue"
    auth = HTTPBasicAuth(JIRA_EMAIL, JIRA_API_TOKEN)
    payload = json.dumps({
        "fields": {
            "project": {
                "key": JIRA_PROJECT_KEY
            },
            "summary": summary,
            "description": {
                "type": "doc",
                "version": 1,
                "content": [
                    {
                        "type": "paragraph",
                        "content": [
                            {
                                "type": "text",
                                "text": description
                            }
                        ]
                    }
                ]
            },
            "issuetype": {
                "id": ISSUE_TYPE_ID
            }
        }
    })

    response = requests.post(url, data=payload, headers=headers, auth=auth)
    if response.status_code == 201:
        print(f"Issue created successfully: {response.json()['key']}")
    else:
        print(f"Failed to create issue: {response.text}")

def generate_user_stories_and_criteria(notification_ideas):
    stories_and_criteria = []
    
    for idea in notification_ideas:
        response = openai.ChatCompletion.create(
            model="gpt-3.5-turbo",
            messages=[
                {"role": "system", "content": "You are a helpful assistant."},
                {"role": "user", "content": f"Generate a user story and acceptance criteria for the following casino notification idea: {idea}"}
            ],
            max_tokens=150,
            n=1,
            temperature=0.7,
        )
        stories_and_criteria.append(response.choices[0]['message']['content'].strip())
    
    return stories_and_criteria

# Example input from Phase 1
notification_ideas = [
    "Get ready to spin and win with our exclusive Slot Showdown promotion! Play your favorite slots and compete for a chance to win exciting prizes every hour.",
    "Join us for Happy Hour at the casino and enjoy special promotions on drinks and games all night long! Cheers to winning big!",
    "Feeling lucky? Test your skills in our Blackjack Blitz promotion and take on the dealer for a chance to win cash prizes and bonus chips."
]

stories_and_criteria = generate_user_stories_and_criteria(notification_ideas)

for i, sc in enumerate(stories_and_criteria):
    summary = f"User Story {i+1}: Slot Showdown Promotion"
    description = sc
    create_jira_issue(summary, description)

print("Generated User Stories and Acceptance Criteria:")
for i, sc in enumerate(stories_and_criteria):
    print(f"{i+1}. {sc}")
```

## One of the Results from de Function

**User Story:**

As a casino player, I want to receive notifications about the Slot Showdown promotion so that I can participate in the competition and have a chance to win exciting prizes while playing my favorite slots.

**Acceptance Criteria:**
1. The notification about the Slot Showdown promotion should be sent to registered casino players every hour during the duration of the competition.
2. The notification should include details about the promotion, such as the name "Slot Showdown," the opportunity to win exciting prizes, and the requirement to play favorite slots to participate.
3. The notification should provide a clear call-to-action, encouraging players to join the competition and start playing their favorite slots.
4. Players should be able to opt-out of receiving notifications about the Slot Showdown promotion.
