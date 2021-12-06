#########################
# Defining applications #
#########################

# Helm
cat helm/templates/ingress.yaml

# Kustomize
cat kustomize/base/ingress.yaml

# Helm
cat helm/values.yaml

# Kustomize
cat kustomize/base/kustomization.yaml

# Helm
helm upgrade --install \
    devops-toolkit helm \
    --namespace helm-dev \
    --create-namespace

# Kustomize
kubectl create namespace kustomize-dev

# Kustomize
kustomize build \
    kustomize/base \
    | kubectl --namespace kustomize-dev \
    apply --filename -

#############################################
# Defining environment-specific application #
#############################################

# Helm
cat helm/values-production.yaml

# Kustomize
cat kustomize/overlays/production/kustomization.yaml

# Kustomize
cat kustomize/overlays/production/ingress-patch.yaml

# Helm
helm upgrade --install \
    devops-toolkit helm \
    --namespace helm-production \
    --create-namespace \
    --values helm/values-production.yaml

# Kustomize
kustomize build \
    kustomize/overlays/production \
    | kubectl apply --filename -

#####################################
# Applying unplanned customizations #
#####################################

# Helm
vim helm/values.yaml

# Helm
cat snippets/deployment-resources.yaml

# Helm
vim helm/templates/deployment.yaml

# Kustomize
cat kustomize/overlays/production/deployment-patch.yaml

# Kustomize
vim kustomize/overlays/production/kustomization.yaml

# Helm
vim helm/values-production.yaml
