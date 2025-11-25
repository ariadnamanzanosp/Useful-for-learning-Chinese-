import time
import random
import os
import tempfile

from gtts import gTTS
from playsound import playsound

# ============================================================
# CONFIGURACIÓN
# ============================================================

# 1) Lista de oraciones para dictado (MODO 1)
SENTENCES = [
    # EJEMPLOS 
    "今天晚上我们去吃饭，好吗？",
    "明天早上八点我有一个重要的会议。",
    "他已经学习汉语三年了。",
    "请你再说一遍，好吗？",
    "这个地方离地铁站很近。",
    "如果有问题，可以随时问我。",
    "周末的时候你喜欢做什么？",
    "我觉得这本书非常有意思。",
    "昨天的作业你做完了吗？",
    "现在我们开始上课。"
]

# 2) Lista de preguntas para práctica (MODO 2)
QUESTIONS = [
    # EJEMPLOS 
    "你叫什么名字？",
    "你是哪里人？",
    "你今年多大了？",
    "你喜欢吃什么？",
    "你为什么学习汉语？",
    "你周末一般做什么？",
    "你工作还是学习？",
    "你每天怎么来学校？",
    "你喜欢看什么电影？",
    "你家有几口人？"
]

# Idioma para gTTS (Mandarín simplificado)
TTS_LANG = "zh-CN"

# Segundos de espera entre primera y segunda lectura (y entre preguntas)
DELAY_SECONDS = 40

# ============================================================
# FUNCIONES DE APOYO
# ============================================================

def speak(text: str, lang: str = TTS_LANG):
    """
    Lee en voz alta el texto usando gTTS y playsound.
    Crea un archivo temporal .mp3, lo reproduce y luego lo borra.
    """
    if not text.strip():
        return

    tts = gTTS(text=text, lang=lang)
    with tempfile.NamedTemporaryFile(delete=False, suffix=".mp3") as fp:
        temp_path = fp.name

    tts.save(temp_path)
    try:
        playsound(temp_path)
    finally:
        # Borrar el archivo temporal aunque haya error al reproducir
        if os.path.exists(temp_path):
            os.remove(temp_path)


def countdown(seconds: int):
    """
    Cuenta regresivamente en segundos para que sepas cuánto tiempo te queda
    para escribir la frase / responder.
    """
    for remaining in range(seconds, 0, -1):
        print(f"   ⏳ {remaining:2d} segundos restantes...", end="\r")
        time.sleep(1)
    print(" " * 40, end="\r")  # limpiar línea


def beep():
    """
    Hace un 'pip' utilizando la campana del sistema.
    En muchas terminales esto produce un sonido.
    """
    print("\a", end="")  # campana del sistema
    # También imprimimos algo por si el beep no se oye
    print("  🔔 pip!")


# ============================================================
# MODO 1: Dictado de oraciones (dos lecturas, orden aleatorio)
# ============================================================

def modo_dictado_oraciones():
    if not SENTENCES:
        print("⚠️ No hay oraciones en la lista SENTENCES. Edita el código y añade tus frases.")
        return

    print("\n===============================")
    print("   MODO 1: DICTADO DE ORACIONES")
    print("===============================\n")

    print(f"Hay {len(SENTENCES)} oraciones en total.")
    print("Se leerán en orden aleatorio. Para cada oración:\n"
          " - Primera repetición\n"
          f" - {DELAY_SECONDS} segundos para escribir\n"
          " - Segunda repetición\n")
    input("Cuando estés lista, pulsa Enter para comenzar...")

    # Mezclamos el orden de las oraciones
    sentences_shuffled = random.sample(SENTENCES, len(SENTENCES))

    for idx, sentence in enumerate(sentences_shuffled, start=1):
        print(f"\n-----------------------------------------")
        print(f"Oración {idx}/{len(sentences_shuffled)} (¡no mires la pantalla!)")
        print("-----------------------------------------")

        # Primera lectura
        speak(sentence)

        # Esperar DELAY_SECONDS antes de la segunda lectura
        print(f"Esperando {DELAY_SECONDS} segundos antes de repetir...")
        countdown(DELAY_SECONDS)

        # Segunda lectura
        print("Segunda lectura:")
        speak(sentence)

        # Pausa opcional antes de pasar a la siguiente
        input("Presiona Enter para pasar a la siguiente oración...")


# ============================================================
# MODO 2: Preguntas al azar con beep + espera
# ============================================================

def modo_preguntas():
    if not QUESTIONS:
        print("⚠️ No hay preguntas en la lista QUESTIONS. Edita el código y añade tus preguntas.")
        return

    print("\n=============================================")
    print("   MODO 2: PREGUNTAS AL AZAR (Q&A / ENTREVISTA)")
    print("=============================================\n")

    print(f"Hay {len(QUESTIONS)} preguntas disponibles.")
    while True:
        try:
            cantidad = int(input("¿Cuántas preguntas quieres practicar? "))
            if cantidad <= 0:
                print("Por favor, escribe un número positivo.")
                continue
            if cantidad > len(QUESTIONS):
                print(f"Solo hay {len(QUESTIONS)} preguntas. Usaré esa cantidad.")
                cantidad = len(QUESTIONS)
            break
        except ValueError:
            print("Por favor, escribe un número entero.")

    print(f"\nSe seleccionarán {cantidad} preguntas al azar.")
    print(f"Para cada pregunta:\n"
          " - Se lee la pregunta en chino\n"
          " - Se hace un 'pip'\n"
          f" - Se esperan {DELAY_SECONDS} segundos para que respondas\n"
          " - Después pasa automáticamente a la siguiente\n")

    input("Cuando estés lista, pulsa Enter para comenzar...")

    selected_questions = random.sample(QUESTIONS, cantidad)

    for idx, question in enumerate(selected_questions, start=1):
        print("\n-----------------------------------------")
        print(f"Pregunta {idx}/{cantidad}")
        print("-----------------------------------------")

        # Leer la pregunta
        speak(question)

        # 'pip' para marcar el inicio del tiempo de respuesta
        beep()

        # Espera para que respondas
        print(f"Tienes {DELAY_SECONDS} segundos para responder...")
        countdown(DELAY_SECONDS)

    print("\n✅ Fin de la sesión de preguntas.")


# ============================================================
# MENÚ PRINCIPAL
# ============================================================

def main():
    print("=====================================")
    print("   PROGRAMA DE DICTADOS EN MANDARÍN  ")
    print("=====================================\n")

    print("Elige un modo:\n")
    print("  1) Modo dictado de oraciones (dos lecturas, orden aleatorio)")
    print("  2) Modo preguntas al azar (pip + 40s para responder)")
    print("  0) Salir\n")

    while True:
        opcion = input("Escribe 1, 2 o 0: ").strip()
        if opcion == "1":
            modo_dictado_oraciones()
            break
        elif opcion == "2":
            modo_preguntas()
            break
        elif opcion == "0":
            print("Saliendo del programa. 再见！")
            return
        else:
            print("Opción no válida. Intenta otra vez.\n")

if __name__ == "__main__":
    main()
