# Azure Monitor baseline
This is my baseline Azure Monitor configuration for PoCs 

# Local deployment
az group create -n monitoring -l westeurope
az group deployment create -g monitoring --template-file deploy.json

# Linux agents install
wget https://raw.githubusercontent.com/Microsoft/OMS-Agent-for-Linux/master/installer/scripts/onboard_agent.sh && sh onboard_agent.sh -w worksspaceid -s workspacekey -d opinsights.azure.com

wget --content-disposition https://aka.ms/dependencyagentlinux -O InstallDependencyAgent-Linux64.bin
sudo sh InstallDependencyAgent-Linux64.bin -s

# Kubernetes agents install
helm repo add incubator https://kubernetes-charts-incubator.storage.googleapis.com/
helm upgrade -i azuremonitor incubator/azuremonitor-containers \
    --set omsagent.secret.wsid=<your_workspace_id> \
    --set omsagent.secret.key=<your_workspace_key> \
    --set omsagent.env.clusterName=<my_prod_cluster> 
