# Instalación del Cluster

## Instalación de Docker (Ubuntu 20.04)

```bash
# Actualizando repositorios
sudo apt update

# Instalando dependencias
sudo apt install apt-transport-https ca-certificates curl software-properties-common

# Agregando clave de repositorio docker
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -

# Agregando repositorio docker
sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu focal stable"

# Revisando versión candidata
apt-cache policy docker-ce

# Instalando docker-ce
sudo apt install docker-ce

# Agregando usuario al grupo docker
sudo usermod -aG docker ${USER}

# Cargando en la sesión la pertenencia al grupo docker
su - ${USER}

```


## Instalación de un cluster de Kubernete local

Fuente: [https://kind.sigs.k8s.io](https://kind.sigs.k8s.io)

```bash
# Creación de una carpeta de trabajo
mkdir workspace 
cd workspace    

# Descarga de la herramienta kind
curl -Lo ./kind https://kind.sigs.k8s.io/dl/v0.11.1/kind-linux-amd64 

# Dando permiso de ejecución a kind
chmod +x ./kind  

# Moviendo kind a una carpeta de binarios
sudo mv kind /usr/local/bin/  

# Ejecutando el comando de ayuda de kind
kind --help 
```


```bash
#Creación de archivo de configuración del cluster de kubernete
mkdir config
nano config/kind-config.yml
```

__Pegar el siguiente contenido:__

```yml
kind: Cluster
apiVersion: kind.x-k8s.io/v1alpha4
name: kind-cluster-101
networking:
  ipFamily: ipv4
  apiServerAddress: "0.0.0.0"
  apiServerPort: 6443
  podSubnet: "10.244.0.0/16"
  serviceSubnet: "10.96.0.0/12"
  kubeProxyMode: "iptables"
nodes:
- role: control-plane
```

__Cerrar el editor de texto presionando__

- tecla control + o, seguido de enter para guardar
- tecla control + x para salir

__Creación del cluster__

```bash
# Creación del cluster
kind create cluster --config config/kind-config.yml
```

# Instalación de herramienta KUBECTL

```bash
# Descargando binario de kubectl
curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"

# Instalando kubectl
sudo install -o root -g root -m 0755 kubectl /usr/local/bin/kubectl

# Obteniendo información del los nodos del cluster local
kubectl get nodes

# Obteniendo un listado del los namespaces del cluster local
kubectl get namespaces

```
