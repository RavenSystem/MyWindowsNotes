# Mis Notas sobre Instalar y Configurar Windows 11

[![Donate](https://img.shields.io/badge/donate-PayPal-blue.svg)](https://paypal.me/ravensystem)

[ [English](README.md) ]

[ [Fuente en GitHub](https://github.com/RavenSystem/MyWindowsNotes) ]

Estas son simplemente mis anotaciones personales sobre cómo instalar y configurar un equipo con Windows 11 para aprovechar al máximo toda su potencia, para juegos, realidad virtual, flujos pesados de trabajo...

**No me hago responsable de posibles daños o pérdida de datos que puedan ocurrir.**

## Crear USB de Instalación para instalar sin el requisito de TPM 2.0
- Es necesario un equipo con Windows (Real o virtual) y un pendrive de al menos 8GB de capacidad.
- Descargar los creadores de medios para Windows 10 y Windows 11 de [Windows Installation Media Creator](https://support.microsoft.com/es-es/windows/crear-medios-de-instalación-de-windows-99a58364-8c02-206f-aa6f-40c3b507420d)
- Usar el Creador de medios de **Windows 11** y crear un USB de instalación.
- Copiar el archivo del pendrive `\sources\install.esd` al ordenador.
- Usar el Creador de medios de **Windows 10** y crear un USB de instalación.
- Reemplazar el archivo del pendrive `\sources\install.esd` por el que hemos copiado previamente en el ordenador.
- El pendrive ya está listo para arrancar el equipo desde USB e instalar Windows.

## Crear ISO compatible con BootCamp para Macs Intel
- [Utilidad de creación de ISO BootCamp](https://github.com/RavenSystem/BootCampW11Installer)

## Instalar Windows
- Arrancar desde el USB de instalación.
  - Para Macs Intel, usar el Asistente de Instalación BootCamp con el archivo ISO correspondinte.
- No introducir aquí la clave de activación de Windows (Mejor activar Windows una vez instalado y configurado, aunque se puede dejar sin activar y todo funcionará sin restricciones, menos la parte de personalizar los colores y el fondo de escritorio).
- Una vez que se haya instalado, se ejecutará el asistente inicial de configuración, preguntando el país:
  - Pulsar `Shift + F10` para abrir un Terminal.
  - Para evitar el paso obligatorio de usar una cuenta de Microsoft, hay que ejecutar el siguiente comando (Si la distribución actual del teclado no permite escribir la barra `\`, se puede copiar y pegar con el ratón una del propio Terminal):
```shell
OOBE\BYPASSNRO
```
  - Si el comando anterior no funciona, ejecutar el siguiente comando como alternativa:
```shell
reg add HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\OOBE /v BypassNRO /t REG_DWORD /d 1 /f shutdown /r /t 0
```
   - Cuando el equipo se reinicie, desconectar el cable de red.
      - También es posible dejarlo sin conexión abriendo de nuevo un Terminal (`Shift + F10`) y ejecutando:
```shell
ipconfig /release
```
   - Seguir los pasos del asistente, indicando que no tiene conexión a Internet y creando una cuenta local de usuario.
   - En las preguntas del final de la configuración, de permisos y recopilación de datos, seleccionar siempre la opción de no enviar ni compartir (la segunda opción de las dos que hay).
- Una vez terminado todo, conectarlo a Internet.
- Abrir Configuración -> Windows Update -> Buscar actualizaciones, e instalarlas. Y reiniciar, aunque no lo pida.

## Eliminar software no necesario
(Fuente: [https://github.com/Raphire/Win11Debloat](https://github.com/Raphire/Win11Debloat))
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

## Desactivar funciones y procesos no necesarios
(Fuente: [https://github.com/ChrisTitusTech/winutil](https://github.com/ChrisTitusTech/winutil))
- Abrir un Terminal como Administrador (Botón derecho sobre su icono y Ejecutar como administrador) y ejecutar:
```shell
irm "https://christitus.com/win" | iex
```
- Se abrirá una ventana: Hacer click en "Tweaks".
  - Hacer click en "Standard".
  - A la izquierda, descarmar:
    - Create Restore Point.
  - A la izquierda, marcar:
    - Disable Recall.
    - Debloat Brave.
    - Debloat Edge.
    - Adobe Network Block.
    - Adobe Debloat.
    - Disable Tedero.
    - Disable Background Apps.
    - Disable Fullscreen Optimizations.
    - Disable Microsoft Copilot.
    - Disable Intel MM (vPro LMS).
    - Disable Notification Tray/Calendar.
    - Disable Windows Platform Binary Table (WPBT).
    - Set Classic Right-Click Menu.
    - Remove Home from explorer.
    - Remove Gallery from explorer.
    - Remove OneDrive.
    - Block Razer Software Installs.
  - "Run Tweaks" abajo.
- Esperar a que arriba a la derecha indique "Tweaks finished" y cerrarlo.
- Reiniciar el equipo.

## Deshabilitar las Optimizaciones de Pantalla Completa
- Abrir un Terminal como Administrador (Botón derecho sobre su icono y Ejecutar como administrador) y ejecutar:
```shell
reg add "HKCU\System\GameConfigStore" /v "GameDVR_Enabled" /t REG_DWORD /d "0" /f
reg add "HKCU\System\GameConfigStore" /v "GameDVR_FSEBehaviorMode" /t REG_DWORD /d "2" /f
reg add "HKCU\System\GameConfigStore" /v "GameDVR_FSEBehavior" /t REG_DWORD /d "2" /f
reg add "HKCU\System\GameConfigStore" /v "GameDVR_HonorUserFSEBehaviorMode" /t REG_DWORD /d "1" /f
reg add "HKCU\System\GameConfigStore" /v "GameDVR_DXGIHonorFSEWindowsCompatible" /t REG_DWORD /d "1" /f
reg add "HKCU\System\GameConfigStore" /v "GameDVR_EFSEFeatureFlags" /t REG_DWORD /d "0" /f
reg add "HKCU\System\GameConfigStore" /v "GameDVR_DSEBehavior" /t REG_DWORD /d "2" /f
```

## Activar la entrada de usuario automática (Opcional)
- Abrir un Terminal y ejecutar:
```shell
regedit
```
- Ir a la clave:
```
HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows NT\CurrentVersion\PasswordLess\Device
```
- Establecer el valor a `0` (Por defecto es `2`) de:
```
DevicePasswordLessBuildVersion
```
- Cerrar el Editor de Registro.
- En el Terminal, ejecutar:
```shell
netplwiz
```
- Desactivar la opción de que los usuarios tienen que introducir la contraseña y seguir las instrucciones en pantalla.

## Desactivar la integridad de la memoria
- Incrementa el rendimiento del equipo, a cambio de tener el nivel de seguridad de Windows 10.
- Abrir Configuración -> Privacidad y seguridad -> Seguridad de Windows.
- Pulsar: Abrir Seguridad de Windows.
- Abrir: Seguridad del dispositivo -> Detalles de aislamiento del núcleo.
  - Desactivar: Integridad de memoria.
  - Desactivar: Protección de autoridad de seguridad local (LSA).
- Abrir un Terminal y ejecutar:
```shell
regedit
```
- Ir a la clave:
```
HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\DeviceGuard
```
- Establecer el valor a `0` de:
```
EnableVirtualizationBasedSecurity
```
- Cerrar el Editor de Registro.
- Reiniciar el equipo.

- Para comprobar si la Seguridad basada en virtualización está desactivada, abrir un Terminal y ejecutar:
```shell
msinfo32
```
- Seleccionar: Resumen del sistema.
  - A la derecha, buscar: Seguridad basada en virtualización.

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

## Desactivar algunas opciones visuales
- Abrir Configuración -> Sistema -> Configuración avanzada del sistema.
- En Rendimiento, pulsar Configuración.
- En la pestaña: Efectos visuales, seleccionar Personalizar.
  - Dejar marcado únicamente:
    - Mostrar el contenido de la ventana mientras se arrastra.
    - Mostrar vistas en miniatura en lugar de iconos.
    - Suavizar bordes para las fuentes de pantalla.
  - Pulsar Aceptar.

## Añadir el plan de energía RavenSystem Ultimate
Este plan de energía maximiza la potencia del equipo y reduce la latencia. Ideal para juegos, realidad virtual, y software de simulación en tiempo real.
- Descargar el archivo [ravensystem_ultimate.pow](https://github.com/RavenSystem/MyWindowsNotes/raw/refs/heads/main/ravensystem_ultimate.pow).
- Abrir un Terminal como Administrador (Botón derecho sobre su icono y Ejecutar como administrador) y ejecutar:
```shell
powercfg -import '[ruta absoluta completa]\ravensystem_ultimate.pow' abc00000-0000-0000-0000-000000000011
powercfg -setactive abc00000-0000-0000-0000-000000000011
```

## Mostrar el plan de energía de Máximo Rendimiento en las opciones de energía (Opcional)
- Abrir un Terminal y ejecutar:
```shell
powercfg -duplicatescheme e9a42b02-d5df-448d-aa00-03f14749eb61
```

## Plan de Energía Mejorado (Opcional)
- Abrir un Terminal y ejecutar:
```shell
powercfg -attributes SUB_PROCESSOR PERFINCTHRESHOLD -ATTRIB_HIDE
powercfg -attributes SUB_PROCESSOR PERFCHECK -ATTRIB_HIDE
powercfg -attributes SUB_PROCESSOR CPMINCORES -ATTRIB_HIDE
powercfg -attributes SUB_PROCESSOR CPPERF -ATTRIB_HIDE
powercfg -attributes SUB_PROCESSOR IDLEPROMOTE -ATTRIB_HIDE
powercfg.cpl
```
- El plan recomendado para maximizar la potencia del equipo sin tener un consumo excesivo es: **Equilibrado**.
- Pulsar: Cambiar la configuración del plan (La configuración sólo se aplica al plan seleccionado).
  - Pulsar: Cambiar la configuración avanzada de energía.
    - Pulsar `Restaurar valores predeterminados del plan`.
    - Ir a: Administración de energía del procesador, y configurar:
      - Umbral de aumento de rendimiento de procesador: `90%`
      - Intervalo de comprobación de tiempo de rendimiento de procesador: `20ms`
      - Mínima detención de núcleos de rendimiento de procesador: `10%`.
        - Esta opción indica porcentualmente la cantidad de núcleos que nunca se van a apagar.
      - Estado de rendimiento detenido de la detención de núcleo de rendimiento de procesador: `Estado de rendimiento más profundo`.
      - Umbral de aumento de nivel de inactividad de procesador: `90%`.
    - Pulsar: Aceptar.

## Evitar que ciertos programas cambien el plan de energía activo (Opcional)
- Algunos programas, como SteamVR, cambian el plan de energía activo, provocando una pérdida de rendimiento.
- Abrir un Terminal y ejecutar:
```shell
powercfg -l
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
(Fuente: https://www.xbitlabs.com/win32priorityseparation-performance/)
- Abrir un Terminal y ejecutar:
```shell
regedit
```
- Ir a la clave:
```
HKEY_LOCAL_MACHINE\SYSTEM\ControlSet001\Control\PriorityControl
```
- Establecer el valor a `1a` para uso general, o `24` Hexadecimal para RV (Por defecto es `2`) de:
```
Win32PrioritySeparation
```
- Cerrar el Editor de Registro.

## Desactivar la creación de puntos de restauración (Opcional)
- Abrir Configuración -> Sistema -> Protección del sistema.
  - Pulsar Configurar y seleccionar: Deshabilitar protección del sistema.

## Desactivar el uso del disco como RAM cuando haga falta (Opcional)
- Sólo debe desactivarse si el equipo tiene memoria RAM de sobra (16GB o más, dependiendo del software que se vaya a ejecutar).
- Abrir Configuración -> Sistema -> Configuración avanzada del sistema.
- En Rendimiento, pulsar Configuración.
- En la pestaña: Opciones avanzadas, pulsar Cambiar.
  - Desmarcar: Administrar automáticamente el tamaño del archivo de paginación para todas las unidades.
  - Para cada unidad, seleccionar: Sin archivo de paginación, y pulsar Establecer.
  - Pulsar Aceptar.

## NVIDIA: Panel de Control
- Versión de controlador recomendada: 580.97
- Todo por defecto, excepto:
  - CUDA - Política de uso de la memoria del sistema: No usar la memoria del sistema como respaldo
  - Fotogramas preprocesados para la realidad virtual: Utilizar la configuración de la aplicación 3D
  - Tamaño de la caché del sombreador: 100 GB

## AMD: Software Adrenaline
- Teselación: Desactivado.
