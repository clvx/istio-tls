get_ip:
	export INGRESS_PORT=$(shell kubectl -n istio-system get service istio-ingressgateway -o jsonpath='{.spec.ports[?(@.name=="http2")].nodePort}')
	export SECURE_INGRESS_PORT=$(shell kubectl -n istio-system get service istio-ingressgateway -o jsonpath='{.spec.ports[?(@.name=="https")].nodePort}')
	export TCP_INGRESS_PORT=$(shell kubectl -n istio-system get service istio-ingressgateway -o jsonpath='{.spec.ports[?(@.name=="tcp")].nodePort}')
	export INGRESS_HOST=$(shell kubectl get po -l istio=ingressgateway -n istio-system -o jsonpath='{.items[0].status.hostIP}')

deploy:
	kubectl apply -f ../samples/httpbin.yaml
	kubectl apply -f ./gateway-mtls.yaml
	kubectl apply -f ./virtualserver-mtls.yaml

generate-ca:
	openssl req -x509 -sha256 -nodes -days 365 -newkey rsa:2048 -subj '/O=example Inc./CN=example.com' -keyout example.com.key -out example.com.crt

generate-cert:
	openssl req -out httpbin.example.com.csr -newkey rsa:2048 -nodes -keyout httpbin.example.com.key -subj "/CN=httpbin.example.com/O=httpbin organization"
	openssl x509 -req -sha256 -days 365 -CA example.com.crt -CAkey example.com.key -set_serial 0 -in httpbin.example.com.csr -out httpbin.example.com.crt

generate-client:
	openssl req -out client.example.com.csr -newkey rsa:2048 -nodes -keyout client.example.com.key -subj "/CN=client.example.com/O=client organization"
	openssl x509 -req -sha256 -days 365 -CA example.com.crt -CAkey example.com.key -set_serial 1 -in client.example.com.csr -out client.example.com.crt

create-secret:
	kubectl create -n istio-system secret generic httpbin-credential --from-file=tls.key=httpbin.example.com.key \
	--from-file=tls.crt=httpbin.example.com.crt --from-file=ca.crt=example.com.crt

test:
	curl -v -HHost:httpbin.example.com --resolve "httpbin.example.com:${SECURE_INGRESS_PORT}:${INGRESS_HOST}" \
	--cacert example.com.crt --cert client.example.com.crt --key client.example.com.key \
	"https://httpbin.example.com:${SECURE_INGRESS_PORT}/status/418"

clean:
	kubectl delete -f ./gateway-mtls.yaml
	kubectl delete -f ./virtualserver-mtls.yaml
	kubectl delete --ignore-not-found=true -f ../samples/httpbin.yaml
	rm *.csr *.crt *.key 2> /dev/null

