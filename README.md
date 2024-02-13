import speech_recognition as sr
import pyttsx3
import pywhatkit
import datetime
import wikipedia
import pyjokes

listener = sr.Recognizer()
engine = pyttsx3.init()

voices = engine.getProperty('voices')
engine.setProperty('voice', voices[0].id)
engine.setProperty('rate', 180)

def talk(text):
    engine.say(text)
    engine.runAndWait()

def take_command():
    try:
        with sr.Microphone() as source:
            # r.energy_threshold = 10000
            # r.adjust_for_ambient_noise(source, 2.5)
            print('listening...')

            voice = listener.listen(source)
            command = listener.recognize_google(voice)
            command = command.lower()
            if 'jarvis' in command:
                command = command.replace('jarvis', '')
                print(command)
    except:
        pass
    return command

def run_jarvis():
    command = take_command()
    print(command)
    if 'play' in command:
        song = command.replace('play', '')
        talk('playing' + song)
        pywhatkit.playonyt(song)
    elif 'time' in command:
        time = datetime.datetime.now().strftime('%H:%M')
        print(time)
        talk('current time is ' + time)

    elif 'search' in command:
        person = command.replace('search', '')
        info = wikipedia.summary(person, 2)
        print(info)
        talk(info)

    elif 'date' in command:
        talk('sorry, I am a robot')
    elif 'are you single' in command:
        talk('I am in relationship with wifi')
    else:
        talk('please say the command again.')



while True:
    run_jarvis()
