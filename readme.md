# Practica Iot Stack
Omar Zamorano Garcia #22211676

1. **Captura de pantalla del panel de administración de Tailscale mostrando el nodo dashboard-server.**

   ![Tailscale](https://github.com/OmarZamorano/Omar-Zamorano-Garcia-Iot-Stack/blob/main/imagenes/Captura_tailscale.PNG)

2. **Dashboard de Grafana donde se visualicen los datos de InfluxDB y las métricas de Prometheus.**

   Grafana
   
   ![Grafana](https://github.com/OmarZamorano/Omar-Zamorano-Garcia-Iot-Stack/blob/main/imagenes/Captura_grafana.PNG)

   Prometheus
   
   ![Prometheus](https://github.com/OmarZamorano/Omar-Zamorano-Garcia-Iot-Stack/blob/main/imagenes/Captura_prometheus1.PNG)

   ![Prometheus](https://github.com/OmarZamorano/Omar-Zamorano-Garcia-Iot-Stack/blob/main/imagenes/Captura_prometheus2.PNG)

3. **Código Python utilizado para la simulación de datos.**

```
from datetime import datetime
import random
import time
from influxdb_client import InfluxDBClient, Point, WritePrecision

token = "93Z-civ4vryFmfm1eeKzk5EoaVOC1Z-WUwGwGmHmXo28ijd4q6q-Myqs1KmzFedSzdER8WbD1WiA37tWNAd_Sg=="
org = "iot-lab"
bucket = "sensores"

client = InfluxDBClient(url="http://localhost:8086", token=token, org=org)
write_api = client.write_api()

while True:
    temp = random.uniform(18, 30)
    hum = random.uniform(40, 70)
    point = (
        Point("ambiente")
        .tag("ubicacion", "laboratorio")
        .field("temperatura", temp)
        .field("humedad", hum)
        .time(datetime.utcnow(), WritePrecision.NS)
    )
    write_api.write(bucket=bucket, record=point)
    print(f"Enviado: temp={temp:.2f}, hum={hum:.2f}")
    time.sleep(5)
```

4. **Explicación escrita que conteste:**
   - **¿Por qué es útil Tailscale en este escenario? (responde que permite conectar nodos mediante VPN sin exponer puertos públicos).**

        Tailscale sirve para conectar nodos en un red virtual privada (VPN) sin necesidad de abrir puertos públicos ya que con tailscale manda a un red virtual solo para dichos dispositivos con el fin de que puedan transferir datos entre sí, es como si todos los dispositivos estuvieran en una misma red local pero con una VPN.
     
   - **Diferencia entre las métricas de IoT (InfluxDB) y las métricas de sistema (Prometheus/Node Exporter).**

       Con InfluxDB observamos datos que vienen directo de algun dispositivo, en este caso de sensores. Por otro lado, con Prometheus observamos métricas directas del sistema operativo como rendimiento del CPU, memoria, alertas de la máquina, etc.
