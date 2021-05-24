De acuerdo a la necesidad planteada por el cliente CRAFTECH, quien solicitó un assessment de su actual centro de datos On-Premise para poder estimar costos e infraestructura para migrar todos sus servicios a la nube de amazon, se presenta la siguiente propuesta:

Elementos a usar:

Route53: Se delega el dominio craftech.io a Route53 para centralizar toda la administración de su dominio y  
punteros(NS, MX, A, cname).

Elastic Load Balancer: Para tener alta disponibilidad sobre las peticiones al puntero app.craftech.io y puerto 443, se usará 
    Elastic Load Balancer, que servirá para distribuir el tráfico entre instancias que están en diferentes zonas de disponibilidad. 

Auto Scaling Group: Para hacer un escalado dinámico de instancias bajo ciertos cumplimientos de eventos. La cantidad de instancias 
puede variar de acuerdo al tráfico, uso de cpu y memoria. Se declaran un mínimo de 4 y un máximo de 10.

Imagen de instancia: Se creó una AMI para que el auto scaling haga uso de las mismas. La AMI está creada con Ubuntu Linux 20.01 y los 
complementos para aplicaciones basadas en JS. El tipo de instancia es T2.Micro.

Subnets: Por cada Zona, se crearon dos subredes, una pública y una privada como requisitos de seguridad y no exponer el tier los datos.

Bases de datos:
El cliente manifiesta que sus datos deben estar altamente disponibles y replicados. Según el assessment de RTO y RPO, tolera una 
pérdida de datos no máxima a 10 minutos y un downtime máximo de 5 minutos. Su SLA oscila en el 99.999% de disponibilidad. 
    SQL: La solución del cliente (app), consume datos desde una Base de datos por lo que se desplegará una instancia RDS y con 
    réplica en stand-by en la zona aledaña. 
    NoSQL: La Aplicación del cliente también maneja datos “No relacionales”, es por ello se usara el motor DynamoDB.

Backups: Para resguardo de datos, se creó un plan de backups diario de las instancias RDS y DynamoDB. 

Grupos de seguridad: 
    ELB: Ingress: 443/80
    RDS: sólo peticiones desde las instancias de front y desde la VPN (conexión para los microservicios externos)

El acceso de los microservicios externos,  se realizará mediante AWS VPN site-to-site entre la locación de estos y nuestra infraestructura de AWS.

Monitoreo: Se agrega como elemento de monitoreo a CloudWatch y SNS para envio de mensajes de alertas ante eventos.
