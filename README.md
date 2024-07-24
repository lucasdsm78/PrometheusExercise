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

Normalement, vous devez avoir le même résultat
<img width="899" alt="Capture d’écran 2024-07-24 à 02 07 10" src="https://github.com/user-attachments/assets/96b8fb5e-0b25-4688-b0b4-6443e95485fe">

## 6) Dashboards Grafana

Aller dans Dashboards => New => Import.

Vous pouvez mettre l'ID 6417 pour voir les metrics des pods.

<img width="704" alt="Capture d’écran 2024-07-24 à 02 46 15" src="https://github.com/user-attachments/assets/42406de3-da6d-488c-9e76-e8754622afd2">















