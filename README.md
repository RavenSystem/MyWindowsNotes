# Mis Notas sobre Windows 11

Estas son simplemente mis anotaciones personales sobre cómo instalar y configurar un equipo con Windows 11 para aprovechar al máximo toda su potencia.

[![Donate](https://img.shields.io/badge/donate-PayPal-blue.svg)](https://paypal.me/ravensystem)

## Crear USB de Instalación para instalar sin TPM 2.0
- Es necesario un equipo con Windows (Real o virtual) y un pendrive de al menos 8GB de capacidad.
- Descargar los creadores de medios para Windows 10 y Windows 11 de [Windows Installation Media Creator](https://support.microsoft.com/es-es/windows/crear-medios-de-instalación-de-windows-99a58364-8c02-206f-aa6f-40c3b507420d)
- Usar el Creador de medios de Windows 11 y crear un USB de instalación.
- Copiar el archivo del pendrive `\sources\install.esd` al ordenador.
- Usar el Creador de medios de Windows 10 y crear un USB de instalación.
- Reemplazar el archivo del pendrive `\sources\install.esd` por el que hemos copiado previamente en el ordenador.
- El pendrive ya está listo para arrancar el equipo desde USB e instalar Windows.

## Instalar Windows
- Arrancar desde el USB de instalación.
- No introducir la clave de activación de Windows (Mejor activar Windows una vez instalado y configurado, aunque se puede dejar sin activar y todo funcionará sin restricciones, menos la parte de personalizar los colores y el fondo de escritorio).
- Una vez que se haya instalado, se ejecutará el asistente inicial de configuración, preguntando el país:
    - Pulsar `Shift + F10` para abrir un Terminal.
    - Ejecutar el comando (Si la distribución del teclado no permite escribir la barra `\`, se puede copiar y pegar con el ratón una del propio Terminal):
```shell
OOBE\BYPASSNRO
```
   - Cuando el equipo de reinicie, desconectar el cable de red.
        - También es posible dejarlo sin conexión abriendo de nuevo un Terminal (`Shift + F10`) y ejecutando:
```shell
ipconfig /release
```
   - Seguir los pasos del asistente, indicando que no tiene conexión a Internet y creando una cuenta local de usuario.
    - En las preguntas del final de la configuración, de permisos y recopilación de datos, seleccionar siempre la opción de no enviar ni compartir (la segunda opción de las dos que hay).
- Una vez terminado todo, conectarlo a Internet.
- Abrir Configuración -> Windows Update -> Buscar actualizaciones, e instalarlas. Y reiniciar, aunque no lo pida.

## Eliminar basura de Windows
(Fuente: https://github.com/Raphire/Win11Debloat)
- Abrir un Terminal como Administrador (Botón derecho sobre su icono y Ejecutar como administrador) y ejecutar:
```shell
& ([scriptblock]::Create((irm "https://win11debloat.raphi.re/"))) -RunDefaults -RemoveCommApps -RemoveW11Outlook -RemoveGamingApps -DisableDVR -DisableLockscreenTips -ClearStartAllUsers -RevertContextMenu -ShowHiddenFolders -HideTaskview
```
- Después, ejecutar:
```shell
& ([scriptblock]::Create((irm "https://win11debloat.raphi.re/")))
```
- Seleccionar la opción `3`.
- Marcar: Only show installed apps.
- Seleccionar:
    - Microsoft.Edge
    - Microsoft.GetHelp
    - Microsoft.MSPaint 
    - Microsoft.Windows.DevHome
    - Microsoft.YourPhone
    - Microsoft.ZuneMusic
    - MicrosoftWindows.CrossDevice
- Pulsar Confirm.

## Activar la entrada de usuario automática (Autologin)
- Abrir un Terminal y ejecutar:
```shell
regedit
```
- Ir a la clave:
```
HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows NT\CurrentVersion\PasswordLess\Device
```
- Cambiar el valor a `0` (Por defecto es `2`) de:
```
DevicePasswordLessBuildVersion
```
- Cerrar el Editor de Registro.
- En el Terminal, ejecutar:
```shell
netplwiz
```
- Desactivar la opción de que los usuarios tienen que introducir la contraseña.

## Desactivar la integridad de la memoria
- Incrementa el rendimiento del equipo, a cambio de tener el nivel de seguridad de Windows 10.
- Abrir Configuración -> Privacidad y seguridad -> Seguridad de Windows.
- Pulsar: Abrir Seguridad de Windows.
- Abrir: Seguridad del dispositivo -> Detalles de aislamiento del núcleo.
    - Desactivar: Integridad de memoria.
    - Desactivar: Protección de autoridad de seguridad local (LSA).
- Reiniciar el equipo.
- Abrir un Terminal y ejecutar:
```shell
msinfo32
```
- Seleccionar: Resumen del sistema.
    - A la derecha, buscar: Seguridad basada en virtualización.
    - Si está habilitada o activada:
        - En el Terminal, ejecutar:
```shell
bcedit /set hypervisorlaunchtype off
```
  - Reiniciar el equipo.
    - Nota: Para revertir este cambio, abrir un Terminal y ejecutar:
```shell
bcedit /set hypervisorlaunchtype on
```

## Minimizar el impacto del antivirus
- Abrir Configuración -> Privacidad y seguridad -> Seguridad de Windows.
- Pulsar: Abrir Seguridad de Windows.
- Abrir: Seguridad del dispositivo -> Protección antivirus y contra amenazas.
- En: Configuración de antivirus y protección contra amenazas, pulsar: Administrar la configuración.
    - Desactivar todo, excepto: Protección en tiempo real.
    - Opcional: Abajo del todo, en Exclusiones, pulsar: Agregar o quitar exclusiones.
        - Pulsar en + Agregar exclusión.
        - Carpeta: `C:\Archivos de programa (x86)\Steam\`
        - Añadir cualquier otra carpeta que contenga software que necesite toda la potencia del sistema.

## Desactivar la ejecución en segundo plano de los componentes del sistema
- Abrir Configuración -> Sistema -> Componentes del sistema.
- Para cada uno delos componentes, pulsar el icono de los 3 puntos (...) a la derecha -> Opciones avanzadas.
    - Establecer en Nunca la opción de los Permisos de componente en segundo plano.

## Desactivar algunas opciones visuales
- Abrir Configuración -> Sistema -> Configuración avanzada del sistema.
- En Rendimiento, pulsar Configuración.
- En la pestaña: Efectos visuales, seleccionar Personalizar.
    - Dejar marcado únicamente:
        - Mostrar el contenido de la ventana mientras se arrastra.
        - Mostrar vistas en miniatura en lugar de iconos.
        - Suavizar bordes para las fuentes de pantalla.
    - Pulsar Aceptar.

## Mostrar el plan de energía de Máximo Rendimiento en las opciones de energía
- Abrir un Terminal y ejecutar:
```shell
powercfg -duplicatescheme e9a42b02-d5df-448d-aa00-03f14749eb61
```

## Activar el apagado automático de núcleos de la CPU para maximizar el turbo en los demás
- Se conoce como "Core Parking". Es una opción que está oculta en las opciones de energía del procesador.
- Abrir un Terminal y ejecutar:
```shell
powercfg -attributes SUB_PROCESSOR CPMINCORES -ATTRIB_HIDE
powercfg.cpl
```
- El plan recomendado para maximizar la potencia del equipo es: Equilibrado.
- Pulsar: Cambiar la configuración del plan (La configuración sólo se aplica al plan seleccionado).
    - Pulsar: Cambiar la configuración avanzada de energía.
        - Ir a: Administración de energía del procesador, y configurar:
            - Mínima detención de núcleos de rendimiento de procesador: `50%`.
                - Esta opción indica porcentualmente la cantidad de núcleos que nunca se van a apagar.
            - Estado mínimo del procesador: `5%`.
            - Estado máximo del procesador: `100%`.
        - Pulsar: Aceptar.

## Evitar que ciertos programas cambien el plan de energía activo
- Algunos programas, como SteamVR, cambian el plan de energía activo, provocando una pérdida de rendimiento.
- Abrir un Terminal y ejecutar:
```shell
powercfg /l
```
- Copiar el GUID del plan a dejar siempre activo.
- Ejecutar:
```shell
gpedit.msc
```
- Seleccionar: Directiva Equipo local -> Plantillas administrativas -> Sistema -> Administración de energía
- A la derecha, doble clic en: Especificar un plan de energía activo personalizado.
- Seleccionar: Habilitada.
- Pegar el GUID del plan en: Plan de energía activo personalizado (GUID).
- Pulsar: Aplicar.

## Configurar el gestor de procesos para priorizar los de primer plano
- Abrir un Terminal y ejecutar:
regedit
- Ir a la clave:
```
HKEY_LOCAL_MACHINE\SYSTEM\ControlSet001\Control\PriorityControl
```
- Cambiar el valor a `26` Hexadecimal (Por defecto es `2`) de:
```
Win32PrioritySeparation
```
- Cerrar el Editor de Registro.

## Desactivar la creación de puntos de restauración (Opcional)
- Abrir Configuración -> Sistema -> Protección del sistema.
    - Pulsar Configurar y seleccionar: Deshabilitar protección del sistema.

## Desactivar el uso de disco como RAM cuando haga falta (Opcional)
- Sólo debe desactivarse si el equipo tiene memoria RAM de sobra (16GB o más).
- Abrir Configuración -> Sistema -> Configuración avanzada del sistema.
- En Rendimiento, pulsar Configuración.
- En la pestaña: Opciones avanzadas, pulsar Cambiar.
    - Desmarcar: Administrar automáticamente el tamaño del archivo de paginación para todas las unidades.
    - Para cada unidad, seleccionar: Sin archivo de paginación, y pulsar Establecer.
    - Pulsar Aceptar.

## NVIDIA: Panel de Control
- CUDA - Política de uso de la memoria del sistema: No usar la memoria del sistema como respaldo
- Filtrado de texturas - Calidad: Alta calidad
- Fotogramas preprocesados para la realidad virtual: Utilizar la configuración de la aplicación 3D
- Modo de control de energía: Máximo rendimiento preferido
- Tamaño de la caché del sombreador: 100 GB

## AMD: Software Adrenaline
- Teselación: Desactivado.
