import speech_recognition as sr
import pyttsx3
import datetime
import wikipedia

# Inicialização do reconhecimento de voz
def ouvir_microfone():
    r = sr.Recognizer()
    with sr.Microphone() as source:
        print("Ouvindo...")
        r.pause_threshold = 1
        audio = r.listen(source)
    try:
        print("Reconhecendo...")
        query = r.recognize_google(audio, language='pt-BR')
        print(f"Você disse: {query}\n")
        return query
    except Exception as e:
        print(e)
        return None

# Função para falar
def falar(texto):
    engine = pyttsx3.init()
    engine.say(texto)
    engine.runAndWait()

# Função principal
def main():
    falar("Olá! Como posso ajudar você hoje?")
    while True:
        comando = ouvir_microfone().lower()
        if comando is None:
            continue
        if 'hora' in comando:
            hora_atual = datetime.datetime.now().strftime("%H:%M:%S")
            falar(f"A hora atual é {hora_atual}")
        elif 'wikipedia' in comando:
            falar("O que você gostaria de buscar na Wikipedia?")
            pesquisa = ouvir_microfone()
            resultado = wikipedia.summary(pesquisa, sentences=2)
            falar("Aqui está o resultado da pesquisa na Wikipedia.")
            falar(resultado)
        elif 'parar' in comando:
            falar("Até logo!")
            break

if __name__ == "__main__":
    main()
