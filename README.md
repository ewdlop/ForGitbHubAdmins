# ForGitbHubAdmins

```python
import requests

GITHUB_API_URL = "https://api.github.com"
GITHUB_TOKEN = "your_github_token_here"

def get_sponsored_developers():
    headers = {
        'Authorization': f'token {GITHUB_TOKEN}',
        'Accept': 'application/vnd.github.v3+json',
    }
    
    query = """
    {
      search(query: "is:sponsored", type: USER, first: 100) {
        nodes {
          ... on User {
            login
            name
            bio
            location
            email
            websiteUrl
            company
          }
        }
      }
    }
    """

    response = requests.post(f"{GITHUB_API_URL}/graphql", json={'query': query}, headers=headers)
    
    if response.status_code == 200:
        return response.json()["data"]["search"]["nodes"]
    else:
        raise Exception(f"Query failed to run by returning code of {response.status_code}. {response.json()}")

if __name__ == "__main__":
    developers = get_sponsored_developers()
    for dev in developers:
        print(f"Username: {dev['login']}")
        print(f"Name: {dev.get('name')}")
        print(f"Bio: {dev.get('bio')}")
        print(f"Location: {dev.get('location')}")
        print(f"Email: {dev.get('email')}")
        print(f"Website: {dev.get('websiteUrl')}")
        print(f"Company: {dev.get('company')}")
        print("-" * 20)
```
