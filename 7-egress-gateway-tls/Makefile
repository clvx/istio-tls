deploy:
	kubectl apply -f ../samples/sleep.yaml
	for i in $(shell ls *.yaml); do kubectl apply -f $$i; done
get_pod:
	export SOURCE_POD=$(shell kubectl get pod -l app=sleep -o jsonpath={.items..metadata.name})

test:
	kubectl exec "${SOURCE_POD}" -c sleep -- curl -sSL -o /dev/null -D - http://edition.cnn.com/politics

clean:
	kubectl delete -f ../samples/sleep.yaml
	kubectl delete serviceentry cnn
	kubectl delete gateway istio-egressgateway
	kubectl delete virtualservice direct-cnn-through-egress-gateway
	kubectl delete destinationrule egressgateway-for-cnn

logs:
	kubectl logs -l istio=egressgateway -c istio-proxy -n istio-system | tail
