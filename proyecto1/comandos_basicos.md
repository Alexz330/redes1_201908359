# 📚 Comandos Basicos

Ingresar al modo EXEC privilegiado, para iniciar con la configuración

```console
enable
```

### Comandos - Modo EXEC

* Ingresar al Modo de Configuración Global

    ```console
    configure terminal
    ```
    ó
    ```console
    conf t
    ```

* Mostrar la configuración actual del dispositivo

    ```console
    show run
    ```
    ó
    ```console
    sh run
    ```

* Salir de algún modo de configuración

    ```console
    exit
    ```

* En dispositivos de red como routers y switches, para guardar la configuración actual de forma permanente para que la configuración sea persistente después de reiniciar el dispositivo, se utilizan los siguientes comandos

    ```console
    wr
    ```
    ó
    ```console
    write memory
    ```
    ó
    ```console
    copy running-config startup-config
    ```

### Comandos - Modo de Configuracion Global

* Cambiar el nombre del host
    ```console
    hostname [nombre]
    ```
 
* Para desactivar la búsqueda de nombres de dominio (DNS) cuando se ingresan comandos que no son reconocidos como comandos del dispositivo.

    Se utiliza para evitar la espera de la salida cuando se introduce un comando de forma incorrecta.

    ```console
    no ip domain-lookup
    ```

* En dispositivos de red como routers y switches, para guardar la configuración actual de forma permanente para que la configuración sea persistente después de reiniciar el dispositivo, se utilizan los siguientes comandos

    ```console
    do wr
    ```

* Para establecer una contraseña a los dispositivos

    ```console
    enable secret [contraseña]
    ```

---

### Otros Comandos

* Para visualizar el protocolo ARP desde una pc

    ```console
    arp -a
    ```

* Ping extendido

    ```console
    pint -t [IP]
    ```

* Apagar una interface

    ```console
    int fa0/[NUMERO]
    shutdown
    ```

* Encender una interface

    ```console
    int fa0/[NUMERO]
    no shutdown
    ```