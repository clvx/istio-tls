# Istio TLS

Ejemplos de las diferentes configuraciones de Istio TLS basados en la documentación
oficial de [Istio](https://istio.io/latest/docs/).

## Referencias


### Conceptos
https://istio.io/latest/docs/ops/configuration/traffic-management/tls-configuration/
https://istio.io/latest/docs/ops/integrations/certmanager/
https://istio.io/latest/docs/concepts/security/z

### Labs
https://istio.io/latest/docs/tasks/security/cert-management/plugin-ca-cert/ <- TBD but show it.
https://istio.io/latest/docs/tasks/security/authentication/mtls-migration/ - DONE
https://istio.io/latest/docs/tasks/traffic-management/ingress/secure-ingress/ - DONE
https://istio.io/latest/docs/tasks/traffic-management/ingress/ingress-sni-passthrough - DONE 
https://istio.io/latest/docs/tasks/traffic-management/egress/egress-tls-origination/
https://istio.io/latest/docs/tasks/traffic-management/egress/egress-gateway/
https://istio.io/latest/docs/tasks/traffic-management/egress/egress-gateway-tls-origination/
https://istio.io/latest/docs/tasks/security/authentication/authn-policy/

### Problemas de TLS comunes
https://istio.io/latest/docs/ops/common-problems/network-issues/#503-errors-after-setting-destination-rule
https://istio.io/latest/docs/ops/common-problems/network-issues/#tls-configuration-mistakes
https://istio.io/latest/docs/ops/common-problems/network-issues/#gateway-mismatch
https://istio.io/latest/docs/ops/common-problems/network-issues/#gateway-with-tls-passthrough
https://istio.io/latest/docs/ops/common-problems/network-issues/#double-tls
https://istio.io/latest/docs/ops/common-problems/network-issues/#404-errors-occur-when-multiple-gateways-configured-with-same-tls-certificate
https://istio.io/latest/docs/ops/common-problems/network-issues/#configuring-sni-routing-when-not-sending-sni
https://istio.io/latest/docs/ops/common-problems/security-issues/#mutual-tls-errors

### Mejores prácticas
https://istio.io/latest/docs/ops/best-practices/security/#mutual-tls
https://istio.io/latest/docs/ops/best-practices/security/#configure-tls-verification-in-destination-rule-when-using-tls-origination
https://istio.io/latest/docs/ops/best-practices/security/#explicitly-disable-all-the-sensitive-http-host-under-relaxed-sni-host-matching
https://istio.io/latest/docs/ops/best-practices/security/#protocol-detection

