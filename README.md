# PrometheusExercise
A basic project to learn Prometheus and Grafana

# Installation

## Pré requis

- Il faut avoir un cluster Kubernetes. Vous pouvez utiliser Minikube ou un cluster cloud comme EKS, AKS ou GKE.

- Kubectl pour les commandes Kubernetes

## 1) Installer Helm

Suivez les instructions de ce lien https://helm.sh/docs/intro/install/ pour installer Helm

## 2) Installer Prometheus, Grafana et AlertManager via les packages Helm

```
helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
helm repo update
helm install prometheus prometheus-community/kube-prometheus-stack
```

Vérifie les installations en listant les pods :
```
kubectl get pods -n
```

Vous devez voir les pods Prometheus, Grafana et AlertManager en cours d'exécution :

<img width="710" alt="Capture d’écran 2024-07-24 à 02 32 08" src="https://github.com/user-attachments/assets/174bacfb-4bae-4a33-a1d4-6d6f7e135bab">

## 3) Grafana

### Obtenir le mot de passe pour se connecter à Grafana

```
kubectl get secret --namespace default prometheus-grafana -o jsonpath="{.data.admin-password}" | base64 --decode ; echo
```
### Faire un port-forward vers Grafana

```
kubectl port-forward svc/prometheus-grafana 3000:80
```

Un port forward permet d'accéder à des services internes d'un cluster Kubernetes depuis votre machine locale. Cela vous permet de rediriger un port local vers un port sur un pod ou un service dans le cluster.

Lance ```http://localhost:3000```

Vous atterrissez sur une page comme celle-ci 

 <img width="1718" alt="Capture d’écran 2024-07-24 à 02 04 27" src="https://github.com/user-attachments/assets/c2da387d-fcea-4ea4-b2b8-2666de00b820">

Mettre ```admin``` comme sur la capture pour l'identifiant

## 4) Cloner le projet

```
git clone https://github.com/lucasdsm78/PrometheusExercise
```

## 5) Déployer l'application, le service et le ServiceMonitor

Exemple pour le déploiement

```
kubectl apply -f deployment.yaml
```

## 6) Dashboards Grafana

Aller dans ```Dashboards``` => ```New``` => ```Import```.

Vous pouvez mettre l'ID 6417 pour voir les metrics des pods.

<img width="704" alt="Capture d’écran 2024-07-24 à 02 46 15" src="https://github.com/user-attachments/assets/42406de3-da6d-488c-9e76-e8754622afd2">

Avec l'ID 1860 pour l'état des noeuds Kubernetes avec le CPU, la mémoire et les fichiers systèmes :

<img width="1407" alt="Capture d’écran 2024-07-24 à 02 57 32" src="https://github.com/user-attachments/assets/6ab37d0f-8fec-4914-bddb-08891b738786">

Avec l'ID 315 pour les metrics sur l'utilisation des ressources pour chaque pod :

<img width="1381" alt="Capture d’écran 2024-07-24 à 02 55 07" src="https://github.com/user-attachments/assets/49d44446-d01d-43d0-9c3c-99e9ff727036">



# Troubleshoots

Si vous avez cette erreur pour l'installation de Prometheus, Grafana et AlertManager : 

```
Error: INSTALLATION FAILED: Kubernetes cluster unreachable: Get "https://127.0.0.1:59845/version": dial tcp 127.0.0.1:59845: connect: connection refused
```

C'est qu'il n'y a aucun cluster Kubernetes de lancer. Il faut lancer un cluster Kubernetes.
Pour Minikube, c'est ```minikube start```



















