# 🤖 MOCO AI Assistant

MOCO is a simple offline/online Python voice assistant that can detect system info, tell jokes, switch voices, and search online when connected to the internet.

---

## 🚀 Features
- Responds to voice/text commands  
- Detects OS, CPU, and RAM  
- Switches between male and female voices  
- Works in **offline** and **online** modes  
- Opens web searches automatically  

---

## 🧠 Run the Project

1. Make sure you have Python 3 installed.  
2. Install dependencies:
   ```bash
   pip install pyttsx3 requests psutil
[moco.py](https://github.com/user-attachments/files/23433819/moco.py)
import os
import platform
import pyttsx3
import datetime
import requests
import psutil
import webbrowser

# Initialize text-to-speech engine
engine = pyttsx3.init()
voices = engine.getProperty('voices')
current_voice = 'female'

def set_voice(gender='female'):
    """Set the chatbot voice to male or female."""
    global current_voice
    for voice in voices:
        if gender.lower() == 'female' and 'zira' in voice.name.lower():
            engine.setProperty('voice', voice.id)
            current_voice = 'female'
            return
        elif gender.lower() == 'male' and 'david' in voice.name.lower():
            engine.setProperty('voice', voice.id)
            current_voice = 'male'
            return

# Default voice
set_voice('female')

def speak(text):
    """Speak and print the given text."""
    print(f"MOCO: {text}")
    engine.say(text)
    engine.runAndWait()

# System and AI state
ai_name = "MOCO"
version = "v1.6.5"
mode = "offline"

def detect_internet():
    """Check if internet connection is available."""
    try:
        requests.get("https://www.google.com", timeout=3)
        return True
    except:
        return False

def toggle_mode(command):
    """Switch between online and offline modes."""
    global mode
    if "go online" in command.lower():
        if detect_internet():
            mode = "online"
            speak("Online mode activated.")
        else:
            speak("Internet not available.")
    elif "go offline" in command.lower():
        mode = "offline"
        speak("Offline mode activated.")

def detect_system():
    """Detect and return system information."""
    system_info = {
        "OS": platform.system(),
        "CPU": platform.processor(),
        "RAM": f"{round(psutil.virtual_memory().total / (1024 ** 3))}GB"
    }
    return system_info

def handle_command(command):
    """Process and respond to user commands."""
    toggle_mode(command)

    if "how are you" in command.lower():
        speak("I'm fully operational.")
    elif "detect hardware" in command.lower():
        info = detect_system()
        speak(f"OS: {info['OS']}, CPU: {info['CPU']}, RAM: {info['RAM']}")
    elif "change voice to female" in command.lower():
        set_voice('female')
        speak("Voice changed to female.")
    elif "change voice to male" in command.lower():
        set_voice('male')
        speak("Voice changed to male.")
    elif "tell me a joke" in command.lower():
        speak("Why don’t scientists trust atoms? Because they make up everything!")
    elif "search" in command.lower() and mode == "online":
        query = command.lower().replace("search", "").strip()
        speak(f"Searching for {query} online.")
        webbrowser.open(f"https://www.google.com/search?q={query}")
    elif "status" in command.lower():
        speak(f"I am {ai_name} {version}. Current mode is {mode}.")
    else:
        speak(f"You said: {command}")

def main():
    """Main chatbot loop."""
    speak(f"Hello, I am {ai_name} {version}")
    while True:
        try:
            command = input("You: ")
            if command.lower() in ["exit", "quit"]:
                speak("Goodbye.")
                break
            elif "hey moco" in command.lower():
                speak("Yes?")
                continue
            handle_command(command)
        except KeyboardInterrupt:
            break

if __name__ == "__main__":
    main()
<img width="1185" height="604" alt="Screenshot 2025-11-08 235932" src="https://github.com/user-attachments/assets/42f611a3-0533-40d3-ba2f-2432af146c3b" />
