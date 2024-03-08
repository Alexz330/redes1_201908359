# 📚 Comandos para configurar VLAN

Para iniciar con la configuracion de los VLAN's es necesario estar en el 'Modo de Configuracion Global'

```console
enable
configure terminal
```

* Creacion de una VLAN en modo de acceso

    ```console
    vlan [numero_vlan]
    name [nombre_de_la_vlan]
    exit
    ```

* Configuracion del modo troncal

    ```console
    interface f0/24
    switchport mode trunk
    ```

* Cambiando la vlan nativa 
    ```console
    switchport trunk native vlan 99
    ```
 
* Restringiendo las vlan que se pasarán en modo troncal
    ```console
    switchport trunk allowed vlan 10,20,99,1002-1005
    ```

* configurando el modo acceso

    ```console
    interface f0/1
    switchport mode access
    switchport access vlan 10

    interface f0/2
    switchport mode access
    switchport access vlan 20
    ```

* guardando la configuracion
    ```console
    do write
    end
    ```