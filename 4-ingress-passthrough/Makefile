get_ip:
	export INGRESS_PORT=$(shell kubectl -n istio-system get service istio-ingressgateway -o jsonpath='{.spec.ports[?(@.name=="http2")].nodePort}')
	export SECURE_INGRESS_PORT=$(shell kubectl -n istio-system get service istio-ingressgateway -o jsonpath='{.spec.ports[?(@.name=="https")].nodePort}')
	export TCP_INGRESS_PORT=$(shell kubectl -n istio-system get service istio-ingressgateway -o jsonpath='{.spec.ports[?(@.name=="tcp")].nodePort}')
	export INGRESS_HOST=$(shell kubectl get po -l istio=ingressgateway -n istio-system -o jsonpath='{.items[0].status.hostIP}')

generate-ca:
	openssl req -x509 -sha256 -nodes -days 365 -newkey rsa:2048 -subj '/O=example Inc./CN=example.com' -keyout example.com.key -out example.com.crt

generate-cert:
	openssl req -out nginx.example.com.csr -newkey rsa:2048 -nodes -keyout nginx.example.com.key -subj "/CN=nginx.example.com/O=some organization"
	openssl x509 -req -sha256 -days 365 -CA example.com.crt -CAkey example.com.key -set_serial 0 -in nginx.example.com.csr -out nginx.example.com.crt

nginx-key:
	kubectl create secret tls nginx-server-certs --key nginx.example.com.key --cert nginx.example.com.crt

nginx-configfile:
	kubectl create configmap nginx-configmap --from-file=nginx.conf=./nginx.conf

deploy:
	kubectl apply -f ./nginx.yaml
	kubectl apply -f ./gateway.yaml
	kubectl apply -f ./virtualservice.yaml

test:
	curl -v --resolve "nginx.example.com:${SECURE_INGRESS_PORT}:${INGRESS_HOST}" --cacert example.com.crt "https://nginx.example.com:${SECURE_INGRESS_PORT}"

clean:
	kubectl delete secret nginx-server-certs
	kubectl delete configmap nginx-configmap
	kubectl delete service my-nginx
	kubectl delete deployment my-nginx
	kubectl delete gateway mygateway
	kubectl delete virtualservice nginx
	rm example.com.crt example.com.key nginx.example.com.crt nginx.example.com.key nginx.example.com.csr
