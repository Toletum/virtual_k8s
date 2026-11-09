# virtual_k8s

Guía y configuración para crear un clúster de Kubernetes de desarrollo local con kind sobre Podman.

## Visibilidad y rama

- Público. Rama por defecto: `main`.

## Stack

- kind v0.31.0 (según README), imagen de nodo `kindest/node:v1.31.0`, `kubectl`, Podman como proveedor (`KIND_EXPERIMENTAL_PROVIDER=podman`).
- metrics-server (manifiesto de la última release, desde el README).

## Estructura

- `README.md`: guía de binarios, creación, comprobación de nodos, ejemplo nginx, métricas, arrancar, parar y destruir el clúster.
- `config.yaml`: configuración de kind (1 control-plane y 3 workers, `apiVersion: kind.x-k8s.io/v1alpha4`).

## Comandos

Del README (no ejecutados):

- Crear clúster: `KIND_EXPERIMENTAL_PROVIDER=podman kind create cluster --name k8s-dev --image kindest/node:v1.31.0 --config <ruta>/config.yaml`.
- Comprobar nodos: `kubectl get nodes`.
- Ejemplo: `kubectl create deployment nginx-hola --image=nginxdemos/hello`, `kubectl expose ...`, `kubectl port-forward svc/nginx-hola 8080:80`.
- Métricas: `kubectl apply -f https://github.com/kubernetes-sigs/metrics-server/releases/latest/download/components.yaml`.
- Arrancar y parar: `podman start|stop $(podman ps ... --filter name=k8s-dev)`.
- Destruir (destructivo, pedir confirmación): `kind delete cluster --name k8s-dev`.

## Configuración

- `config.yaml` define el número de nodos. Sin variables de entorno ni secretos.
- Nombre del clúster: `k8s-dev` (kubectl usa el contexto `kind-k8s-dev`).

## Convenciones para agentes

- No instalar `kind` ni `kubectl` en el sistema; si faltan, pedir permiso (regla del entorno).
- Usar Podman, no Docker, para los contenedores.
- Antes de borrar el clúster o parar contenedores, pedir confirmación.

## Estado

- Sin tests ni CI.
- Inconsistencia: el README usa `--config k8s-dev/config.yaml`, pero el fichero está en la raíz (`config.yaml`); el comando falla tal cual si no existe ese directorio.
- Las instrucciones descargan binarios con `curl` y el metrics-server se aplica desde `latest`: versiones no fijadas.
- Sin `.gitignore`.
- La salida de `kubectl get nodes` del README es un ejemplo, no un resultado verificado.
