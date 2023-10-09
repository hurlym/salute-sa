# SALUTE S.A.

Salute S.A es una empresa de medicina prepaga y quiere desarrollar cotizador web para nuevos clientes.

## SITUACION ACTUAL

Actualmente la empresa utiliza el sistema de VENTAS Y FACTURACION para cotizar. Cuando un nuevo cliente llama se lo comunica con un vendedor, el mismo carga una venta que luego no se graba o se anula inmediatamente.
Esta “COTIZACION” o “VENTA FANTASMA” se alimenta de la lista de precios actual y los parámetros de cálculo que determinan los valores como rango de edad, enfermedades preexistentes, etc . Estas tablas son utilizadas por el sistema para calcular y emitir la facturación periódica de los afiliados.
El sistema de VENTAS Y FACTURACION son dos FRONTEND-SPA independientes que comparten un mismo backend (que es una REST-API).
Los vendedores acceden al FRONTEND de VENTAS. Donde se ingresan nuevos clientes y cada vendedor hace un seguimiento de sus comisiones.
El área de administración accede al sistema de FACTURACION que se encarga de emitir en forma automática la facturación mensual, la administración de la lista de precios, parámetros y método de cálculo de descuentos y otros conceptos.
El sistema de FACTURACION Y VENTAS se encuentra diseñado para intranet.

- NO implementa HTTPS
- La autenticación se realiza con HTTP BASIC con un método interno, una contrala propia del sistema y utilizando el email corporativo como nombre de usuario

DATOS EXTRA:

- Actualmente la empresa cuenta con Office 365 como sistema de mail, servicio de mensajería empresarial entre otros. Incluido con este producto se encuentra el servicio **Azure Active Directory que les provee interfaces OIDC y SAML.**
- La normativa de la Super Intendencia de Servicios de Salud exige que los aumentos sean anunciados con 30 días de anticipación. Es por este motivo que las cotizaciones utilizan el ultimo valor con aumento y tienen una validez de 30 días.

- ![Diagrama estructural](./diagrama.drawio.svg)

## REQUERIMIENTO

Se debe desarrollar un sistema cotizados e integrarlo con el / los sistemas de FACTURACION Y VENTAS.

El ingreso a la web del cotizador estará en un LINK en la página principal de la empresa. Al hacer click el usuario es redirigido al cotizador. De esta forma no es necesario que el cotizado tenga SEO.

El usuario podrá ingresar al cotizador con un usuario nuevo (propio del cotizador), su cuenta de Gmail, Facebook u Outlook/ Live. LA registración es necesaria por dos motivos;

- Evitar BOTS o acceso de otras prepagas para espiar su lista de precios y métodos de cálculo.
- Poder hacer un seguimiento de las cotizaciones

Una vez autenticado, el cliente, ingresara los datos necesarios para calcular la cotización (Tipo de plan, grupo familiar, zona de residencia, etc.). Estos datos deben ser persistidos, dado que tiene calidad de declaración jurada.

Con los datos ingresados por el usuario el sistema debe utilizar la lista de precios actuales de la empresa, parámetros y método de cálculo utilizado sistema de FACTURACION Y VENTAS.

Una vez calculada la cotización, se envía una copia por mail al cliente y se asigna automáticamente, por un algoritmo similar a un roud-robin, a un vendedor encargado de hacer el seguimiento de dicha cotización.

Los vendedores deberán ingresar al sistema cotizador para hacer un seguimiento de las cotizaciones y en caso de prosperar cargar la venta. En una primera etapa el sector de ventas está dispuesto a volver a cargar los datos del cliente en el sistema de FACTURACION Y VENTAS. Pero puede proponer una opción alternativa.

## PROPUESTA

- Realice un diagrama estructural sistema cotizador y las adecuaciones que crea necesarias al sistema de FACTURACION Y VENTAS
- Tenga en cuenta las ventajas y desventajas de estos cambios
- RECUERDE
  o    EVITAR Referencias circulares
  o    La función ESFUERZO = FUERZA DE ACOPLAMIENTO x VOLATILIDAD x DISTANCIA
  o    La gran mayoría de Identity providers (Keycloak, Okta, Auth0, etc.) tiene la opción de Identidades Federadas, es decir confiar en credenciales emitidas por terceras partes que implementen el protocolo OIDC