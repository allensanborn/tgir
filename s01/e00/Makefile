include ../../Makefile

create-kindDefaultCluster: $(KIND) ##Create Default Kind Cluster
	kind create cluster
delete-kindDefaultCluster: $(KIND) ##Create Default Kind Cluster
	kind delete cluster --name=kind

create-kindIngressCluster: $(KUBECTL) $(KIND) ##Create Kind K8s cluster using Ingress
	kind create cluster --config=./cluster-ingress.yaml --name=ingress-demo
	# Display current context
	kubectl config current-context
	# https://kind.sigs.k8s.io/docs/user/ingress#ingress-nginx
	kubectl config use-context kind-ingress-demo
apply-ingressNginx: create-kindIngressCluster
	kubectl config use-context kind-ingress-demo
	kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/master/deploy/static/provider/kind/deploy.yaml
install-ingressEchoService: apply-ingressNginx ##Create cluster, apply ingress, install echo service
	kubectl config use-context kind-ingress-demo
	kubectl get all
	kubectl wait --namespace ingress-nginx --for=condition=ready pod --selector=app.kubernetes.io/component=controller --timeout=90s
	kubectl apply -f https://kind.sigs.k8s.io/examples/ingress/usage.yaml
	curl localhost/foo # should output "foo"
	curl localhost/bar # should output "bar"
	
delete-kindIngressCluster: $(KIND) ##Delete Kind K8s cluster using Ingress
	kind delete cluster --name=ingress-demo

kubectlVersion: $(KUBECTL) ##three
	kubectl version
.PHONY: clean ##clean
clean:
	rm -f one two three
