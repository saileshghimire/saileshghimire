- 👋 Hi, I’m @saileshghimire
- 👀 I’m interested in python
- 🌱 I’m currently learning django
- 💞️ I’m looking to collaborate on django and tkinter
- 📫 How to reach me 
- saileshghimire1@gmail.com
- insta:ghimire.shailesh

<!---
saileshghimire/saileshghimire is a ✨ special ✨ repository because its `README.md` (this file) appears on your GitHub profile.
You can click the Preview link to take a look at your changes.
--->
[![GitHub Streak](https://streak-stats.demolab.com/?user=saileshghimire&theme=tokyonight)](https://git.io/streak-stats)


import requests
import json

def get_most_used_language(username, token):
    # GitHub GraphQL API endpoint
    url = "https://api.github.com/graphql"

    # GraphQL query to fetch user's repositories and language statistics
    query = '''
    query ($username: String!) {
      user(login: $username) {
        repositories(first: 100, privacy: PUBLIC) {
          edges {
            node {
              primaryLanguage {
                name
              }
            }
          }
        }
      }
    }
    '''

    headers = {
        "Authorization": f"Bearer {token}"
    }

    # Send the request with the GraphQL query
    response = requests.post(url, headers=headers, json={"query": query, "variables": {"username": username}})

    if response.status_code == 200:
        data = response.json()
        language_count = {}

        # Count the occurrences of each language in the repositories
        for edge in data["data"]["user"]["repositories"]["edges"]:
            language = edge["node"]["primaryLanguage"]
            if language:
                language_name = language["name"]
                if language_name in language_count:
                    language_count[language_name] += 1
                else:
                    language_count[language_name] = 1

        # Find the most used language
        most_used_language = max(language_count, key=language_count.get)
        return most_used_language

    else:
        # Handle API error
        print(f"Failed to fetch repositories for user {username}. Error: {response.status_code}")

# Usage example
username = "your_github_username"
token = "your_github_personal_access_token"
most_used_language = get_most_used_language(username, token)
print(f"The most used language in {username}'s profile is: {most_used_language}")
