# подготовка
minikube start
minikube addons enable metrics-server

# Запуск
	kubectl apply -f deployment.yaml
	kubectl apply -f service.yaml
	kubectl apply -f hpa.yaml

# Запуск locust 
locust -f locustfile.py --host $(minikube service scaletestapp --url | grep -E '^http' | head -n1) -u 3000 -r 2

# Запуск dashboard
minikube dashb

# Результат

![VideoLOG](https://github.com/ZergZet/Sprint8-InureTech-/blob/main/Task2/Log/HPA.gif)

# привести кластер в исходное состояние
# 1. Остановить Locust, если ещё работает (в его терминале Ctrl+C или)
pkill -f locust

# 2. Удалить HPA
kubectl delete hpa scaletestapp-hpa

# 3. Скейлить Deployment в 0 (быстрее, чем ждать graceful termination)
kubectl scale deployment scaletestapp --replicas=0

# 4. Удалить Deployment и Service
kubectl delete deployment scaletestapp
kubectl delete service scaletestapp

# 5. Проверить, что чисто
kubectl get pods,deployment,service,hpa
