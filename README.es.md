# debug-on-mac

> Acceda a un escritorio MacOS a través de VNC, gratis, en menos de 5 minutos 

Shell que configura un servidor VNC en MacOS VNC que puede correr a través de Github Actions y que puede ser accesado a través de un TCP tunnel.

**NOTA:** Este repositorio está basado en [fastmac](https://github.com/fastai/fastmac) y [fastmac-VNCgui](https://github.com/dakotaKat/fastmac-VNCgui).

---

## ¿Qué es esto?
 
Este repositorio incluye un shell y una definición para Github Actions que
  - habilita el servidor VNC en MacOS VNC server, asignando un nuevo usuario administrador y asignando un password para VNC 
  - instala `ngrok` y lo usa para configurar un tcp tunnel para la conexión con VNC/Apple Screen Sharing.
  - ejecuta `tmate` para configurar un Web Shell y una conexión SSH

---

## ¿Cómo usarlo?

Ud. debe clonar el repositorio, configurar algunos secretos en el repositorio resultante y ejecutar un de los pipelines definidos para Github Actions.

---

## Configuración

### Clonar Plantilla (Template)

Este repositorio ha sido configurado como una plantilla (template). Ud. puede crear un nuevo repositorio usando [este link](https://github.com/vnc-tools/debug-on-mac/generate)
  - Escriba `debug-on-mac` en el campo "repository name"
  - y haga clic "Create repository from template".

### Configuración

Para ejecutar un escritorio MacOS desktop, debe configurar tres secretos para Github Actions:
  - `NGROK_AUTH_TOKEN` con su llave de autenticación de https://dashboard.ngrok.com/auth
  - `VNC_USER_PASSWORD` con la contraseña deseada para el usuario "VNC User" (la cuenta `vncuser`) en el escritorio
  - `VNC_PASSWORD` con la contraseña para la conexión VNC

---

## Escritorio MacOS

### Iniciando un Escritorio MacOS

Luego de configurar el repositorio, se puede ejecutar el escritorio MacOS ejecutando el pipeline "macos-with-vnc" Github Actions
  - Ir al pipeline <a href="../../actions?query=workflow%3Amacos-with-vnc">macos-with-vnc</a>
  - Hacer clic en la lista desplegable "Run workflow" a la derecha, y
  - Hacer clic en el botón verde "Run workflow" que aparece después.

<img width="365" src="https://user-images.githubusercontent.com/346999/92965396-91320680-f42a-11ea-9bc3-90682e740343.png" />


### Accesando el Escritorio MacOS

Una vez el pipeline se inicia, se puede revisar el status del pipeline. En la sección "you can VNC to..." en el log se puede  obtener la dirección IP para el tunel ngrok para VNC.

Si está usando **Apple Screen Sharing** o **RealVNC Viewer**
  - use la dirección del tuner ngrok para VNC como la dirección IP del VNC
  - use el usuario y la contraseña del usuario del sistema ("VNC User"/contraseña del usuario). Esta contraseña es asignada a partir del secreto `VNC_USER_PASSWORD`
  
Si está usando cualquier otro cliente VNC
  - use la dirección del tuner ngrok para VNC como la dirección IP del VNC
  - use la contraseña para la conexión VNC. Esta contraseña es asignada a partir del secreto `VNC_PASSWORD`.

**NOTA:** Si inicia un escritorio MacOS, Ud puede acceder a la misma máquina usando un shell SSH. Puede seguir los pasos descritos más abajo para conectarse al shell de MacOs.

---

## Shell de MacOS

### Iniciando un Shell de MacOS

Luego de configurar los secretos, se puede iniciar un shell de MacOS ejecutando el pipeline "macos-with-ssh" de Github Actions
  - Ir al pipeline <a href="../../actions?query=workflow%3Amacos-with-vnc">macos-with-ssh</a>
  - Hacer clic en la lista desplegable "Run workflow" a la derecha, y
  - Hacer clic en el botón verde "Run workflow" que aparece después.

<img width="365" src="https://user-images.githubusercontent.com/346999/92965396-91320680-f42a-11ea-9bc3-90682e740343.png" />

### Conectando al shell de MacOS

Una vez el pipeline se inicia, se puede revisar el status del pipeline. En la sección  "Setup tmate session", una vez se ha instalado `tmate` se puede ver repetidamente unas lineas como las siguientes:

```
WebURL: https://tmate.io/t/rXbusP3qkYsfALDSLMQZVwG3d

SSH: ssh rXbusP3qkYsfALDSLMQZVwG3d@sfo2.tmate.io
```

Copie y pegue la línea ssh line (p.ej., `ssh rXbusP3qkYsfALDSLMQZVwG3d@sfo2.tmate.io` en este ejemplo) en su terminal y presione kbd>Enter</kbd>.

---

## Detener la sesión de MacOS

La sesión de MacOs se ejecutará por hasta seis horas. Cuando termine de trabajar, se debe cerrar la sesión, ya que de otra forma estará tomando un computador que otra persona podría estar usando !

Para cerrar la sesión, haga clic en el botón rojo "Cancel workflow" al lado derecho de la pantalla de Github Actions.



---

## Consideraciones

### Términos del servicio de Github Actions

Por favor revise los [GitHub Actions Terms of Service](https://docs.github.com/en/github/site-policy/github-additional-product-terms#5-actions-and-packages). Note que su repositorio debe ser público, de otra forma tendrá un límite mensual sobre la cantidad de minutos que puede usar. Note también que, de acuerdo a los TOS, el repositorio que contiene estos archivos necesita ser el mismo donde Ud está desarrollando el proyecto y para el cuál está ejecutando el pipeline, y especialmente que este servicio se está usando para "*production, testing, deployment, or publication of [that] software project*".

### tmate expone un shell que se puede compartir

El pipeline definido en el repositorio es un wrapper sobre [tmate](https://tmate.io/), así que todas las funcionalides de tmate están disponibles. tmate, en si mismo, está basado en [tmux](https://github.com/tmux/tmux/wiki), así que tiene todas sus funcionalidades también. En la práctica, esto significa que otras personas pueden compartir la misma pantalla. Estopuede ser muy útil para depuración y soporte. La integración con Github Actions la provee [action-tmate](https://github.com/mxschmitt/action-tmate).
