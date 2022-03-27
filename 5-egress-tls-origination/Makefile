deploy:
	kubectl apply -f ../samples/sleep.yaml
	kubectl apply -f ./serviceentry.yaml
	kubectl apply -f ./destinationrule.yaml
get_pod:
	export SOURCE_POD=$(shell kubectl get pod -l app=sleep -o jsonpath={.items..metadata.name})

test:
	kubectl exec "${SOURCE_POD}" -c sleep -- curl -sSL -o /dev/null -D - http://edition.cnn.com/politics

clean:
	kubectl delete serviceentry edition-cnn-com
	kubectl delete destinationrule edition-cnn-com
	kubectl delete -f ../samples/sleep.yaml
