# Aquatope
Scholar project | Main code

# Modules 
* pyttsx3
* SpeechRecognition
* pyaudio
* rustylib (by ZeyaTsu)


## pyttsx3
```py
engine = pyttsx3.init()

fr_voice = moteur.getProperty('voices')[0]
engine.setProperty('voice', fr_voice.id)

sentence = ""


engine.say(phrase)
engine.runAndWait()
```

```py
import speech_recognition as sr
import pyttsx3

reco = sr.Recognizer()

while True:
    try:
        with sr.Microphone() as mic:
            reco.adjust_for_ambient_noise(mic, duration=0.2)
            audio = reco.listen(mic)

            text = reco.recognize_google(audio, language="fr-FR")
            text = text.lower()

            print(f"{text}")

    except Exception as e:
        print(e)
```
