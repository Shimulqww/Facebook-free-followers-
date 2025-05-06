import time
from bs4 import BeautifulSoup

# Set your Freebitco.in session cookie here
user_session_cookie = "63e5f800d241a98953ea1dcf6a9e4901819ea71880f0c4c6f3e54f16786ff04f"

session = requests.Session()
session.cookies.set("user_session", user_session_cookie, domain="freebitco.in")
headers = {'User-Agent': 'Mozilla/5.0'}

def roll():
    url = "https://freebitco.in/"
    res = session.get(url, headers=headers)

    if "FREE BTC" not in res.text:
        print("[-] Session invalid. Check cookie.")
        return

    soup = BeautifulSoup(res.text, 'html.parser')
    csrf_token_tag = soup.find("input", {"name": "csrf_token"})

    if not csrf_token_tag:
        print("[-] Could not find CSRF token.")
        return

    csrf_token = csrf_token_tag["value"]
    print(f"[INFO] CSRF token: {csrf_token}")

    payload = {
        "op": "free_play",
        "csrf_token": csrf_token
    }

    roll_res = session.post(url, data=payload, headers=headers)

    if "You win" in roll_res.text:
        print("[+] Roll successful!")
    elif "Please wait" in roll_res.text or "next roll" in roll_res.text.lower():
        print("[!] Cooldown active. Try later.")
    else:
        print("[!] Roll failed. Response snippet:")
        print(roll_res.text[:500])

while True:
    try:
        roll()
    except Exception as e:
        print(f"[ERROR] {e}")
    print("Waiting 1 hour...")
    time.sleep(3600)
