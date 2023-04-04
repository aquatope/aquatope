# Aquatope
Scholar project | Main code

# Modules 
* pyttsx3


## pyttsx3
```py
engine = pyttsx3.init()

fr_voice = moteur.getProperty('voices')[0]
engine.setProperty('voice', fr_voice.id)

sentence = ""


engine.say(phrase)
engine.runAndWait()
```
