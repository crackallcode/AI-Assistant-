import os
import time
import pyaudio
import speech_recognition as sr
import playsound 
from gtts import gTTS
import openai
import pyautogui
import pytesseract
from PIL import Image 

api_key = "CHANGE THIS"
lang = 'en'
openai.api_key = api_key

guy = ""

microphone = sr.Microphone(device_index=1)

def play_audio(text):
    speech = gTTS(text=text, lang=lang, slow=False, tld="com.au")
    speech.save("output.mp3")
    playsound.playsound("output.mp3")

def create_note_file(note, file_path):
    with open(file_path, "a") as f:
        f.write(note + "\n")

def add_note_again(note, file_path):
    with open(file_path, "a") as f:
        f.write(note + "\n")

def get_audio():
    r = sr.Recognizer()
    with microphone as source:
        audio = r.listen(source)
        said = ""

        try:
            said = r.recognize_google(audio)
            print(said)
            global guy
            guy = said

            if "note" in said:
                print("Opening note")
                play_audio("What would you like to make a note about")
                note_audio = r.listen(source)
                note = r.recognize_google(note_audio)
                print("Note saved")
                play_audio("Note saved successfully.")
                file_path = os.path.expanduser("~/Desktop/note.txt")
                create_note_file(note, file_path)

                while True:
                    play_audio("Would you like to save another note?")
                    another_note_audio = r.listen(source)
                    response = r.recognize_google(another_note_audio)
                    if "yes" in response:
                        play_audio("What would you like to add to the notes")
                        note_audio = r.listen(source)
                        note = r.recognize_google(note_audio)
                        add_note_again(note, file_path)
                        play_audio("The note was saved again")
                    else:
                        break

            elif "suck" in said:
                play_audio("I am sorry you are frustrated")

            elif "Friday" in said:
                new_string = said.replace("Friday", "")
                print(new_string)
                completion = openai.ChatCompletion.create(model="gpt-3.5-turbo", messages=[{"role": "user", "content": new_string}])
                text = completion.choices[0].message.content
                play_audio(text)

            elif "screenshot" in said:
                print("Getting the screenshot")
                screenshot_dir = os.path.expanduser("~/Desktop")
                file_name = "screenshot"
                extension = ".png"
                file_path = os.path.join(screenshot_dir, file_name + extension)

                if os.path.exists(file_path):
                    counter = 1
                    while os.path.exists(file_path):
                        new_filename = file_name + str(counter) + extension
                        file_path = os.path.join(screenshot_dir, new_filename)
                        counter += 1
                
                capture_screenshot(file_path)
                play_audio("Screenshot was saved")


        except Exception as e:
            print("Exception:", str(e))

def capture_screenshot(file_path):
    screenshot = pyautogui.screenshot()
    screenshot.save(file_path)

while True:
    if "stop" in guy:
        break
    get_audio()
