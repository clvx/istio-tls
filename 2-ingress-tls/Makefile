get_ip:
	export INGRESS_PORT=$(shell kubectl -n istio-system get service istio-ingressgateway -o jsonpath='{.spec.ports[?(@.name=="http2")].nodePort}')
	export SECURE_INGRESS_PORT=$(shell kubectl -n istio-system get service istio-ingressgateway -o jsonpath='{.spec.ports[?(@.name=="https")].nodePort}')
	export TCP_INGRESS_PORT=$(shell kubectl -n istio-system get service istio-ingressgateway -o jsonpath='{.spec.ports[?(@.name=="tcp")].nodePort}')
	export INGRESS_HOST=$(shell kubectl get po -l istio=ingressgateway -n istio-system -o jsonpath='{.items[0].status.hostIP}')

deploy:
	kubectl apply -f ../samples/httpbin.yaml
	kubectl apply -f gateway-tls.yaml
	kubectl apply -f virtualservice-tls.yaml

create-secret:
	kubectl create -n istio-system secret tls httpbin-credential --key=httpbin.example.com.key --cert=httpbin.example.com.crt

generate-ca:
	openssl req -x509 -sha256 -nodes -days 365 -newkey rsa:2048 -subj '/O=example Inc./CN=example.com' -keyout example.com.key -out example.com.crt

generate-cert:
	openssl req -out httpbin.example.com.csr -newkey rsa:2048 -nodes -keyout httpbin.example.com.key -subj "/CN=httpbin.example.com/O=httpbin organization"
	openssl x509 -req -sha256 -days 365 -CA example.com.crt -CAkey example.com.key -set_serial 0 -in httpbin.example.com.csr -out httpbin.example.com.crt

test:
	curl -v -HHost:httpbin.example.com --resolve "httpbin.example.com:${SECURE_INGRESS_PORT}:${INGRESS_HOST}" --cacert example.com.crt "https://httpbin.example.com:${SECURE_INGRESS_PORT}/status/418"

clean:
	kubectl -n istio-system delete secrets httpbin-credential 
	kubectl delete -f gateway-tls.yaml
	kubectl delete  -f virtualservice-tls.yaml
	kubectl delete --ignore-not-found=true -f ../samples/httpbin.yaml
	rm *.csr *.crt *.key

