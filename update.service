import subprocess
import os
import filecmp
import shutil

# Ruta del repositorio local

path_local_repo = '/home/alumno/'
repositorio_local = '/home/alumno/hosts/'
url_github = 'https://github.com/Javier-Veron/hosts.git'

# Ruta del archivo hosts en el repositorio clonado y en el sistema

archivo_hosts_repositorio = os.path.join(repositorio_local, 'hosts')
archivo_hosts_sistema = '/etc/hosts'

# Función para clonar o actualizar el repositorio de GitHub
def actualizar_repositorio():
    if not os.path.exists(repositorio_local):
        # Clonar el repositorio si no existe
        result = subprocess.run(['git', 'clone', url_github, path_local_repo], capture_output=True, text=True)
        if result.returncode == 0:
            print("Repositorio clonado con éxito.")
        else:
            print("Error al clonar el repositorio:", result.stderr)
            return False
    else:
        # Actualizar el repositorio si ya está clonado
        os.chdir(repositorio_local)
        result = subprocess.run(['git', 'pull','origin','master'], capture_output=True, text=True)
        if "Already up to date" in result.stdout:
            print("No hay actualizaciones.")
            return False
        else:
            print("Repositorio actualizado.")
            return True

# Función para reemplazar el archivo /etc/hosts si hay actualizaciones
def reemplazar_hosts():
    if not os.path.exists(archivo_hosts_repositorio):
        print(f"El archivo {archivo_hosts_repositorio} no existe en el repositorio.")
        return False
    
    # Comparar el archivo hosts del sistema con el del repositorio
    if not filecmp.cmp(archivo_hosts_sistema, archivo_hosts_repositorio):
        print("El archivo /etc/hosts ha sido modificado. Procediendo a reemplazarlo.")
        shutil.copy(archivo_hosts_repositorio, archivo_hosts_sistema)
        print("/etc/hosts reemplazado con éxito.")
        return True
    else:
        print("El archivo /etc/hosts no ha cambiado.")
        return False

# Función para limpiar la caché DNS
def limpiar_cache_dns():
    result = subprocess.run(['sudo', 'systemctl', 'restart', 'systemd-resolved'], capture_output=True, text=True)
    if result.returncode == 0:
        print("Caché DNS limpiada.")
    else:
        print("Error al limpiar la caché DNS.")

# Función para borrar el historial de los navegadores
def borrar_historial_navegadores():
    chrome_history_path = os.path.expanduser("~/.config/google-chrome/Default/History")
    if os.path.exists(chrome_history_path):
        os.remove(chrome_history_path)
        print("Historial de Google Chrome borrado.")
    else:
        print("No se encontró historial de Google Chrome.")

    firefox_history_path = os.path.expanduser("~/.mozilla/firefox/*.default-release/places.sqlite")
    if os.path.exists(firefox_history_path):
        os.remove(firefox_history_path)
        print("Historial de Firefox borrado.")
    else:
        print("No se encontró historial de Firefox.")

# Función principal
def main():
    if actualizar_repositorio():
        # Si el repositorio se actualiza, comprobar si se debe reemplazar /etc/hosts
        if reemplazar_hosts():
            limpiar_cache_dns()
            borrar_historial_navegadores()

if __name__ == "__main__":
    main()
