# Speech-to-text-and-text-to-speech-converter-and-translator
#translator
from googletrans import Translator

translator = Translator()

language = {"bn": "Bangla",
            "en": "English",
            "ko": "Korean",
            "fr": "French",
            "de": "German",
            "he": "Hebrew",
            "hi": "Hindi",
            "it": "Italian",
            "ja": "Japanese",
            'la': "Latin",
            "ms": "Malay",
            "ne": "Nepali",
            "ru": "Russian",
            "ar": "Arabic",
            "zh": "Chinese",
            "es": "Spanish"
            }

allow = True  # variable to control correct language code input

while allow:  # checking if language code is valid

    user_code = input(
        f"Please input desired language code. To see the language code list enter 'options' \n")

    if user_code == "options":  # showing language options
        print("Code : Language")  # Heading of language option menu
        for i in language.items():
            print(f"{i[0]} => {i[1]}")
        print()  # adding an empty space

    else:  # validating user input
        for lan_code in language.keys():
            if lan_code == user_code:
                print(f"You have selected {language[lan_code]}")
                allow = False
        if allow:
            print("It's not a valid language code!")

while True:  # starting translation loop
    string = input(
        "\nWrite the text you want to translate: \nTo exit the program write 'close'\n")

    if string == "close":  # exit program command
        print(f"\nHave a nice Day!")
        break

    # translating method from googletrans
    translated = translator.translate(string, dest=user_code)

    # printing translation
    print(f"\n{language[user_code]} translation: {translated.text}")
    # printing pronunciation
    print(f"Pronunciation : {translated.pronunciation}")

    for i in language.items():  # checking if the source language is listed on language dict and printing it
        if translated.src == i[0]:
            print(f"Translated from : {i[1]}")




#text to speech
import speech_recognition as sr
import googletrans
from gtts import gTTS
import os
import pyaudio

def recognize_speech():
    r = sr.Recognizer()
    with sr.Microphone() as source:
        print("Speak something:")
        audio = r.listen(source)
    try:
        text = r.recognize_google(audio, language="en-US")
        print("You said: " + text)
        return text
    except sr.UnknownValueError:
        print("Google Speech Recognition could not understand your audio")
        return None
    except sr.RequestError as e:
        print("Could not request results from Google Speech Recognition service; {0}".format(e))
        return None

def translate_text(text, language_code):
    if not language_code:
        raise ValueError("Language code is required")
    translator = googletrans.Translator()
    translated_text = translator.translate(text, dest=language_code)
    return translated_text.text

def text_to_speech(text, language_code):
    tts = gTTS(text=text, lang=language_code)
    tts.save("output.mp3")
    play_audio("output.mp3")

def play_audio(file_path):
    pyaudio.play(file_path)

def main():
    language_code = "fr"  # Example language code
    text = recognize_speech()
    if text:
        translated_text = translate_text(text, language_code)
        text_to_speech(translated_text, language_code)

if __name__ == "__main__":
    main()


    #speech to text

    import speech_recognition as sr
import googletrans
from gtts import gTTS
import os

def translate_speech_to_text(language_code):
    # Create a speech recognition object
    r = sr.Recognizer()

    # Use the microphone as the audio source
    with sr.Microphone() as source:
        print("Speak something:")
        audio = r.listen(source)

    # Try to recognize the speech
    try:
        text = r.recognize_google(audio, language="en-US")
        print("You said: " + text)

        # Translate the text to the chosen language
        translator = googletrans.Translator()
        translated_text = translator.translate(text, dest=language_code)
        print("Translated text: " + translated_text.text)

        # Convert the translated text to speech
        tts = gTTS(text=translated_text.text, lang=language_code)
        tts.save("output.mp3")
        os.system("mpg321 output.mp3")

    except sr.UnknownValueError:
        print("Google Speech Recognition could not understand your audio")
    except sr.RequestError as e:
        print("Could not request results from Google Speech Recognition service; {0}".format(e))

# Example usage
translate_speech_to_text("fr")  # Translate to French



