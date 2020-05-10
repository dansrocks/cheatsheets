# Rsync

## Comparar recursivamente el contenido de dos directorios
Comparar dos directorios sin actualizar:

    rsync --dry-run -v -r -c --delete directorioA/ directorioB/

Sincronizar un directorio con el contenido de otro

    rsync origen/ destino/
    
Nota: si no se pone la / final, creará una carpeta '*origen*' dentro de '*destino*'.

    
Opciones:
* –dry-run -v : simula las acciones y muestra lo que haría
* -r : recursivo
* -c : comprueba el contenido del fichero. E.o.c: usa tamaño y timestamp
* -delete : borra los archivos en destino que no están/ en origen/
* -e ssh : abre un canal ssh para sincronizar Puede usarse en origen o en destino.