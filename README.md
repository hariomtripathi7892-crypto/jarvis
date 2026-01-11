import openai
import pyttsx3
import speech_recognition as sr
import webbrowser
import os

openai.api_key = "YOUR_OPENAI_API_KEY"

def listen():
    r = sr.Recognizer()
    with sr.Microphone() as source:
        print("Listening...")
        audio = r.listen(source)
    try:
        return r.recognize_google(audio)
    except Exception as e:
        print("Error:", e)
        return ""

def speak(text):
    engine = pyttsx3.init()
    engine.say(text)
    engine.runAndWait()

def ask_gpt(prompt):
    response = openai.ChatCompletion.create(
        model="gpt-4",
        messages=[{"role": "user", "content": prompt}]
    )
    return response['choices'][0]['message']['content']

def execute_command(command):
    if "open youtube" in command:
        webbrowser.open("https://youtube.com")
        speak("Opening YouTube.")
    elif "play music" in command:
        os.system("open /Applications/iTunes.app") # platform-specific
        speak("Playing music.")
    else:
        result = ask_gpt(command)
        speak(result)

def main():
    while True:
        command = listen()
        if command:
            if "stop" in command or "quit" in command:
                speak("Goodbye!")
                break
            execute_command(command)

if __name__ == "__main__":
    main()# jarvis
