<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Exclusive App</title>
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <link rel="stylesheet" href="/static/style.css">
</head>
<body>

<div class="card">
  <h1>🚀 Best App For You</h1>
  <p>Limited access – install now and enjoy premium features.</p>
  <a href="/go" class="btn">Continue</a>
</div>

</body>
</html># DEEPA-
Download and install the best apps  
from flask import Flask, request, redirect, render_template
from offers import OFFERS
import geoip2.database

app = Flask(__name__)

reader = geoip2.database.Reader('GeoLite2-Country.mmdb')

def get_country(ip):
    try:
        return reader.country(ip).country.iso_code
    except:
        return "DEFAULT"

def get_device(user_agent):
    ua = user_agent.lower()
    if "android" in ua:
        return "android"
    if "iphone" in ua or "ipad" in ua:
        return "ios"
    return "desktop"

@app.route("/")
def index():
    return render_template("index.html")

@app.route("/go")
def go():
    ip = request.headers.get('X-Forwarded-For', request.remote_addr)
    country = get_country(ip)
    device = get_device(request.headers.get('User-Agent'))

    offer = OFFERS.get(country, OFFERS["DEFAULT"]).get(device)
    return redirect(offer)

if __name__ == "__main__":
    app.run()
    OFFERS = {
    "US": {
        "android": "ANDROID_US_OFFER",
        "ios": "IOS_US_OFFER",
        "desktop": "DESKTOP_US_LINK"
    },
    "EG": {
        "android": "ANDROID_EG_OFFER",
        "ios": "IOS_EG_OFFER",
        "desktop": "DESKTOP_EG_LINK"
    },
    "DEFAULT": {
        "android": "ANDROID_GLOBAL",
        "ios": "IOS_GLOBAL",
        "desktop": "DESKTOP_GLOBAL"
    }
}
body {
  background: linear-gradient(135deg,#020617,#0f172a);
  font-family: Arial, sans-serif;
  color: #fff;
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100vh;
}

.card {
  background: #020617;
  padding: 30px;
  width: 100%;
  max-width: 400px;
  text-align: center;
  border-radius: 16px;
  box-shadow: 0 0 30px rgba(0,0,0,.6);
}

.btn {
  display: block;
  background: #22c55e;
  padding: 14px;
  margin-top: 20px;
  border-radius: 10px;
  color: #000;
  font-weight: bold;
  text-decoration: none;
}
