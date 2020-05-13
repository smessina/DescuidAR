El [sistema web](https://www.argentina.gob.ar/circular) del **Certificado Único de Circulación** es tan inseguro como innecesario.

Lo desarrollado no solo tiene **graves fallas en la seguridad** de los datos personales, sino que la solución está mal pensada (¿inocentemente?).

---
> El presente articulo es una versión extendida del [hilo](https://twitter.com/smessina_/status/1259970553164845064) publicado en Twitter el 11/05/2020.

---

Si un ciudadano argentino está encuadrado en las excepciones al **AISLAMIENTO SOCIAL PREVENTIVO Y OBLIGATORIO**, establecido por el [Decreto 2020-297-APN-PTE](https://www.boletinoficial.gob.ar/detalleAviso/primera/227042/20200320), se debe tramitar el **Certificado Único de Circulación** *(Declaración Jurada de Excepción para Circulación en Emergencia Sanitaria – COVID 19 conforme Artículo 6º Decreto 297/2020)* desde la plataforma web [argentina.gob.ar/circular](https://www.argentina.gob.ar/circular) y completar un amplio formulario con datos que lo identifican *(en particular su número de DNI + número de Trámite impreso en el mismo)* y que justifican su excepción.

![argentina.gob.ar/circular](https://i.ibb.co/5LBj22q/01.jpg)

Aproximadamente en 24hs estará aprobado el permiso, para lo cual se debe ingresar nuevamente a [argentina.gob.ar/circular](https://www.argentina.gob.ar/circular), cargar otra vez el  DNI + número de Trámite, descargar en PDF el respectivo **Certificado Único de Circulación** y luego imprimirlo o llevarlo en tu celular al circular.

![argentina.gob.ar/circular/descarga](https://i.ibb.co/XD6d5vb/02.jpg)

Acá es donde empieza la curiosidad.

El PDF obtenido detalla las formalidades que autorizan la circulación y exceptúan del aislamiento. Incluye un **código QR** que será escaneado por las Fuerzas de Seguridad para confirmar dicha autorización cuando se está en circulación.

¿A dónde lleva el QR?

![QR](https://i.ibb.co/TqRVtR6/03.jpg)

Usando un lector QR se observa que el incluido en el PDF apunta a la dirección web https://form-ddjj.argentina.gob.ar/validacion/?hash_code=XXXXXXXXX, donde ese **XXXXXXXXX** es un hash único de autenticación; supuestamente para corroborar la autenticidad del certificado y de la persona referida en el mismo.

![QR](https://i.ibb.co/WcFqXTY/04.jpg)

Efectivamente al acceder a https://form-ddjj.argentina.gob.ar/validacion/?hash_code=XXXXXXXXX vía QR se corrobora que quién se menciona en el Certificado Único de Circulación está habilitado para circular, indicando incluso el motivo (originalmente cargado por el ciudadano el en formulario de [argentina.gob.ar/circular](https://www.argentina.gob.ar/circular)).

![Habilitado](https://i.ibb.co/YfpcVLc/05.jpg)

¿Como es que el QR asocia al autorizado en el Certificado?

Desmenuzando el hash parametrizado en la dirección web apuntada se descubre que:

- Está expresado en **Base64** (no encriptado)
- El contenido real es un conjunto de datos en formato **JSON**
- Dicho JSON contiene el número de DNI + número de Trámite del ciudadano

![HASH](https://i.ibb.co/MCx7jZ6/06.jpg)

### ALERTA #1

El **código QR** expone dos datos muy sensibles y utilizados/requeridos por un gran número de trámites online ante organismos estatales.

Por ejemplo, si el PDF en cuestión fuera extraviado/sustraído, con dichos datos y un poco de ingeniería social se podría **restablecer la Clave de Seguridad Social** (ANSES) de cualquier ciudadano y con ello acceder a más información sensible del mismo y de sus familiares *(domicilio, cobros de sueldo, jubilación, asignaciones, etc.)*.

![ANSES](https://i.ibb.co/ByKV53K/07.jpg)

¿Puede hacerse más seguro? Si, de mínima que cada permiso de circulación tenga un número **ID propio**, **desasociado públicamente** al  número de DNI + número de Trámite del ciudadano y que el respectivo código **QR** sea su representación.

---

Aunque también podemos cuestionar la efectividad de todo el sistema de solicitud y acreditación del permiso de circulación.

Por lo visto anteriormente se podría decir que el mencionado **código QR** resulta ser una versión "reducida" del DNI, pues cumple el rol de asociar la identidad de la persona mencionada en el **Certificado Único de Circulación** con la efectiva autorización expresada por el Gobierno vía la dirección web apuntada.

A la vez, para circular el ciudadano debe llevar encima su **DNI** y el certificado para que eventualmente las Fuerzas de Seguridad:

- Identifique a la persona presente con su DNI
- Solicite el certificado respectivo que lo autoriza
- Corrobore la autenticidad del mismo mediante el código QR
- Acredite que la identidad arrojada por el código QR es de la persona presente e identificada con su DNI

### ALERTA #2

Ocurren dos autenticaciones "redundantes" en dicho proceso:

- Persona == número de DNI + número de Trámite
- Certificado == número de DNI + número de Trámite

Algo ineficiente ¿no?

¿Qué objetivo real cumple el **Certificado Único de Circulación** presentado en papel o en PDF?

¿Por qué no acreditar directamente la autorización de circulación del ciudadano con su DNI físico?

---

Acreditar la autorización tramitada vía **DNI físico** resulta factible haciendo lectura del **código PDF417** impreso en el mismo (no es código QR, es otro tipo de formato).

Dicho código PDF417 contiene **exactamente** la misma información que el código  QR impregnado en el **Certificado Único de Circulación**:

![PDF417](https://i.ibb.co/8DfT56V/12.jpg)

Desde ya que se requerirá un software lector especial para los códigos PDF417 (ningún teléfono móvil lo incorpora)...

Frente a ello no hay problema, pues existen una gran variedad de librerías Open Source para integrar tal funcionalidad a cualquier desarrollo de software, como por ejemplo: https://github.com/PDF417

![GitHub](https://i.ibb.co/yhBny9q/13.jpg)

Y afortunadamente ofrece soporte para implementarlo en el desarrollo de cualquier sistema mobile.

Con dicho recurso se podría elaborar una aplicación mobile **exclusivamente** para las Fuerzas de Seguridad, que funcione como lector de DNI para cotejar si el ciudadano identificado por el mismo está efectivamente exceptuado del aislamiento y habilitado para circular.

Este tipo de solución no es innovadora, pues ya está implementado en múltiples situaciones en donde es requerida la identificación del ciudadano.

Por ejemplo el **REGLAMENTO DE PREVENCIÓN CONTRA LA VIOLENCIA EN ESPECTÁCULOS FUTBOLÍSTICOS** ([Resolución 355/17 del Ministerio de Seguridad](http://servicios.infoleg.gob.ar/infolegInternet/verNorma.do;jsessionid=F555E41A3199DB2528AB3696CF91DFF7?id=274046)) establece que la obligación de mostrar el DNI físico al ingresar al estadio deportivo.

![DEPORTES](https://i.ibb.co/56ncM38/skitch.png)
https://www.argentina.gob.ar/justicia/derechofacil/aplicalaley/voycancha

Al respecto se diseñó el programa [#TRIBUNASEGURA](https://www.argentina.gob.ar/seguridad/tribunasegura) y estableció que cualquier ciudadano que asista a un espectáculo deportivo debe concurrir con su **DNI físico** para constatar con un teléfono 4G y en tiempo real si el ciudadano tiene un pedido de captura y/o aplicado derecho de admisión.

![#TRIBUNASEGURA](https://i.ibb.co/phNS14Z/Pantalla-completa-13-5-20-13-55.jpg)

https://www.argentina.gob.ar/seguridad/tribunasegura

---

Por ende, ante la necesidad y/o excepcionalidad del ciudadano frente al aislamiento social preventivo y obligatorio, de seguirse los mismos lineamiento que en los casos mencionados este nomás tendría que realizar **únicamente** el trámite de autorización y sin tener que descargar ningún PDF ni imprimirlo y/o llevarlo consigo.

Su **DNI físico** sería suficiente.

Nada de códigos QR / URL con números de DNI + números de Trámite expuestos públicamente (cosa que ya ocurrió anteriormente):

[![@Mis2Centavos](https://i.ibb.co/hYDk7q3/Imagen-13-5-20-14-03-pegada.jpg)](https://twitter.com/mis2centavos/status/1139702908180504576)

Pero no, al parecer es más eficiente crear un sistema que masivamente tengan que utilizar **TODOS** los ciudadanos en vez de uno que **SOLO** usarían las Fuerzas de Seguridad, más escalable y extensible desde lo técnico y funcional.

Peor aún resulta la propuesta de unificar el **Certificado Único de Circulación** en la aplicación mobile **CUIDAR**:

[![TELAM](https://i.ibb.co/RQCQmx8/16.jpg)](https://www.telam.com.ar/notas/202005/462219-cafiero-aclaro-que-los-datos-de-la-aplicacion-cuidar-solo-puede-verlos-el-sistema-salud.html)

**NO**, **FALSO**! la aplicación **NO** es la mejor herramienta para llevarlo.

No ofrece nada que el propio DNI físico del ciudadano, que ya lleva consigo por default, no ofrezca para acreditar tanto su identidad como su autorización para circular exceptuado del aislamiento social preventivo y obligatorio. Trasladarle a la gente responsabilidad técnica no es modernización.

---

Respecto a dicha aplicación y más allá de la cuestión sobre la protección de datos personales, geolocalización/rastreo de los ciudadanos, cesión de los datos recolectados, validez empírica de lo que tal app ofrece, etc. vale cuestionarse también la eficiencia desde lo técnico y lo práctico.

### CONCLUSIÓN

El sistema de generación y control del **Certificado Único de Circulación** está mal encarado. Le traslada al ciudadano la carga probatoria de punta a punta, genera procesos redundantes e ineficientes y en medio de ellos huecos de seguridad.

Esto de la "uberización", el solucionismo tecnológico, la falsa automatización y que todo se resuelve inventando una app hace que en realidad no se encuentren soluciones, sino que al final se agreguen complejidades.

Ni mencionar qué encima se escondan intereses oscuros con resultados nefastos.

A la larga la realidad expone que los gurúes tech son más huecos de lo imaginado. Pero si es el Estado el que termina comprando ese solucionismo mágico, a la larga los que perdemos somos todos.

---
### DescuidAR - Certificado Único de Circulación
Autor: **Silvio Messina**
Web: [silviomessina.com](https://silviomessina.com)
Twitter: [twitter.com/smessina_](https://twitter.com/smessina_)
GitHub: [github.com/smessina](https://github.com/smessina)
