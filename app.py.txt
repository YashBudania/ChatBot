from flask import Flask, request, jsonify, render_template
import json
import datetime

app = Flask(__name__)

# Load chatbot answers
with open("data.json", "r") as f:
    data = json.load(f)

@app.route("/")
def home():
    return render_template("index.html")

@app.route("/chat", methods=["POST"])
def chat():
    msg = request.json["message"].lower()

    if "time" in msg:
        return jsonify({"reply": datetime.datetime.now().strftime("Current time is %H:%M:%S")})

    if "date" in msg:
        return jsonify({"reply": datetime.datetime.now().strftime("Today is %d %B %Y")})

    for key in data:
        if key in msg:
            return jsonify({"reply": data[key]})

    return jsonify({"reply": "Sorry, I don’t understand that."})

app.run()
