# Demo Prometheus + Node Exporter: **Monitoreando una máquina**

## Requisitos

Clonar el repositorio

## Pasos
```bash
tar -xzf node_exporter-0.17.0-rc.0.darwin-amd64.tar
cd node_exporter-0.17.0-rc.0.darwin-amd64
./node_exporter
```

Entrar en el explorador en `localhost:9100`

Click en `metrics`

Aqui se podrán ver métricas de desempeño del computador, en un formato de texto plano poco legible.

Abrir otro terminal en el directorio raíz donde estamos trabajando

```bash
tar -xzf prometheus-2.5.0.darwin-amd64.tar
cd prometheus-2.5.0.darwin-amd64
```

Dentro debemos editar el archivo `prometheus.yml` y reemplazar con las siguientes líneas:
```yml
global:
  scrape_interval: 5s

scrape_configs:
  - job_name: node
    static_configs:
      - targets: ["localhost:9100"]
  - job_name: prometheus
    static_configs:
      - targets: ["localhost:9090"] 
```

Teniendo el archivo listo basta con ingresan en el terminal

```
./prometheus
```
Luego entrar en la url `localhost:9090` y hacer click en `Graphs`

Una vez dentro prodemos probar escribiendo `node_cpu_seconds_total`, lo que nos dará las estadísticas de tiempo de uso de cpu.

Podemos también mostrar el graph de los mismos datos en el tiempo y cambiar de intervalos (1h, 30m, 15m, etc)

Con el comando irate podremos obtener la estadística segundo a segundo dentro de un intervalo de tiempo, 1 minuto en el caso del ejemplo siguiente

```
irate(node_cpu_seconds_total[1m])
```

Finalmente haremos una agregación de los datos sumándose cada una de las categorías de uso (user, idle, etc) de cpu, con el comando 
```
sum without (cpu)(irate(node_cpu_seconds_total[1m]))
```
lo que mostrará un gráfico con colores que representan cada una de estas categorías superpuestas.

Fuente: [Monitoring a Machine with Prometheus: A Brief Introduction](https://www.youtube.com/watch?v=WUkNnY65htQ)
