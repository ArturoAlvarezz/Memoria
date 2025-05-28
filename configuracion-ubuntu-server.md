# Instrucciones para configurar maquinas de ubuntu server en Gns3

## Configurar interfaz de red

'''bash
sudo nano /etc/netplan/50-cloud-init.yaml
'''

```yaml
network:
  version: 2
  ethernets:
    ens3:
      dhcp4: true
      dhcp-identifier: mac
  version: 2
```

```bash
sudo netplan apply
sudo ip link set ens3 up
```

## Cambiar nombre del host

```bash
sudo hostnamectl set-hostname nodo6
sudo nano /etc/hosts
```

Cambiar ubuntu2404 por nodo*

## Instalar docker

```bash
for pkg in docker.io docker-doc docker-compose docker-compose-v2 podman-docker containerd runc; do sudo apt-get remove $pkg; done

# Add Docker's official GPG key:
sudo apt-get update
sudo apt-get install ca-certificates curl
sudo install -m 0755 -d /etc/apt/keyrings
sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc
sudo chmod a+r /etc/apt/keyrings/docker.asc

# Add the repository to Apt sources:
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/ubuntu \
  $(. /etc/os-release && echo "${UBUNTU_CODENAME:-$VERSION_CODENAME}") stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt-get update

sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin

sudo docker run hello-world

sudo usermod -aG docker $USER
```

## Instalar K3s(worker)

```bash
curl -sfL https://get.k3s.io | K3S_URL=https://192.168.122.30:6443 K3S_TOKEN=K107eb9cde0fa0994823ec59df34e58ca6c107e5d0d312b8a08e05e5647c4d6ab3f::server:b9d7d1b5759b7c4dec08eb44000e212e sh -
```

## Instalar Go

```bash
sudo snap install go --classic 

go version
```

## Instalar podman

```bash
sudo apt install podman -y
podman -v
```

## Instalar helm

```bash
curl -fsSL -o get_helm.sh https://raw.githubusercontent.com/helm/helm/main/scripts/get-helm-3
chmod 700 get_helm.sh
./get_helm.sh
export KUBECONFIG=/etc/rancher/k3s/k3s.yaml
kubectl get pods --all-namespaces
helm ls --all-namespaces