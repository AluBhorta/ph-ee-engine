# testing notes

- [official installation instructions](https://github.com/openMF/mifos-documentation/blob/d0dca451bdf138da09f726bee99712db6dcfff7e/payment-hub-ee/overview/installation-instructions.md)
    
    or alternatively https://mifos.gitbook.io/docs/payment-hub-ee/overview/installation-instructions
    
    NOTE: installation doesn’t work as expected
    
- ❌ using ph-ee-labs
    
    ```bash
    cd ph-ee-env-labs/helm/payment-hub-med
    
    helm repo add ph-ee-engine https://fynarfin.io/images
    
    # note the changes made in Chart.yaml
    helm dep build
    
    helm install phee-med .
    # isn't working...
    ```
    
- ❌ directly installing chart
    
    ```bash
    helm install phee https://fynarfin.io/images/ph-ee-engine-5.0.1-SNAPSHOT/ph-ee-engine-5.0.1-SNAPSHOT.tgz
    ```
    
    error i’m getting now
    
    ```bash
    coalesce.go:220: warning: cannot overwrite table with non table for ph-ee-engine.operations_web.deployment.config (map[])
    coalesce.go:220: warning: cannot overwrite table with non table for ph-ee-engine.operations_web.deployment.config (map[])
    coalesce.go:220: warning: cannot overwrite table with non table for ph-ee-engine.operations_web.deployment.config (map[])
    Error: INSTALLATION FAILED: unable to build kubernetes objects from release manifest: [resource mapping not found for name: "ph-ee-elasticsearch-pdb" namespace: "" from "": no matches for kind "PodDisruptionBudget" in version "policy/v1beta1"
    ensure CRDs are installed first, resource mapping not found for name: "ccms-service-monitor" namespace: "" from "": no matches for kind "ServiceMonitor" in version "monitoring.coreos.com/v1"
    ensure CRDs are installed first]
    ```
    
- ✅ modifying a downloaded chart
    
    following are steps i took:
    
    - download and extract [ph-ee-engine-5.0.1-SNAPSHOT.tgz](https://fynarfin.io/images/ph-ee-engine-5.0.1-SNAPSHOT/ph-ee-engine-5.0.1-SNAPSHOT.tgz)
        
        or another version from [this registry](https://fynarfin.io/images/)
        
        
        ```bash
        curl -O https://fynarfin.io/images/ph-ee-engine-5.0.1-SNAPSHOT/ph-ee-engine-5.0.1-SNAPSHOT.tgz
        tar xvzf ph-ee-engine-5.0.1-SNAPSHOT.tgz
        ```
        
    - install chart
        - resolve initial errors of install
            - change all `PodDisruptionBudget` `apiVersion` from `policy/v1beta1` to `policy/v1`
            - install CRD for service monitor
                
                ```bash
                k apply -f https://raw.githubusercontent.com/prometheus-operator/prometheus-operator/main/example/prometheus-operator-crd/monitoring.coreos.com_servicemonitors.yaml
                ```
                
        
        ```bash
        helm install phee .
        ```
        
    - troubleshoot errors
        - image pull errors from private ecr repo: 419830066942
            
            solution: either build your own images for the needed services, or find already built ones available on public registries.
            
            - following are some users with some public ph-ee images on docker-hub
                - https://hub.docker.com/u/lalithkota
                - https://hub.docker.com/u/shanidkh
                - https://hub.docker.com/u/danishfynarfin
                - https://hub.docker.com/u/tunexio
    - my lastest update
        - I’ve replaced the enabled service images from *`419830066942.dkr.ecr.ap-south-1.amazonaws.com`* to images [this user’s images](https://hub.docker.com/u/lalithkota).
        - the helm chart now gets installed successfully, but all the components don’t run on my environment due to limited resources like ram and storage.
        - i'm adding the minimum setup instructions on [setup.md](./setup.md) and these notes on [notes.md](./notes.md).
        - i can certainly help troubleshoot further issues for this project and help your team operate k8s effectively. let me know what's next in priority.
    - prospective next steps
        - install the chart on a k8s cluster with enough resources ie. cpu, mem, storage.
        - only enable the phee components that are needed in `values.yaml`. set the others to `enabled: false`. this will help reduce the complexity and get the project going faster.
        - (optional)
          - adjust `values.yaml` according to needs
          - try other versions of the chart
          - build and use your own images