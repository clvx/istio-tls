create_ns:
	kubectl create ns foo
	kubectl label namespace foo istio-injection=enabled --overwrite
	kubectl create ns bar
	kubectl label namespace bar istio-injection=enabled --overwrite
	kubectl create ns legacy

deploy:
	kubectl apply -f ../samples/httpbin.yaml -n foo
	kubectl apply -f ../samples/sleep.yaml -n foo
	kubectl apply -f ../samples/httpbin.yaml -n bar
	kubectl apply -f ../samples/sleep.yaml -n bar
	kubectl apply -f ../samples/httpbin.yaml -n legacy
	kubectl apply -f ../samples/sleep.yaml -n legacy

test:
	for from in "foo" "bar" "legacy"; do for to in "foo" "bar" "legacy"; do kubectl exec "$(kubectl get pod -l app=sleep -n ${from} -o jsonpath={.items..metadata.name})" -c sleep -n ${from} -- curl -s "http://httpbin.${to}:8000/ip" -s -o /dev/null -w "sleep.${from} to httpbin.${to}: %{http_code}\n"; done; done

has_mtls:
	kubectl exec "$(kubectl get pod -l app=sleep -n foo -o jsonpath={.items..metadata.name})" -c sleep -n foo -- curl -s http://httpbin.foo:8000/headers -s | grep X-Forwarded-Client-Cert | sed 's/Hash=[a-z0-9]*;/Hash=<redacted>;/'
	kubectl exec "$(kubectl get pod -l app=sleep -n foo -o jsonpath={.items..metadata.name})" -c sleep -n foo -- curl http://httpbin.legacy:8000/headers -s | grep X-Forwarded-Client-Cert

clean:
	#kubectl delete -f ../samples/httpbin.yaml 2> /dev/null
	#kubectl delete -f ../samples/sleep.yaml 2> /dev/null
	kubectl delete ns foo 2> /dev/null
	kubectl delete ns bar 2> /dev/null
	kubectl delete ns legacy 2> /dev/null
